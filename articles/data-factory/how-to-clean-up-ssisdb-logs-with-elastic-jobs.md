---
title: Czyszczenie dzienników SSISDB za pomocą zadań Elastic Database platformy Azure
description: W tym artykule opisano sposób oczyszczania dzienników SSISDB przy użyciu usługi Azure zadania Elastic Database do wyzwolenia procedury składowanej, która istnieje w tym celu
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/09/2020
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 3539b867d8f03d11e7799498d0207a65ac9db7d8
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2020
ms.locfileid: "92636633"
---
# <a name="clean-up-ssisdb-logs-with-azure-elastic-database-jobs"></a>Czyszczenie dzienników SSISDB za pomocą zadań Elastic Database platformy Azure

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

W tym artykule opisano, jak używać zadań Elastic Database platformy Azure do wyzwalania procedury składowanej, która czyści dzienniki dla bazy danych wykazu SQL Server Integration Services, `SSISDB` .

Zadania Elastic Database to usługa platformy Azure, która ułatwia Automatyzowanie i uruchamianie zadań w odniesieniu do bazy danych lub grupy baz danych. Możesz planować, uruchamiać i monitorować te zadania za pomocą interfejsów API Azure Portal, Transact-SQL, PowerShell lub REST. Użyj zadania Elastic Database, aby wyzwolić procedurę składowaną służącą do czyszczenia dzienników jednokrotnie lub zgodnie z harmonogramem. Możesz wybrać interwał harmonogramu oparty na SSISDB zasobów, aby uniknąć dużego obciążenia bazy danych.

Aby uzyskać więcej informacji, zobacz [Zarządzanie grupami baz danych za pomocą zadań Elastic Database](../azure-sql/database/elastic-jobs-overview.md).

W poniższych sekcjach opisano sposób wyzwalania procedury składowanej `[internal].[cleanup_server_retention_window_exclusive]` , która usuwa dzienniki SSISDB poza oknem przechowywania ustawionym przez administratora.

## <a name="clean-up-logs-with-power-shell"></a>Czyszczenie dzienników przy użyciu powłoki PowerShell

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

Następujące przykładowe skrypty programu PowerShell tworzą nowe zadanie elastyczne, aby wyzwolić procedurę składowaną dla czyszczenia dziennika SSISDB. Aby uzyskać więcej informacji, zobacz [Tworzenie elastycznego agenta zadań przy użyciu programu PowerShell](../azure-sql/database/elastic-jobs-powershell-create.md).

### <a name="create-parameters"></a>Tworzenie parametrów

``` powershell
# Parameters needed to create the Job Database
param(
$ResourceGroupName = $(Read-Host "Please enter an existing resource group name"),
$AgentServerName = $(Read-Host "Please enter the name of an existing logical SQL server(for example, yhxserver) to hold the SSISDBLogCleanup job database"),
$SSISDBLogCleanupJobDB = $(Read-Host "Please enter a name for the Job Database to be created in the given SQL Server"),
# The Job Database should be a clean,empty,S0 or higher service tier. We set S0 as default.
$PricingTier = "S0",

# Parameters needed to create the Elastic Job agent
$SSISDBLogCleanupAgentName = $(Read-Host "Please enter a name for your new Elastic Job agent"),

# Parameters needed to create the job credential in the Job Database to connect to SSISDB
$PasswordForSSISDBCleanupUser = $(Read-Host "Please provide a new password for SSISDBLogCleanup job user to connect to SSISDB database for log cleanup"),
# Parameters needed to create a login and a user in the SSISDB of the target server
$SSISDBServerEndpoint = $(Read-Host "Please enter the name of the target logical SQL server which contains SSISDB you need to cleanup, for example, myserver") + '.database.windows.net',
$SSISDBServerAdminUserName = $(Read-Host "Please enter the target server admin username for SQL authentication"),
$SSISDBServerAdminPassword = $(Read-Host "Please enter the target server admin password for SQL authentication"),
$SSISDBName = "SSISDB",

# Parameters needed to set job scheduling to trigger execution of cleanup stored procedure
$RunJobOrNot = $(Read-Host "Please indicate whether you want to run the job to cleanup SSISDB logs outside the log retention window immediately(Y/N). Make sure the retention window is set appropriately before running the following powershell scripts. Those removed SSISDB logs cannot be recoverd"),
$IntervalType = $(Read-Host "Please enter the interval type for the execution schedule of SSISDB log cleanup stored procedure. For the interval type, Year, Month, Day, Hour, Minute, Second can be supported."),
$IntervalCount = $(Read-Host "Please enter the detailed interval value in the given interval type for the execution schedule of SSISDB log cleanup stored procedure"),
# StartTime of the execution schedule is set as the current time as default. 
$StartTime = (Get-Date)
```

### <a name="trigger-the-cleanup-stored-procedure"></a>Wyzwalanie procedury składowanej oczyszczania

```powershell
# Install the latest PackageManagement powershell package which PowershellGet v1.6.5 is dependent on
Find-Package PackageManagement -RequiredVersion 1.1.7.2 | Install-Package -Force
# You may need to restart the powershell session
# Install the latest PowershellGet module which adds the -AllowPrerelease flag to Install-Module
Find-Package PowerShellGet -RequiredVersion 1.6.5 | Install-Package -Force

# Place AzureRM.Sql preview cmdlets side by side with existing AzureRM.Sql version
Install-Module -Name AzureRM.Sql -AllowPrerelease -Force

# Sign in to your Azure account
Connect-AzureRmAccount

# Create a Job Database which is used for defining jobs of triggering SSISDB log cleanup stored procedure and tracking cleanup history of jobs
Write-Output "Creating a blank SQL database to be used as the SSISDBLogCleanup  Job Database ..."
$JobDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $AgentServerName -DatabaseName $SSISDBLogCleanupJobDB -RequestedServiceObjectiveName $PricingTier
$JobDatabase

# Enable the Elastic Jobs preview in your Azure subscription
Register-AzureRmProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql

# Create the Elastic Job agent
Write-Output "Creating the Elastic Job agent..."
$JobAgent = $JobDatabase | New-AzureRmSqlElasticJobAgent -Name $SSISDBLogCleanupAgentName
$JobAgent

# Create the job credential in the Job Database to connect to SSISDB database in the target server for log cleanup
Write-Output "Creating job credential to connect to SSISDB database..."
$JobCredSecure = ConvertTo-SecureString -String $PasswordForSSISDBCleanupUser -AsPlainText -Force
$JobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "SSISDBLogCleanupUser", $JobCredSecure
$JobCred = $JobAgent | New-AzureRmSqlElasticJobCredential -Name "SSISDBLogCleanupUser" -Credential $JobCred

# In the master database of the target SQL server which contains SSISDB to cleanup 
# - Create the job user login
Write-Output "Grant permissions on the master database of the target server..."
$Params = @{
  'Database' = 'master'
  'ServerInstance' = $SSISDBServerEndpoint
  'Username' = $SSISDBServerAdminUserName
  'Password' = $SSISDBServerAdminPassword
  'OutputSqlErrors' = $true
  'Query' = "CREATE LOGIN SSISDBLogCleanupUser WITH PASSWORD = '" + $PasswordForSSISDBCleanupUser + "'"
}
Invoke-SqlCmd @Params

# For SSISDB database of the target SQL server
# - Create the SSISDBLogCleanup user from the SSISDBlogCleanup user login
# - Grant permissions for the execution of SSISDB log cleanup stored procedure 
Write-Output "Grant appropriate permissions on SSISDB database..."
$TargetDatabase = $SSISDBName
$CreateJobUser = "CREATE USER SSISDBLogCleanupUser FROM LOGIN SSISDBLogCleanupUser"
$GrantStoredProcedureExecution = "GRANT EXECUTE ON internal.cleanup_server_retention_window_exclusive TO SSISDBLogCleanupUser"

$TargetDatabase | % {
  $Params.Database = $_
  $Params.Query = $CreateJobUser
  Invoke-SqlCmd @Params
  $Params.Query = $GrantStoredProcedureExecution
  Invoke-SqlCmd @Params
}

# Create a target group which includes SSISDB database needed to cleanup
Write-Output "Creating the target group including only SSISDB database needed to cleanup ..."
$SSISDBTargetGroup = $JobAgent | New-AzureRmSqlElasticJobTargetGroup -Name "SSISDBTargetGroup"
$SSISDBTargetGroup | Add-AzureRmSqlElasticJobTarget -ServerName $SSISDBServerEndpoint -Database $SSISDBName 

# Create the job to trigger execution of SSISDB log cleanup stored procedure
Write-Output "Creating a new job to trigger execution of the stored procedure for SSISDB log cleanup"
$JobName = "CleanupSSISDBLog"
$Job = $JobAgent | New-AzureRmSqlElasticJob -Name $JobName -RunOnce
$Job

# Add the job step to execute internal.cleanup_server_retention_window_exclusive
Write-Output "Adding the job step for the cleanup stored procedure execution"
$SqlText = "EXEC internal.cleanup_server_retention_window_exclusive"
$Job | Add-AzureRmSqlElasticJobStep -Name "step to execute cleanup stored procedure" -TargetGroupName $SSISDBTargetGroup.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText

# Run the job to immediately start cleanup stored procedure execution for once
IF(($RunJobOrNot = "Y") -Or ($RunJobOrNot = "y"))
{
Write-Output "Start a new execution of the stored procedure for SSISDB log cleanup immediately..."
$JobExecution = $Job | Start-AzureRmSqlElasticJob
$JobExecution
}

# Schedule the job running to trigger stored procedure execution on schedule for removing SSISDB logs outside the retention window
Write-Output "Start the execution schedule of the stored procedure for SSISDB log cleanup..."
$Job | Set-AzureRmSqlElasticJob -IntervalType $IntervalType -IntervalCount $IntervalCount -StartTime $StartTime -Enable
```

## <a name="clean-up-logs-with-transact-sql"></a>Czyszczenie dzienników przy użyciu języka Transact-SQL

Następujące przykładowe skrypty Transact-SQL tworzą nowe zadanie elastyczne, aby wyzwolić procedurę składowaną dla czyszczenia dziennika SSISDB. Aby uzyskać więcej informacji, zobacz [Używanie języka Transact-SQL (T-SQL) w celu tworzenia zadań Elastic Database i zarządzania nimi](../azure-sql/database/elastic-jobs-tsql-create-manage.md).

1. Utwórz lub Zidentyfikuj pustą S0ę lub wyższą Azure SQL Database, aby być bazą danych zadań SSISDBCleanup. Następnie Utwórz agenta elastycznego zadania w [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.SQLElasticJobAgent).

2. W bazie danych zadań Utwórz poświadczenie dla zadania oczyszczania dziennika SSISDB. To poświadczenie jest używane do nawiązania połączenia z bazą danych SSISDB w celu oczyszczenia dzienników.

    ```sql
    -- Connect to the job database specified when creating the job agent
    -- Create a database master key if one does not already exist, using your own password.  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD= '<EnterStrongPasswordHere>';  

    -- Create a credential for SSISDB log cleanup.  
    CREATE DATABASE SCOPED CREDENTIAL SSISDBLogCleanupCred WITH IDENTITY = 'SSISDBLogCleanupUser', SECRET = '<EnterStrongPasswordHere>'; 
    ```

3. Zdefiniuj grupę docelową, która zawiera bazę danych SSISDB, dla której chcesz uruchomić procedurę składowaną oczyszczania.

    ```sql
    -- Connect to the job database 
    -- Add a target group 
    EXEC jobs.sp_add_target_group 'SSISDBTargetGroup'

    -- Add SSISDB database into the target group
    EXEC jobs.sp_add_target_group_member 'SSISDBTargetGroup',
    @target_type = 'SqlDatabase',
    @server_name = '<EnterSSISDBTargetServerName>',
    @database_name = '<EnterSSISDBName>'

    --View the recently created target group and target group members
    SELECT * FROM jobs.target_groups WHERE target_group_name = 'SSISDBTargetGroup';
    SELECT * FROM jobs.target_group_members WHERE target_group_name = 'SSISDBTargetGroup';
    ```
4. Udziel odpowiednich uprawnień dla bazy danych SSISDB. Wykaz SSISDB musi mieć odpowiednie uprawnienia do procedury składowanej, aby pomyślnie uruchomić oczyszczanie dziennika SSISDB. Aby uzyskać szczegółowe wskazówki, zobacz Zarządzanie nazwami [logowania](../azure-sql/database/logins-create-manage.md).

    ```sql
    -- Connect to the master database in the target server including SSISDB 
    CREATE LOGIN SSISDBLogCleanupUser WITH PASSWORD = '<strong_password>';

    -- Connect to SSISDB database in the target server to cleanup logs
    CREATE USER SSISDBLogCleanupUser FROM LOGIN SSISDBLogCleanupUser;
    GRANT EXECUTE ON internal.cleanup_server_retention_window_exclusive TO SSISDBLogCleanupUser
    ```
5. Utwórz zadanie i Dodaj krok zadania, aby wyzwolić wykonywanie procedury składowanej dla czyszczenia dziennika SSISDB.

    ```sql
    --Connect to the job database 
    --Add the job for the execution of SSISDB log cleanup stored procedure.
    EXEC jobs.sp_add_job @job_name='CleanupSSISDBLog', @description='Remove SSISDB logs which are outside the retention window'

    --Add a job step to execute internal.cleanup_server_retention_window_exclusive
    EXEC jobs.sp_add_jobstep @job_name='CleanupSSISDBLog',
    @command=N'EXEC internal.cleanup_server_retention_window_exclusive',
    @credential_name='SSISDBLogCleanupCred',
    @target_group_name='SSISDBTargetGroup'
    ```
6. Przed kontynuowaniem upewnij się, że okno przechowywania zostało odpowiednio ustawione. Dzienniki SSISDB poza oknem zostaną usunięte i nie można ich odzyskać.

   Następnie możesz uruchomić zadanie natychmiast, aby rozpocząć oczyszczanie dziennika SSISDB.

    ```sql
    --Connect to the job database 
    --Run the job immediately to execute the stored procedure for SSISDB log cleanup
    declare @je uniqueidentifier
    exec jobs.sp_start_job 'CleanupSSISDBLog', @job_execution_id = @je output

    --Watch the execution results for SSISDB log cleanup 
    select @je
    select * from jobs.job_executions where job_execution_id = @je
    ```
7. Opcjonalnie możesz zaplanować wykonywanie zadań, aby usunąć dzienniki SSISDB poza oknem przechowywania zgodnie z harmonogramem. Aby zaktualizować parametry zadania, należy użyć podobnej instrukcji.

    ```sql
    --Connect to the job database 
    EXEC jobs.sp_update_job
    @job_name='CleanupSSISDBLog',
    @enabled=1,
    @schedule_interval_type='<EnterIntervalType(Month,Day,Hour,Minute,Second)>',
    @schedule_interval_count='<EnterDetailedIntervalValue>',
    @schedule_start_time='<EnterProperStartTimeForSchedule>',
    @schedule_end_time='<EnterProperEndTimeForSchedule>'
    ```

## <a name="monitor-the-cleanup-job-in-the-azure-portal"></a>Monitoruj zadanie oczyszczania w Azure Portal

Wykonanie zadania oczyszczania można monitorować w Azure Portal. Dla każdego wykonania zobaczysz stan, godzinę rozpoczęcia i godzinę zakończenia zadania.

![Monitoruj zadanie oczyszczania w Azure Portal](media/how-to-clean-up-ssisdb-logs-with-elastic-jobs/monitor-cleanup-job-portal.png)

## <a name="monitor-the-cleanup-job-with-transact-sql"></a>Monitorowanie zadania oczyszczania przy użyciu języka Transact-SQL

Możesz również użyć języka Transact-SQL, aby wyświetlić historię wykonywania zadania oczyszczania.

```sql
--Connect to the job database 
--View all execution statuses for the job to cleanup SSISDB logs
SELECT * FROM jobs.job_executions WHERE job_name = 'CleanupSSISDBLog' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions WHERE is_active = 1
ORDER BY start_time DESC
```

## <a name="next-steps"></a>Następne kroki

Zadania związane z zarządzaniem i monitorowaniem dotyczące Azure-SSIS Integration Runtime można znaleźć w następujących artykułach. Azure-SSIS IR to aparat środowiska uruchomieniowego dla pakietów SSIS przechowywanych w SSISDB w Azure SQL Database.

-   [Ponowne konfigurowanie środowiska Azure SSIS Integration Runtime](manage-azure-ssis-integration-runtime.md)

-   [Monitoruj środowisko Azure-SSIS Integration Runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime).
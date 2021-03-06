---
title: Nawiązywanie połączenia z usługą Synapse SQL przy użyciu Azure Data Studio (wersja zapoznawcza)
description: Użyj Azure Data Studio (wersja zapoznawcza), aby nawiązać połączenie z usługą SQL Synapse i wysyłać do niej zapytania Synapse.
services: synapse analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 3af28ef575f770a5b8c3fb73ade1f9c53be66843
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685753"
---
# <a name="connect-to-synapse-sql-with-azure-data-studio-preview"></a>Nawiązywanie połączenia z usługą Synapse SQL przy użyciu Azure Data Studio (wersja zapoznawcza)

> [!div class="op_single_selector"]
>
> * [Azure Data Studio](get-started-azure-data-studio.md)
> * [Power BI](get-started-power-bi-professional.md)
> * [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> * [sqlcmd](get-started-connect-sqlcmd.md)
> * [SSMS](get-started-ssms.md)

Możesz użyć [Azure Data Studio (wersja zapoznawcza)](/sql/azure-data-studio/download-azure-data-studio?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) , aby nawiązać połączenie z usługą SQL Synapse i wysyłać do niej zapytania Synapse. 

## <a name="connect"></a>Połącz

Aby nawiązać połączenie z usługą Synapse SQL, Otwórz Azure Data Studio i wybierz pozycję **nowe połączenie**.

![Otwórz Azure Data Studio](./media/get-started-azure-data-studio/1-start.png)

Wybierz **Microsoft SQL Server** jako **Typ połączenia**.

Połączenie wymaga następujących parametrów:

* **Serwer:** Serwer w formie `<Azure Synapse workspace name>` OnDemand.SQL.azuresynapse.NET
* **Baza danych:** Nazwa bazy danych

> [!NOTE]
> Jeśli chcesz użyć **bezserwerowej puli SQL (wersja zapoznawcza)** , adres URL powinien wyglądać następująco:
>
> - `<Azure Synapse workspace name>`-ondemand.sql.azuresynapse.net.
>
> Jeśli chcesz użyć **dedykowanej puli SQL** , adres URL powinien wyglądać następująco:
>
> - `<Azure Synapse workspace name>`. sql.azuresynapse.net

Wybierz pozycję **uwierzytelnianie systemu Windows**, **Azure Active Directory** lub **Identyfikator logowania SQL** jako **Typ uwierzytelniania**.

Aby użyć **logowania SQL** jako typu uwierzytelniania, Dodaj parametry nazwy użytkownika/hasła:

* **Użytkownik:** Użytkownik serwera w formularzu `<User>`
* **Hasło:** Hasło skojarzone z użytkownikiem

Aby użyć Azure Active Directory, należy wybrać wymagany typ uwierzytelniania.

![Uwierzytelnianie za pomocą usługi AAD](./media/get-started-azure-data-studio/3-aad-auth.png)

Poniższy zrzut ekranu przedstawia **szczegóły połączenia** dla **uwierzytelniania systemu Windows**:

![Uwierzytelnianie systemu Windows](./media/get-started-azure-data-studio/3-windows-auth.png)

Poniższy zrzut ekranu przedstawia **szczegóły połączenia** przy użyciu **logowania SQL**:

![Identyfikator logowania SQL](./media/get-started-azure-data-studio/2-database-details.png)

Po pomyślnym zalogowaniu powinien zostać wyświetlony pulpit nawigacyjny podobny do tego: ![ pulpit nawigacyjny](./media/get-started-azure-data-studio/4-dashboard.png)

## <a name="query"></a>Zapytanie

Po nawiązaniu połączenia możesz wysyłać zapytania do Synapse SQL przy użyciu obsługiwanych instrukcji [języka Transact-SQL (T-SQL)](/sql/t-sql/language-reference?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) dla tego wystąpienia. Wybierz pozycję **nowe zapytanie** w widoku pulpitu nawigacyjnego, aby rozpocząć pracę.

![Nowe zapytanie](./media/get-started-azure-data-studio/5-new-query.png)

Na przykład można użyć następującej instrukcji języka Transact-SQL do [wykonywania zapytań dotyczących plików Parquet](query-parquet-files.md) przy użyciu puli SQL bezserwerowej:

```sql
SELECT COUNT(*)
FROM  
OPENROWSET(
    BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
    FORMAT='PARQUET'
)
```
## <a name="next-steps"></a>Następne kroki 
Poznaj inne sposoby nawiązywania połączenia z usługą Synapse SQL: 

- [SSMS](get-started-ssms.md)
- [Power BI](get-started-power-bi-professional.md)
- [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [sqlcmd](get-started-connect-sqlcmd.md)

Aby uzyskać więcej informacji, odwiedź stronę [używanie Azure Data Studio do łączenia i wykonywania zapytań dotyczących danych za pomocą dedykowanej puli SQL w usłudze Azure Synapse Analytics](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-dw). 

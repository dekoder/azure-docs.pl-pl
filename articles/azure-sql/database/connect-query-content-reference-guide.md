---
title: Nawiązywanie połączeń i wykonywanie zapytań
description: Linki do Azure SQL Database przewodników szybki start przedstawiających sposób nawiązywania połączeń z usługą i Azure SQL Database zapytań oraz wystąpienia zarządzanego usługi Azure SQL.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: guide
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/29/2020
ms.openlocfilehash: ec40e209d522daf882b8c704c7de6a1fb6aa29f9
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/26/2020
ms.locfileid: "96183944"
---
# <a name="azure-sql-database-and-azure-sql-managed-instance-connect-and-query-articles"></a>Azure SQL Database i wyszukiwanie w artykułach wystąpienia zarządzanego usługi Azure SQL
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Poniższy dokument zawiera linki do przykładów platformy Azure, w których pokazano, jak nawiązać połączenie i wykonać zapytania dotyczące Azure SQL Database i wystąpienia zarządzanego Azure SQL. Niektóre powiązane zalecenia dotyczące zabezpieczeń na poziomie transportu znajdują się w temacie [zagadnienia dotyczące łączności z bazą danych TLS](#tls-considerations-for-database-connectivity).

## <a name="quickstarts"></a>Przewodniki Szybki start

| Szybki start | Opis |
|---|---|
|[SQL Server Management Studio](connect-query-ssms.md)|Ten przewodnik Szybki Start przedstawia sposób łączenia się z bazą danych za pomocą programu SSMS, a następnie używania instrukcji języka Transact-SQL do wykonywania zapytań, wstawiania, aktualizowania i usuwania danych w bazie danych.|
|[Azure Data Studio](/sql/azure-data-studio/quickstart-sql-database?toc=%2fazure%2fsql-database%2ftoc.json)|W tym przewodniku szybki start pokazano, jak za pomocą Azure Data Studio połączyć się z bazą danych, a następnie użyć instrukcji języka Transact-SQL (T-SQL) w celu utworzenia TutorialDB używanych w samouczkach Azure Data Studio.|
|[Witryna Azure Portal](connect-query-portal.md)|W tym przewodniku szybki start pokazano, jak używać edytora zapytań do łączenia się z bazą danych (tylko Azure SQL Database), a następnie używać instrukcji języka Transact-SQL do wykonywania zapytań, wstawiania, aktualizowania i usuwania danych w bazie danych.|
|[Visual Studio Code](connect-query-vscode.md)|W tym przewodniku szybki start pokazano, jak za pomocą Visual Studio Code połączyć się z bazą danych, a następnie użyć instrukcji języka Transact-SQL do wykonywania zapytań, wstawiania, aktualizowania i usuwania danych w bazie danych.|
|[.NET przy użyciu programu Visual Studio](connect-query-dotnet-visual-studio.md)|W tym przewodniku szybki start pokazano, jak za pomocą programu .NET Framework utworzyć program w języku C# z programem Visual Studio, aby połączyć się z bazą danych i użyć instrukcji języka Transact-SQL do wykonywania zapytań dotyczących danych.|
|[.NET Core](connect-query-dotnet-core.md)|Ten przewodnik Szybki Start przedstawia sposób korzystania z platformy .NET Core w systemie Windows/Linux/macOS w celu utworzenia programu w języku C# do łączenia się z bazą danych i używania instrukcji języka Transact-SQL do wykonywania zapytań dotyczących danych.|
|[Przejdź](connect-query-go.md)|W tym przewodniku szybki start pokazano, jak używać języka go do łączenia się z bazą danych. Ponadto przedstawiono instrukcje języka Transact-SQL umożliwiające wykonywanie zapytań i modyfikowanie danych.|
|[Java](connect-query-java.md)|W tym przewodniku szybki start pokazano, jak nawiązać połączenie z bazą danych przy użyciu języka Java, a następnie użyć instrukcji języka Transact-SQL do wykonywania zapytań dotyczących danych.|
|[Node.js](connect-query-nodejs.md)|W tym przewodniku szybki start pokazano, jak za pomocą Node.js utworzyć program do łączenia się z bazą danych i używania instrukcji języka Transact-SQL do wykonywania zapytań dotyczących danych.|
|[PHP](connect-query-php.md)|Ten przewodnik Szybki Start przedstawia sposób tworzenia programu w celu nawiązania połączenia z bazą danych przy użyciu języka PHP i wykonywania zapytań o dane przy użyciu instrukcji Transact-SQL.|
|[Python](connect-query-python.md)|Ten przewodnik Szybki Start przedstawia sposób nawiązywania połączeń z bazą danych przy użyciu języka Python i używania instrukcji Transact-SQL do wykonywania zapytań dotyczących danych. |
|[Ruby](connect-query-ruby.md)|Ten przewodnik Szybki Start przedstawia sposób tworzenia programu w celu nawiązania połączenia z bazą danych i używania instrukcji języka Transact-SQL do wykonywania zapytań dotyczących danych.|
|[R](connect-query-r.md)|Ten przewodnik Szybki Start przedstawia sposób używania języka R z Azure SQL Database Machine Learning Services do tworzenia programu w celu nawiązania połączenia z bazą danych w Azure SQL Database i używania instrukcji języka Transact-SQL do wykonywania zapytań dotyczących danych.|
|||

## <a name="tls-considerations-for-database-connectivity"></a>Zagadnienia dotyczące łączności z bazą danych TLS

Usługa Transport Layer Security (TLS) jest używana przez wszystkie sterowniki dostarczone lub obsługiwane przez firmę Microsoft w celu łączenia się z bazami danych w Azure SQL Database lub wystąpieniu zarządzanym Azure SQL. Nie jest konieczna żadna specjalna konfiguracja. W przypadku wszystkich połączeń z wystąpieniem SQL Server, bazy danych w Azure SQL Database lub wystąpienia zarządzanego wystąpienia usługi Azure SQL zaleca się, aby wszystkie aplikacje ustawili następujące konfiguracje lub ich odpowiedniki:

- **Encrypt = On**
- **TrustServerCertificate = Off**

Niektóre systemy używają innych, lecz równoważnych słów kluczowych dla tych słów kluczowych konfiguracji. Te konfiguracje zapewniają, że sterownik klienta weryfikuje tożsamość certyfikatu TLS otrzymanego z serwera.

Zalecamy również wyłączenie protokołu TLS 1.1 i 1.0 na kliencie, jeśli wymagana jest zgodność ze standardem Payment Card Industry Data Security Standard (PCI DSS).

Sterowniki firm innych niż Microsoft mogą nie używać domyślnie protokołu TLS. Może to być współczynnik podczas nawiązywania połączenia z usługą Azure SQL Database lub wystąpieniem zarządzanym usługi Azure SQL. Aplikacje ze sterownikami osadzonymi mogą nie pozwalać na kontrolowanie tych ustawień połączenia. Zaleca się zbadanie zabezpieczeń takich sterowników i aplikacji przed ich użyciem w systemach, które wchodzą w interakcję z danymi poufnymi.

## <a name="libraries"></a>Biblioteki

Do łączenia się z usługą Azure SQL Database lub wystąpieniem zarządzanym Azure SQL można używać różnych bibliotek i platform. Zapoznaj się z naszymi [samouczkami wprowadzającymi](https://aka.ms/sqldev), aby szybko rozpocząć pracę z językami programowania, takimi jak C#, Java, Node.js, PHP i Python. Następnie utwórz aplikację przy użyciu programu SQL Server w systemie Linux lub Windows albo platformy Docker w systemie macOS.

W poniższej tabeli wymieniono biblioteki łączności (*sterowniki*), których aplikacje klienckie mogą używać w różnych językach, aby łączyć się z programem SQL Server działającym lokalnie lub w chmurze i korzystać z niego. Można ich używać w systemie Linux, Windows lub Docker i używać ich do łączenia się z Azure SQL Database, wystąpienia zarządzanego usługi Azure SQL i usługi Azure Synapse Analytics (dawniej SQL Data Warehouse).

| Język | Platforma | Dodatkowe zasoby | Pobierz | Wprowadzenie |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Program Microsoft ADO.NET dla programu SQL Server](/sql/connect/ado-net/microsoft-ado-net-sql-server) | [Pobieranie](https://www.microsoft.com/net/download/) | [Wprowadzenie](https://www.microsoft.com/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Sterownik JDBC firmy Microsoft dla programu SQL Server](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server/) | [Pobieranie](https://go.microsoft.com/fwlink/?linkid=852460) |  [Wprowadzenie](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [Sterowniki PHP SQL dla programu SQL Server](/sql/connect/php/microsoft-php-driver-for-sql-server) | [Pobieranie](/sql/connect/php/download-drivers-php-sql-server) | [Wprowadzenie](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/)
| Node.js | Windows, Linux, macOS | [Sterowniki Node.js dla programu SQL Server](/sql/connect/node-js/node-js-driver-for-sql-server/) | [Instalacja](/sql/connect/node-js/step-1-configure-development-environment-for-node-js-development/) |  [Wprowadzenie](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Sterownik SQL języka Python](/sql/connect/python/python-driver-for-sql-server/) | Opcje instalacji: <br/> \* [pymssql](/sql/connect/python/pymssql/step-1-configure-development-environment-for-pymssql-python-development/) <br/> \* [pyodbc](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development/) |  [Wprowadzenie](https://www.microsoft.com/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Sterowniki Ruby dla programu SQL Server](/sql/connect/ruby/ruby-driver-for-sql-server/) | [Instalacja](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development/) | [Wprowadzenie](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Sterownik ODBC firmy Microsoft dla programu SQL Server](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server/) | [Pobieranie](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server/) |  

W poniższej tabeli przedstawiono przykłady struktur mapowania obiektów (ORM) i platformy sieci Web, które mogą być używane przez aplikacje klienckie z SQL Server, Azure SQL Database, wystąpienia zarządzanego Azure SQL lub Azure Synapse Analytics. Można używać platform w systemie Linux, Windows lub Docker.

| Język | Platforma | Struktury ORM |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](/ef)<br>[Entity Framework Core](/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](https://hibernate.org/orm)|
| PHP | Windows, Linux, macOS | [Laravel (Eloquent)](https://laravel.com/docs/eloquent)<br>[Doctrine](https://www.doctrine-project.org/projects/orm.html) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](https://sequelize.org/) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](https://rubyonrails.org/) |
||||

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje na temat architektury łączności, zobacz artykuł [Azure SQL Database Connectivity Architecture (Architektura łączności usługi Azure SQL Database)](connectivity-architecture.md).
- Znajdź [SQL Server sterowniki](/sql/connect/sql-connection-libraries/) , które są używane do nawiązywania połączenia z aplikacji klienckich.
- Połącz z usługą Azure SQL Database lub wystąpieniem zarządzanym Azure SQL:
  - [Łączenie i wykonywanie zapytań przy użyciu platformy .NET (C#)](connect-query-dotnet-core.md)
  - [Łączenie i wykonywanie zapytań przy użyciu języka PHP](connect-query-php.md)
  - [Łączenie i wykonywanie zapytań przy użyciu Node.js](connect-query-nodejs.md)
  - [Nawiązywanie połączenia i wykonywanie zapytań przy użyciu języka Java](connect-query-java.md)
  - [Łączenie i wykonywanie zapytań przy użyciu języka Python](connect-query-python.md)
  - [Łączenie i zapytanie przy użyciu języka Ruby](connect-query-ruby.md)
- Przykłady kodu logiki ponawiania próby:
  - [Odporne na połączenia z usługą ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
  - [Odporne na błędy w języku PHP][step-4-connect-resiliently-to-sql-with-php-p42h]

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: /sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: /sql/connect/php/step-4-connect-resiliently-to-sql-with-php
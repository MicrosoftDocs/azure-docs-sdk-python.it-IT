---
title: Librerie del database SQL di Azure per Python
description: Connettersi al database SQL di Azure tramite il driver ODBC e pyodbc oppure gestire le istanze di SQL di Azure con l'API di gestione.
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 01/09/2018
ms.topic: reference
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: baa0e53a77d18dc93241135b5b0fecff5786114c
ms.sourcegitcommit: ab96bcebe9d5bfa5f32ec5a61b79bd7483fadcad
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/17/2018
---
# <a name="azure-sql-database-libraries-for-python"></a>Librerie del database SQL di Azure per Python

## <a name="overview"></a>Panoramica

Usare i dati archiviati nel [database SQL di Azure](/azure/sql-database/sql-database-technical-overview) da Python con il [driver di database ODBC](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers) pyodbc. Visualizzare la [guida introduttiva](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) sulla connessione a un database SQL di Azure e sull'uso di istruzioni Transact-SQL per eseguire query sui dati e ottenere un [esempio](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) introduttivo con pyodbc.

## <a name="install-odbc-driver-and-pyodbc"></a>Installare il driver ODBC e pyodbc

```bash
pip install pyodbc
```
Altri [dettagli](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) sull'installazione delle librerie di comunicazione Python e dei database.

## <a name="connect-and-execute-a-sql-query"></a>Connettersi ed eseguire una query SQL

### <a name="connect-to-a-sql-database"></a>Connettersi al database SQL

```python
import pyodbc

server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'

cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
```

### <a name="execute-a-sql-query"></a>Eseguire una query SQL

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [Esempio di pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a>Connessione a soluzioni ORM

pyodbc interagisce con altre soluzioni ORM, ad esempio [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) e [Django](https://github.com/lionheart/django-pyodbc/). 

## <a name="management-apipythonapioverviewazuresqlmanagementlibrary"></a>[API di gestione](/python/api/overview/azure/sql/managementlibrary)

Creare e gestire le risorse del database SQL di Azure nella sottoscrizione con l'API di gestione. 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a>Esempio

Creare una risorsa del database SQL e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/sql/managementlibrary)


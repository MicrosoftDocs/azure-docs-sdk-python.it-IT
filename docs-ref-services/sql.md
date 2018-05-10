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
ms.openlocfilehash: 5b73977fb58ed3cb17d675784da921b0e199d165
ms.sourcegitcommit: 560362db0f65307c8b02b7b7ad8642b5c4aa6294
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2018
---
# <a name="azure-sql-database-libraries-for-python"></a><span data-ttu-id="acc86-103">Librerie del database SQL di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="acc86-103">Azure SQL Database libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="acc86-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="acc86-104">Overview</span></span>

<span data-ttu-id="acc86-105">Usare i dati archiviati nel [database SQL di Azure](/azure/sql-database/sql-database-technical-overview) da Python con il [driver di database ODBC](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers) pyodbc.</span><span class="sxs-lookup"><span data-stu-id="acc86-105">Work with data stored in [Azure SQL Database](/azure/sql-database/sql-database-technical-overview) from Python with the pyodbc [ODBC database driver](https://github.com/mkleehammer/pyodbc/wiki/Drivers-and-Driver-Managers).</span></span> <span data-ttu-id="acc86-106">Visualizzare la [guida introduttiva](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) sulla connessione a un database SQL di Azure e sull'uso di istruzioni Transact-SQL per eseguire query sui dati e ottenere un [esempio](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) introduttivo con pyodbc.</span><span class="sxs-lookup"><span data-stu-id="acc86-106">View our [quickstart](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) on connecting to an Azure SQL database and using Transact-SQL statements to query data and getting started [sample](https://github.com/mkleehammer/pyodbc/wiki/Getting-started) with pyodbc.</span></span>

## <a name="install-odbc-driver-and-pyodbc"></a><span data-ttu-id="acc86-107">Installare il driver ODBC e pyodbc</span><span class="sxs-lookup"><span data-stu-id="acc86-107">Install ODBC driver and pyodbc</span></span>

```bash
pip install pyodbc
```
<span data-ttu-id="acc86-108">Altri [dettagli](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) sull'installazione delle librerie di comunicazione Python e dei database.</span><span class="sxs-lookup"><span data-stu-id="acc86-108">More [details](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) about installing the python and database communication libraries.</span></span>

## <a name="connect-and-execute-a-sql-query"></a><span data-ttu-id="acc86-109">Connettersi ed eseguire una query SQL</span><span class="sxs-lookup"><span data-stu-id="acc86-109">Connect and execute a SQL query</span></span>

### <a name="connect-to-a-sql-database"></a><span data-ttu-id="acc86-110">Connettersi al database SQL</span><span class="sxs-lookup"><span data-stu-id="acc86-110">Connect to a SQL database</span></span>

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

### <a name="execute-a-sql-query"></a><span data-ttu-id="acc86-111">Eseguire una query SQL</span><span class="sxs-lookup"><span data-stu-id="acc86-111">Execute a SQL query</span></span>

```python
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print (str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="acc86-112">Esempio di pyodbc</span><span class="sxs-lookup"><span data-stu-id="acc86-112">pyodbc sample</span></span>](https://github.com/mkleehammer/pyodbc/wiki/Getting-started)

## <a name="connecting-to-orms"></a><span data-ttu-id="acc86-113">Connessione a soluzioni ORM</span><span class="sxs-lookup"><span data-stu-id="acc86-113">Connecting to ORMs</span></span>

<span data-ttu-id="acc86-114">pyodbc interagisce con altre soluzioni ORM, ad esempio [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) e [Django](https://github.com/lionheart/django-pyodbc/).</span><span class="sxs-lookup"><span data-stu-id="acc86-114">pyodbc works with other ORMs such as [SQLAlchemy](http://docs.sqlalchemy.org/en/latest/dialects/mssql.html?highlight=pyodbc#module-sqlalchemy.dialects.mssql.pyodbc) and [Django](https://github.com/lionheart/django-pyodbc/).</span></span> 

## <a name="management-apipythonapioverviewazuresqlmanagement"></a>[<span data-ttu-id="acc86-115">API di gestione</span><span class="sxs-lookup"><span data-stu-id="acc86-115">Management API</span></span>](/python/api/overview/azure/sql/management)

<span data-ttu-id="acc86-116">Creare e gestire le risorse del database SQL di Azure nella sottoscrizione con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="acc86-116">Create and manage Azure SQL Database resources in your subscription with the management API.</span></span> 

```bash
pip install azure-common
pip install azure-mgmt-sql
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="acc86-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="acc86-117">Example</span></span>

<span data-ttu-id="acc86-118">Creare una risorsa del database SQL e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.</span><span class="sxs-lookup"><span data-stu-id="acc86-118">Create a SQL Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.sql import SqlManagementClient

RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_SERVER = 'yourvirtualsqlserver'
SQL_DB = 'YOUR_SQLDB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

# create resource client
resource_client = get_client_from_cli_profile(ResourceManagementClient)
# create resource group
resource_client.resource_groups.create_or_update(RESOURCE_GROUP, {'location': LOCATION})

sql_client = get_client_from_cli_profile(SqlManagementClient)

# Create a SQL server
server = sql_client.servers.create_or_update(
    RESOURCE_GROUP,
    SQL_SERVER,
    {
        'location': LOCATION,
        'version': '12.0', # Required for create
        'administrator_login': USERNAME, # Required for create
        'administrator_login_password': PASSWORD # Required for create
    }
)

# Create a SQL database in the Basic tier
database = sql_client.databases.create_or_update(
    RESOURCE_GROUP,
    SQL_SERVER,
    SQL_DB,
    {
        'location': LOCATION,
        'collation': 'SQL_Latin1_General_CP1_CI_AS',
        'create_mode': 'default',
        'requested_service_objective_name': 'Basic'
    }
)

# Open access to this server for IPs
firewall_rule = sql_client.firewall_rules.create_or_update(
    RESOURCE_GROUP,
    SQL_DB,
    "firewall_rule_name_123.123.123.123",
    "123.123.123.123", # Start ip range
    "167.220.0.235"  # End ip range
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="acc86-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="acc86-119">Explore the Management APIs</span></span>](/python/api/overview/azure/sql/management)


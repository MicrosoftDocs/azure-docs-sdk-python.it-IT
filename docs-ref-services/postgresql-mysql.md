---
title: Librerie di Azure per MySQL/PostgreSQL per Python
description: ''
keywords: Azure, Python, SDK, API, SQL, database, MySQL, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.openlocfilehash: e3cd84288ad8d49cfcd673506e2db150c02e7918
ms.sourcegitcommit: 16eecfc4ed0e2a8344ce5887327cdf2619ba89e4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/21/2018
ms.locfileid: "39189591"
---
# <a name="azure-mysqlpostgresql-libraries-for-python"></a><span data-ttu-id="afdb0-103">Librerie di Azure per MySQL/PostgreSQL per Python</span><span class="sxs-lookup"><span data-stu-id="afdb0-103">Azure MySQL/PostgreSQL libraries for Python</span></span>

## <a name="mysql"></a><span data-ttu-id="afdb0-104">MySQL</span><span class="sxs-lookup"><span data-stu-id="afdb0-104">MySQL</span></span>

<span data-ttu-id="afdb0-105">Usare le risorse e i dati archiviati nel [database MySQL di Azure](/azure/mysql/overview) da Python con lo strumento di gestione MySQL e pyodbc.</span><span class="sxs-lookup"><span data-stu-id="afdb0-105">Work with resources and data stored in [Azure MySQL Database](/azure/mysql/overview) from python with the MySQL manager and pyodbc.</span></span>

### <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="afdb0-106">Driver ODBC client e pyodbc</span><span class="sxs-lookup"><span data-stu-id="afdb0-106">Client ODBC driver and pyodbc</span></span>

<span data-ttu-id="afdb0-107">La libreria client consigliata per l'accesso al Database di Azure per MySQL è il [driver ODBC](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft.</span><span class="sxs-lookup"><span data-stu-id="afdb0-107">The recommended client library for accessing Azure Database for MySQL is the Microsoft [ODBC driver](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span> <span data-ttu-id="afdb0-108">Usare il driver ODBC per connettersi al database ed eseguire direttamente le istruzioni SQL.</span><span class="sxs-lookup"><span data-stu-id="afdb0-108">Use the ODBC driver to connect to the database and execute SQL statements directly.</span></span>

#### <a name="example"></a><span data-ttu-id="afdb0-109">Esempio</span><span class="sxs-lookup"><span data-stu-id="afdb0-109">Example</span></span>

<span data-ttu-id="afdb0-110">Connettersi a un database di Azure per MySQL e selezionare tutti i record nella tabella relativa alle vendite.</span><span class="sxs-lookup"><span data-stu-id="afdb0-110">Connect to a Azure Database for MySQL and select all records in the sales table.</span></span> <span data-ttu-id="afdb0-111">È possibile ottenere la stringa di connessione ODBC per il database dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="afdb0-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

```python
SERVER = 'YOUR_SEVER_NAME' + '.mysql.database.azure.com'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_MYSQL_USERNAME'
PASSWORD = 'YOUR_MYSQL_PASSWORD'

DRIVER = '{MySQL ODBC 5.3 UNICODE Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=3306;SERVER=' + SERVER + ';PORT=3306;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

### <a name="management-api"></a><span data-ttu-id="afdb0-112">API di gestione</span><span class="sxs-lookup"><span data-stu-id="afdb0-112">Management API</span></span>

<span data-ttu-id="afdb0-113">Creare e gestire le risorse di MySQL nella sottoscrizione con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="afdb0-113">Create and manage MySQL resources in your subscription with the management API.</span></span>

#### <a name="requirements"></a><span data-ttu-id="afdb0-114">Requisiti</span><span class="sxs-lookup"><span data-stu-id="afdb0-114">Requirements</span></span>
<span data-ttu-id="afdb0-115">È necessario installare le librerie di gestione di MySQL per Python.</span><span class="sxs-lookup"><span data-stu-id="afdb0-115">You must install the MySQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="afdb0-116">Per informazioni dettagliate su come ottenere le credenziali per l'autenticazione nel client di gestione, vedere la pagina relativa all'[autenticazione di Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).</span><span class="sxs-lookup"><span data-stu-id="afdb0-116">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

#### <a name="example"></a><span data-ttu-id="afdb0-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="afdb0-117">Example</span></span>

<span data-ttu-id="afdb0-118">Creare una risorsa del database MySQL 5.7 e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.</span><span class="sxs-lookup"><span data-stu-id="afdb0-118">Create a MySQL 5.7 Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

```python

from azure.mgmt.rdbms.mysql import MySQLManagementClient
from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE-GROUP_WITH_POSTGRES"
MYSQL_SERVER = "YOUR_DESIRED_MYSQL_SERVER_NAME"
MYSQL_ADMIN_USER = "YOUR_MYSQL_ADMIN_USERNAME"
MYSQL_ADMIN_PASSWORD = "YOUR_MYSQL_ADMIN_PASSOWRD"
LOCATION = "westus"  # example Azure availability zone, should match resource group


client = MySQLManagementClient(credentials=creds,
    subscription_id=SUBSCRIPTION_ID)

server_creation_poller = client.servers.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=MYSQL_SERVER,
    ServerForCreate(
        ServerPropertiesForDefaultCreate(
            administrator_login=MYSQL_ADMIN_USER,
            administrator_login_password=MYSQL_ADMIN_PASSWORD,
            version=ServerVersion.five_full_stop_seven
        ),
        location=LOCATION
    )
)

server = server_creation_poller.result()

# Open access to this server for IPs
rule_creation_poller = client.firewall_rules.create_or_update(
    RESOURCE_GROUP
    MYSQL_SERVER,
    "some_custom_ip_range_whitelist_rule_name",  # Custom firewall rule name
    "123.123.123.123",  # Start ip range
    "167.220.0.235"  # End ip range
)

firewall_rule = rule_creation_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="afdb0-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="afdb0-119">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/mysql/management)

## <a name="postgresql"></a><span data-ttu-id="afdb0-120">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="afdb0-120">PostgreSQL</span></span>
<span data-ttu-id="afdb0-121">Usare il driver ODBC e pyodbc per connettersi al database ed eseguire direttamente le istruzioni SQL.</span><span class="sxs-lookup"><span data-stu-id="afdb0-121">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="afdb0-122">Altre informazioni sul [Database di Azure per PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span><span class="sxs-lookup"><span data-stu-id="afdb0-122">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

### <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="afdb0-123">Driver ODBC client e pyodbc</span><span class="sxs-lookup"><span data-stu-id="afdb0-123">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="afdb0-124">La libreria client consigliata per l'accesso al Database di Azure per PostgreSQL è il [driver ODBC Microsoft e pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span><span class="sxs-lookup"><span data-stu-id="afdb0-124">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

#### <a name="example"></a><span data-ttu-id="afdb0-125">Esempio</span><span class="sxs-lookup"><span data-stu-id="afdb0-125">Example</span></span> 

<span data-ttu-id="afdb0-126">Connettersi a un database di Azure per PostgreSQL e selezionare tutti i record della tabella `SALES`.</span><span class="sxs-lookup"><span data-stu-id="afdb0-126">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="afdb0-127">È possibile ottenere la stringa di connessione ODBC per il database dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="afdb0-127">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

```python
import pyodbc

SERVER = 'YOUR_SERVER_NAME.postgres.database.azure.com'
DATABASE = 'YOUR_DB_NAME'
USERNAME = 'YOUR_USERNAME'
PASSWORD = 'YOUR_PASSWORD'

DRIVER = '{PostgreSQL ODBC Driver}'
cnxn = pyodbc.connect(
    'DRIVER=' + DRIVER + ';PORT=5432;SERVER=' + SERVER +
    ';PORT=5432;DATABASE=' + DATABASE + ';UID=' + USERNAME +
    ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES" # SALES is an example table name
cursor.execute(selectsql)
```

### <a name="management-api"></a><span data-ttu-id="afdb0-128">API di gestione</span><span class="sxs-lookup"><span data-stu-id="afdb0-128">Management API</span></span>
#### <a name="requirements"></a><span data-ttu-id="afdb0-129">Requisiti</span><span class="sxs-lookup"><span data-stu-id="afdb0-129">Requirements</span></span>
<span data-ttu-id="afdb0-130">È necessario installare le librerie di gestione di PostgreSQL per Python.</span><span class="sxs-lookup"><span data-stu-id="afdb0-130">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="afdb0-131">Per informazioni dettagliate su come ottenere le credenziali per l'autenticazione nel client di gestione, vedere la pagina relativa all'[autenticazione di Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).</span><span class="sxs-lookup"><span data-stu-id="afdb0-131">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

#### <a name="example"></a><span data-ttu-id="afdb0-132">Esempio</span><span class="sxs-lookup"><span data-stu-id="afdb0-132">Example</span></span>
<span data-ttu-id="afdb0-133">In questo esempio verrà creato un nuovo database Postgres nel server Postgres esistente.</span><span class="sxs-lookup"><span data-stu-id="afdb0-133">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
```python
from azure.mgtm.rdbms.postgresql import PostgreSQLManagementClient

SUBSCRIPTION_ID = "YOUR_AZURE_SUBSCRIPTION_ID"
RESOURCE_GROUP = "YOUR_AZURE_RESOURCE_GROUP_WITH_POSTGRES"
POSTGRES_SERVER = "YOUR_POSTGRES_SERVER_NAME"
DB_NAME = "YOUR_DESIRED_DATABASE_NAME"

client = PostgreSQLManagementClient(credentials, SUBSCRIPTION_ID)

db_creation_poller = client.databases.create_or_update(
    resource_group_name=RESOURCE_GROUP,
    server_name=POSTGRES_SERVER, database_name=DB_NAME)
db = db_creation_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="afdb0-134">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="afdb0-134">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/mysql/management)
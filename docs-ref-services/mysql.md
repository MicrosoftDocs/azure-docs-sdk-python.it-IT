---
title: Librerie di Azure per MySQL per Python
description: 
keywords: Azure, Python, SDK, API, SQL, database, MySQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: mysql
ms.openlocfilehash: 2ea2ed06d4532f9c9366257e049856dcef73385e
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-mysql-libraries-for-python"></a><span data-ttu-id="9f1b7-103">Librerie di Azure per MySQL per Python</span><span class="sxs-lookup"><span data-stu-id="9f1b7-103">Azure MySQL libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="9f1b7-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9f1b7-104">Overview</span></span>

<span data-ttu-id="9f1b7-105">Usare le risorse e i dati archiviati nel [database MySQL di Azure](/azure/mysql/overview) da Python con lo strumento di gestione MySQL e pyodbc.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-105">Work with resources and data stored in [Azure MySQL Database](/azure/mysql/overview) from python with the MySQL manager and pyodbc.</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="9f1b7-106">Driver ODBC client e pyodbc</span><span class="sxs-lookup"><span data-stu-id="9f1b7-106">Client ODBC driver and pyodbc</span></span>

<span data-ttu-id="9f1b7-107">La libreria client consigliata per l'accesso al Database di Azure per MySQL è il [driver ODBC](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-107">The recommended client library for accessing Azure Database for MySQL is the Microsoft [ODBC driver](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).</span></span> <span data-ttu-id="9f1b7-108">Usare il driver ODBC per connettersi al database ed eseguire direttamente le istruzioni SQL.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-108">Use the ODBC driver to connect to the database and execute SQL statements directly.</span></span>

### <a name="example"></a><span data-ttu-id="9f1b7-109">Esempio</span><span class="sxs-lookup"><span data-stu-id="9f1b7-109">Example</span></span>

<span data-ttu-id="9f1b7-110">Connettersi a un database di Azure per MySQL e selezionare tutti i record nella tabella relativa alle vendite.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-110">Connect to a Azure Database for MySQL and select all records in the sales table.</span></span> <span data-ttu-id="9f1b7-111">È possibile ottenere la stringa di connessione ODBC per il database dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="9f1b7-112">API di gestione</span><span class="sxs-lookup"><span data-stu-id="9f1b7-112">Management API</span></span>

<span data-ttu-id="9f1b7-113">Creare e gestire le risorse di MySQL nella sottoscrizione con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-113">Create and manage MySQL resources in your subscription with the management API.</span></span>

### <a name="requirements"></a><span data-ttu-id="9f1b7-114">Requisiti</span><span class="sxs-lookup"><span data-stu-id="9f1b7-114">Requirements</span></span>
<span data-ttu-id="9f1b7-115">È necessario installare le librerie di gestione di MySQL per Python.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-115">You must install the MySQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="9f1b7-116">Per informazioni dettagliate su come ottenere le credenziali per l'autenticazione nel client di gestione, vedere la pagina relativa all'[autenticazione di Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).</span><span class="sxs-lookup"><span data-stu-id="9f1b7-116">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="9f1b7-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="9f1b7-117">Example</span></span>

<span data-ttu-id="9f1b7-118">Creare una risorsa del database MySQL 5.7 e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.</span><span class="sxs-lookup"><span data-stu-id="9f1b7-118">Create a MySQL 5.7 Database resource and restrict access to a range of IP addresses using a firewall rule.</span></span>

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
> [<span data-ttu-id="9f1b7-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="9f1b7-119">Explore the Management APIs</span></span>](/python/api/overview/azure/mysql/managementlibrary)
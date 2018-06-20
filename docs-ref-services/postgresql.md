---
title: Librerie di Azure per PostgreSQL per Python
description: ''
keywords: Azure, Python, SDK, API, SQL, database, Postgres, PostgreSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: postgresql
ms.openlocfilehash: cad5995072d5040764986765d9a900f46f5141ec
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478934"
---
#<a name="azure-postgresql-libraries-for-python"></a><span data-ttu-id="12078-103">Librerie di Azure per PostgreSQL per Python</span><span class="sxs-lookup"><span data-stu-id="12078-103">Azure PostgreSQL libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="12078-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="12078-104">Overview</span></span>
<span data-ttu-id="12078-105">Usare il driver ODBC e pyodbc per connettersi al database ed eseguire direttamente le istruzioni SQL.</span><span class="sxs-lookup"><span data-stu-id="12078-105">Use the ODBC driver and pyodbc to connect to the database and execute SQL statements directly.</span></span>

<span data-ttu-id="12078-106">Altre informazioni sul [Database di Azure per PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span><span class="sxs-lookup"><span data-stu-id="12078-106">Learn more about [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/).</span></span>

## <a name="client-odbc-driver-and-pyodbc"></a><span data-ttu-id="12078-107">Driver ODBC client e pyodbc</span><span class="sxs-lookup"><span data-stu-id="12078-107">Client ODBC driver and pyodbc</span></span>
<span data-ttu-id="12078-108">La libreria client consigliata per l'accesso al Database di Azure per PostgreSQL è il [driver ODBC Microsoft e pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span><span class="sxs-lookup"><span data-stu-id="12078-108">The recommended client library for accessing Azure Database for PostgreSQL is the Microsoft [ODBC driver and pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)</span></span>

### <a name="example"></a><span data-ttu-id="12078-109">Esempio</span><span class="sxs-lookup"><span data-stu-id="12078-109">Example</span></span> 

<span data-ttu-id="12078-110">Connettersi a un database di Azure per PostgreSQL e selezionare tutti i record della tabella `SALES`.</span><span class="sxs-lookup"><span data-stu-id="12078-110">Connect to a Azure Database for PostgreSQL and select all records in the `SALES` table.</span></span> <span data-ttu-id="12078-111">È possibile ottenere la stringa di connessione ODBC per il database dal portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="12078-111">You can get the ODBC connection string for the database from the Azure Portal.</span></span>

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

## <a name="management-api"></a><span data-ttu-id="12078-112">API di gestione</span><span class="sxs-lookup"><span data-stu-id="12078-112">Management API</span></span>
### <a name="requirements"></a><span data-ttu-id="12078-113">Requisiti</span><span class="sxs-lookup"><span data-stu-id="12078-113">Requirements</span></span>
<span data-ttu-id="12078-114">È necessario installare le librerie di gestione di PostgreSQL per Python.</span><span class="sxs-lookup"><span data-stu-id="12078-114">You must install the PostgreSQL management libraries for Python.</span></span>
```bash
pip install azure-mgmt-rdbms
```

<span data-ttu-id="12078-115">Per informazioni dettagliate su come ottenere le credenziali per l'autenticazione nel client di gestione, vedere la pagina relativa all'[autenticazione di Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).</span><span class="sxs-lookup"><span data-stu-id="12078-115">Please see the [Python SDK authentication](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate) page for details on obtaining credentials to authenticate with the management client.</span></span>

### <a name="example"></a><span data-ttu-id="12078-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="12078-116">Example</span></span>
<span data-ttu-id="12078-117">In questo esempio verrà creato un nuovo database Postgres nel server Postgres esistente.</span><span class="sxs-lookup"><span data-stu-id="12078-117">In this example we will create a new Postgres database on our existing Postgres server.</span></span>
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
> [<span data-ttu-id="12078-118">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="12078-118">Explore the Management APIs</span></span>](/python/api/overview/azure/postgresql/management)


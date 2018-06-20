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
#<a name="azure-postgresql-libraries-for-python"></a>Librerie di Azure per PostgreSQL per Python

## <a name="overview"></a>Panoramica
Usare il driver ODBC e pyodbc per connettersi al database ed eseguire direttamente le istruzioni SQL.

Altre informazioni sul [Database di Azure per PostgreSQL](https://docs.microsoft.com/azure/postgresql/).

## <a name="client-odbc-driver-and-pyodbc"></a>Driver ODBC client e pyodbc
La libreria client consigliata per l'accesso al Database di Azure per PostgreSQL è il [driver ODBC Microsoft e pyodbc](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries)

### <a name="example"></a>Esempio 

Connettersi a un database di Azure per PostgreSQL e selezionare tutti i record della tabella `SALES`. È possibile ottenere la stringa di connessione ODBC per il database dal portale di Azure.

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

## <a name="management-api"></a>API di gestione
### <a name="requirements"></a>Requisiti
È necessario installare le librerie di gestione di PostgreSQL per Python.
```bash
pip install azure-mgmt-rdbms
```

Per informazioni dettagliate su come ottenere le credenziali per l'autenticazione nel client di gestione, vedere la pagina relativa all'[autenticazione di Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).

### <a name="example"></a>Esempio
In questo esempio verrà creato un nuovo database Postgres nel server Postgres esistente.
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
> [Esplorare le API di gestione](/python/api/overview/azure/postgresql/management)


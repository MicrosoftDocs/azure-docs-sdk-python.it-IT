---
title: Librerie del database SQL di Azure per Python
description: 
keywords: Azure, Python, SDK, API, SQL, database , pyodbc
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: sql-database
ms.openlocfilehash: b580c5011412bc77fd8fd55b709a305be07e2316
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-sql-database-libraries-for-python"></a>Librerie del database SQL di Azure per Python

## <a name="overview"></a>Panoramica

Usare i dati archiviati nel [database SQL di Azure](/azure/sql-database/sql-database-technical-overview) da Python con il driver Microsoft ODBC e pyodbc. 

## <a name="client-odbc-driver-and-pyodbc"></a>Driver ODBC client e pyodbc

```bash
pip install pyodbc
```
Altri dettagli sull'installazione delle librerie di comunicazione Python e dei database sono disponibili [qui](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries).

### <a name="example"></a>Esempio

Connettersi a un database SQL e selezionare tutti i record di una tabella.

```python
import pyodbc 

SERVER = 'YOUR_SERVER_NAME.database.windows.net'
DATABASE = 'YOUR_DATABASE_NAME'
USERNAME = 'YOUR_DB_USERNAME'
PASSWORD = 'YOUR_DB_PASSWORD'

DRIVER= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER=' + DRIVER + ';PORT=1433;SERVER=' + SERVER +
    ';PORT=1443;DATABASE=' + DATABASE + ';UID=' + USERNAME + ';PWD=' + PASSWORD)
cursor = cnxn.cursor()
selectsql = "SELECT * FROM SALES"  # SALES is an example table name
cursor.execute(selectsql)
```

## <a name="management-api"></a>API di gestione

Creare e gestire le risorse del database SQL di Azure nella sottoscrizione con l'API di gestione. 

```bash
pip install azure-mgmt-sql
```

### <a name="example"></a>Esempio

Creare una risorsa del database SQL e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.

```python
RESOURCE_GROUP = 'YOUR_RESOURCE_GROUP_NAME'
LOCATION = 'eastus'  # example Azure availability zone, should match resource group
SQL_DB = 'YOUR_SQLDB_NAME'

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

## <a name="samples"></a>Esempi

* [Creare e gestire database SQL][1]    
* [Usare Python per connettersi ai dati ed eseguire query][2]   

[1]: https://github.com/Azure-Samples/sql-database-python-manage
[2]: https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python

Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=SQL) degli esempi di codice per il database SQL di Azure. 
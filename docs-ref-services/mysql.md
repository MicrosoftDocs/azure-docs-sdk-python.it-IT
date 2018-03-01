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
ms.openlocfilehash: f03134bfddfabc426cbcaf4d98ef86d14038861f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
---
# <a name="azure-mysql-libraries-for-python"></a>Librerie di Azure per MySQL per Python 

## <a name="overview"></a>Panoramica

Usare le risorse e i dati archiviati nel [database MySQL di Azure](/azure/mysql/overview) da Python con lo strumento di gestione MySQL e pyodbc.

## <a name="client-odbc-driver-and-pyodbc"></a>Driver ODBC client e pyodbc

La libreria client consigliata per l'accesso al Database di Azure per MySQL è il [driver ODBC](/azure/sql-database/sql-database-connect-query-python#install-the-python-and-database-communication-libraries) Microsoft. Usare il driver ODBC per connettersi al database ed eseguire direttamente le istruzioni SQL.

### <a name="example"></a>Esempio

Connettersi a un database di Azure per MySQL e selezionare tutti i record nella tabella relativa alle vendite. È possibile ottenere la stringa di connessione ODBC per il database dal portale di Azure.

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

## <a name="management-api"></a>API di gestione

Creare e gestire le risorse di MySQL nella sottoscrizione con l'API di gestione.

### <a name="requirements"></a>Requisiti
È necessario installare le librerie di gestione di MySQL per Python.
```bash
pip install azure-mgmt-rdbms
```

Per informazioni dettagliate su come ottenere le credenziali per l'autenticazione nel client di gestione, vedere la pagina relativa all'[autenticazione di Python SDK](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).

### <a name="example"></a>Esempio

Creare una risorsa del database MySQL 5.7 e limitare l'accesso a un intervallo di indirizzi IP usando una regola del firewall.

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
> [Esplorare le API di gestione](/python/api/overview/azure/mysql/management)
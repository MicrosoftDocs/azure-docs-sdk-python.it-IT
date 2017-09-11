---
title: Introduzione alle librerie di Azure per Python
description: Introduzione alle librerie di Azure per Python
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: get-started
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 848ca9dc40000e68e5e3cea3af8b8a0d22747881
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-the-azure-libraries-for-python"></a>Introduzione alle librerie di Azure per Python

Questa guida illustra l'utilizzo di alcune librerie di Azure per Python.  Verranno eseguite operazioni per la configurazione dell'autenticazione, la creazione e l'uso di un account di archiviazione di Azure, la creazione e l'uso di un database SQL di Azure, la distribuzione di alcune macchine virtuali e la distribuzione di un'app Web del Servizio app di Azure da GitHub.

## <a name="prerequisites"></a>Prerequisiti

- Un account Azure. Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
- [Python](https://www.python.org/downloads/)
- [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

[!INCLUDE [azure-cloud-shell](../docs-ref-conceptual/includes/cloud-shell-try-it.md)]

## <a name="set-up-authentication"></a>Configurare l'autenticazione
> [!IMPORTANT]
> Questo approccio è consigliato come esperienza di avvio rapido per gli sviluppatori. Per finalità di produzione, usare [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) o il proprio sistema di credenziali.
> Eventuali modifiche apportate alla configurazione dell'interfaccia della riga di comando influiranno sull'esecuzione dell'SDK.

L'SDK consente di creare un client tramite la sottoscrizione attiva dell'interfaccia della riga di comando.

Per definire le credenziali attive, usare [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).
L'ID sottoscrizione predefinito corrisponde a quello disponibile oppure è possibile definirlo usando [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli).

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
## <a name="create-a-virtual-environment"></a>Creare un ambiente virtuale

> [!IMPORTANT]
> La creazione di un ambiente virtuale è facoltativa, ma consigliata.

Creare un ambiente virtuale in Bash (Linux o [Bash per Windows](https://msdn.microsoft.com/commandline/wsl/about)):
```bash
pip install virtualenv
virtualenv mytestenv
cd mytestenv
source ./bin/activate
```

Creare un ambiente virtuale in Powershell:
```powershell
pip install virtualenv
virtualenv mytestenv
cd mytestenv
.\Scripts\activate
```

> [!IMPORTANT]
> Si noti che se si usa [VSCode](https://code.visualstudio.com/) e l'[estensione Python](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python), è possibile avviarla con `code .` e ottenere la configurazione dell'ambiente.

## <a name="create-a-linux-virtual-machine"></a>Creare una macchina virtuale Linux
Questo codice crea una nuova VM Linux denominata `testLinuxVM` in un gruppo di risorse `sampleVmResourceGroup` in esecuzione nell'area di Azure Stati Uniti orientali.

Autenticazione:
```azcli
az login
```
Creare un gruppo di risorse:
```azurecli-interactive
az group create -l eastus --n sampleVmResourceGroup
```

Creare una rete virtuale e una subnet:
```azurecli-interactive
az network vnet create -g sampleVmResourceGroup -n azure-sample-vnet --address-prefix 10.0.0.0/16 --subnet-name azure-sample-subnet --subnet-prefix 10.0.0.0/24
```

Creare un indirizzo IP pubblico:
```azurecli-interactive
az network public-ip create -g sampleVmResourceGroup -n azure-sample-ip --allocation-method Dynamic --version IPv6
```
Creare un client di interfaccia di rete:
```azurecli-interactive
az network nic create -g sampleVmResourceGroup --vnet-name azure-sample-vnet --subnet azure-sample-subnet -n azure-sample-nic --public-ip-address azure-sample-ip
```

```python
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
from azure.common.client_factory import get_client_from_cli_profile

# Azure Datacenter
LOCATION = 'eastus'

# Resource Group
GROUP_NAME = 'sampleVmResourceGroup'

# Network
VNET_NAME = 'azure-sample-vnet'
SUBNET_NAME = 'azure-sample-subnet'

# VM
NIC_NAME = 'azure-sample-nic'
VM_NAME = 'testLinuxVM'

USERNAME = 'userlogin'
PASSWORD = 'Pa$$w0rd91'

IP_ADDRESS_NAME='azure-sample-ip'

VM_REFERENCE = {
    'linux': {
        'publisher': 'Canonical',
        'offer': 'UbuntuServer',
        'sku': '16.04.0-LTS',
        'version': 'latest'
    },
    'windows': {
        'publisher': 'MicrosoftWindowsServerEssentials',
        'offer': 'WindowsServerEssentials',
        'sku': 'WindowsServerEssentials',
        'version': 'latest'
    }
}


def run_example():

    compute_client = get_client_from_cli_profile(ComputeManagementClient)
    network_client = get_client_from_cli_profile(NetworkManagementClient)

    # get nic id
    nic = network_client.network_interfaces.get(GROUP_NAME, NIC_NAME)

    # Create Linux VM
    print('\nCreating Linux Virtual Machine')
    vm_parameters = create_vm_parameters(nic.id, VM_REFERENCE['linux'])
    print(vm_parameters)
    async_vm_creation = compute_client.virtual_machines.create_or_update(
        GROUP_NAME, VM_NAME, vm_parameters)
    async_vm_creation.wait()


def create_vm_parameters(nic_id, vm_reference):
    """Create the VM parameters structure.
    """
    return {
        'location': LOCATION,
        'os_profile': {
            'computer_name': VM_NAME,
            'admin_username': USERNAME,
            'admin_password': PASSWORD
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': vm_reference['publisher'],
                'offer': vm_reference['offer'],
                'sku': vm_reference['sku'],
                'version': vm_reference['version']
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': nic_id,
            }]
        },
    }


if __name__ == "__main__":
    run_example()
```

Al termine del programma, verificare la macchina virtuale nella sottoscrizione con l'interfaccia della riga di comando di Azure 2.0:

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare la VM e le risorse.

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a>Distribuire un'app Web da un repository di GitHub
Questo codice distribuisce un'applicazione Web Flask dal ramo `master` in un repository GitHub in una nuova [app Web del servizio app di Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) in esecuzione nel livello gratuito. 

Prima di iniziare, creare una copia tramite fork di https://github.com/Azure-Samples/python-docs-hello-world

Autenticazione:
```azcli
az login
```
Creare un gruppo di risorse:
```azurecli-interactive
az group create -l eastus -n sampleWebResourceGroup
```

Creare un piano di servizio app gratuito:
```azurecli-interactive
az appservice plan create -g sampleWebResourceGroup -n sampleFreePlan  --sku Free
```

```python
from azure.mgmt.web import WebSiteManagementClient
from azure.mgmt.web.models import Site, SiteSourceControl, SiteConfig
from azure.common.client_factory import get_client_from_cli_profile

RESOURCE_GROUP_NAME = 'sampleWebResourceGroup'
SERVICE_PLAN_NAME = 'sampleFreePlanName'
WEB_APP_NAME = 'sampleflaskapp123'
REPO_URL = 'GitHub_Repository_URL'

#log in
web_client = get_client_from_cli_profile(WebSiteManagementClient)

# get service plan id
service_plan = web_client.app_service_plans.get(RESOURCE_GROUP_NAME, SERVICE_PLAN_NAME)

# create a web app
siteConfiguration = SiteConfig(
    python_version='3.4',
)
site_async_operation = web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=service_plan.id,
        site_config=siteConfiguration
    ),
)

site = site_async_operation.result()
print('created webapp: ' + site.default_host_name)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url= REPO_URL,
        branch='master'
    )
)

source_control = source_control_async_operation.result()
print("added source control to: " + source_control.name + "azurewebsites.net")
```

Aprire un browser che punta all'applicazione usando l'interfaccia della riga di comando:
```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

Rimuovere l'app Web e il piano dalla sottoscrizione dopo avere verificato la distribuzione. 
```azurecli-interactive
az group delete --name sampleWebResourceGroup
```


## <a name="connect-to-a-sql-database"></a>Connettersi al database SQL
Questo codice crea un nuovo database SQL con una regola del firewall che consente l'accesso remoto e quindi vi si connette usando il driver Microsoft ODBC. Pyodbc usa il driver ODBC di Microsoft su Linux per connettersi ai database SQL. 

Autenticazione:
```azcli
az login
```
Creare un gruppo di risorse:
```azurecli-interactive
az group create -l eastus -n azure-sample-group
```

Creare un server SQL:
```azurecli-interactive
az sql server create --admin-password HusH_Sec4et --admin-user mysecretname -l eastus -n samplesqlserver123 -g azure-sample-group
```

Aggiungere una regola del firewall:
```azurecli-interactive
az sql server firewall-rule create --end-ip-address 167.220.0.235 --name firewall_rule_name_123.123.123.123 --resource-group azure-sample-group --server samplesqlserver123 --start-ip-address 123.123.123.123
```

Creare un database SQL:
```azurecli-interactive
az sql db create --name sample-db --resource-group azure-sample-group --server samplesqlserver123
```


```python
from azure.mgmt.sql import SqlManagementClient
from azure.common.client_factory import get_client_from_cli_profile
import pyodbc

LOCATION = 'eastus'
GROUP_NAME = 'azure-sample-group'
SERVER_NAME = 'samplesqlserver123'
DATABASE_NAME = 'sample-db'
USER_NAME ='mysecretname'
PASSWORD='HusH_Sec4et'

# authenticate
sql_client = get_client_from_cli_profile(SqlManagementClient)


def run_example():
    # Get SQL database
    print('Get SQL database')
    database = sql_client.databases.get(
        GROUP_NAME,
        SERVER_NAME,
        DATABASE_NAME
    )
    print(database)
    print("\n\n")


def create_and_insert():
    server = SERVER_NAME+'.database.windows.net'
    database = DATABASE_NAME
    username = USER_NAME
    password = PASSWORD
    driver = '{ODBC Driver 13 for SQL Server}'
    cnxn = pyodbc.connect(
        'DRIVER=' + driver + ';PORT=1433;SERVER=' + server + ';PORT=1443;DATABASE=' + database + ';UID=' + username + ';PWD=' + password)
    cursor = cnxn.cursor()
    tsql = "CREATE TABLE CLOUD (name varchar(255), code int);"
    tsqlInsert = "INSERT INTO CLOUD (name, code) VALUES ('Azure', 1); "
    selectValues = "SELECT * FROM CLOUD"
    with cursor.execute(tsql):
        print('Successfully created table!')
    with cursor.execute(tsqlInsert):
        print('Successfully inserted data into table')
    cursor.execute(selectValues)
    row = cursor.fetchone()
    while row:
        print(str(row[0]) + " " + str(row[1]))
        row = cursor.fetchone()
    cnxn.commit()

if __name__ == "__main__":
    run_example()
    create_and_insert()
```

Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare il database SQL e le rispettive risorse.

```azurecli-interactive
az group delete --name azure-sample-group
```

## <a name="write-a-blob-into-a-new-storage-account"></a>Scrivere un BLOB in un nuovo account di archiviazione

Autenticazione:
```azcli
az login
```
Creare un gruppo di risorse:
```azurecli-interactive
az group create -l eastus -n sampleStorageResourceGroup
```

Creare un account di archiviazione:
```azurecli-interactive
az storage account create -n samplestorageaccountname -g sampleStorageResourceGroup -l eastus --sku Standard_RAGRS
```

Questo codice crea un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e quindi usa le librerie di archiviazione di Azure per Python per creare un nuovo file HTML nel cloud. 
```python
from azure.storage import CloudStorageAccount
from azure.storage.blob import PublicAccess
from azure.storage.blob.models import ContentSettings
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.storage import StorageManagementClient

RESOURCE_GROUP = 'sampleStorageResourceGroup'
STORAGE_ACCOUNT_NAME = 'samplestorageaccountname'
CONTAINER_NAME = 'samplecontainername'

# log in
storage_client = get_client_from_cli_profile(StorageManagementClient)

# create a public storage container to hold the file
storage_keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP, STORAGE_ACCOUNT_NAME)
storage_keys = {v.key_name: v.value for v in storage_keys.keys}

storage_client = CloudStorageAccount(STORAGE_ACCOUNT_NAME, storage_keys['key1'])
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(CONTAINER_NAME, public_access=PublicAccess.Container)

blob_service.create_blob_from_bytes(
    CONTAINER_NAME,
    'helloworld.html',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url(CONTAINER_NAME, 'helloworld.html'))
```
Per verificare che il contenuto sia stato caricato correttamente, passare all'URL stampato. 

Pulire l'account di archiviazione usando l'interfaccia della riga di comando:
```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a>Esplorare altri esempi

Per altre informazioni su come usare le librerie di gestione di Azure per Python per gestire le risorse e l'automazione delle attività, vedere il codice di esempio per [macchine virtuali](python-sdk-azure-web-apps-samples.md), [app Web](python-sdk-azure-web-apps-samples.md) e [database SQL](python-sdk-azure-sql-database-samples.md).


## <a name="reference"></a>riferimento

Le [informazioni di riferimento](/python/api/overview/azure/?view=azure-python) sono disponibili per tutti i pacchetti.

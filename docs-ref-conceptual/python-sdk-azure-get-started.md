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
# <a name="get-started-with-the-azure-libraries-for-python"></a><span data-ttu-id="265e9-104">Introduzione alle librerie di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="265e9-104">Get started with the Azure libraries for Python</span></span>

<span data-ttu-id="265e9-105">Questa guida illustra l'utilizzo di alcune librerie di Azure per Python.</span><span class="sxs-lookup"><span data-stu-id="265e9-105">This guide demonstrates the usage of several Azure libraries for Python.</span></span>  <span data-ttu-id="265e9-106">Verranno eseguite operazioni per la configurazione dell'autenticazione, la creazione e l'uso di un account di archiviazione di Azure, la creazione e l'uso di un database SQL di Azure, la distribuzione di alcune macchine virtuali e la distribuzione di un'app Web del Servizio app di Azure da GitHub.</span><span class="sxs-lookup"><span data-stu-id="265e9-106">You will set up authentication, create and use an Azure Storage account, create and use an Azure SQL Database, deploy some virtual machines, and deploy an Azure App Service Web App from GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="265e9-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="265e9-107">Prerequisites</span></span>

- <span data-ttu-id="265e9-108">Un account Azure.</span><span class="sxs-lookup"><span data-stu-id="265e9-108">An Azure account.</span></span> <span data-ttu-id="265e9-109">Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="265e9-109">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
- [<span data-ttu-id="265e9-110">Python</span><span class="sxs-lookup"><span data-stu-id="265e9-110">Python</span></span>](https://www.python.org/downloads/)
- <span data-ttu-id="265e9-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) o [interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="265e9-111">[Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) or [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

[!INCLUDE [azure-cloud-shell](../docs-ref-conceptual/includes/cloud-shell-try-it.md)]

## <a name="set-up-authentication"></a><span data-ttu-id="265e9-112">Configurare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="265e9-112">Set up authentication</span></span>
> [!IMPORTANT]
> <span data-ttu-id="265e9-113">Questo approccio è consigliato come esperienza di avvio rapido per gli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="265e9-113">This should be used as quick start developer experience.</span></span> <span data-ttu-id="265e9-114">Per finalità di produzione, usare [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) o il proprio sistema di credenziali.</span><span class="sxs-lookup"><span data-stu-id="265e9-114">For production purposes, use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) or your own credentials system.</span></span>
> <span data-ttu-id="265e9-115">Eventuali modifiche apportate alla configurazione dell'interfaccia della riga di comando influiranno sull'esecuzione dell'SDK.</span><span class="sxs-lookup"><span data-stu-id="265e9-115">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="265e9-116">L'SDK consente di creare un client tramite la sottoscrizione attiva dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="265e9-116">The SDK is able to create a client using your CLI active subscription.</span></span>

<span data-ttu-id="265e9-117">Per definire le credenziali attive, usare [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="265e9-117">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="265e9-118">L'ID sottoscrizione predefinito corrisponde a quello disponibile oppure è possibile definirlo usando [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="265e9-118">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli).</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
## <a name="create-a-virtual-environment"></a><span data-ttu-id="265e9-119">Creare un ambiente virtuale</span><span class="sxs-lookup"><span data-stu-id="265e9-119">Create a virtual environment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="265e9-120">La creazione di un ambiente virtuale è facoltativa, ma consigliata.</span><span class="sxs-lookup"><span data-stu-id="265e9-120">Create a virtual environment is optional, but we strongly suggest you to do it.</span></span>

<span data-ttu-id="265e9-121">Creare un ambiente virtuale in Bash (Linux o [Bash per Windows](https://msdn.microsoft.com/commandline/wsl/about)):</span><span class="sxs-lookup"><span data-stu-id="265e9-121">Create a virtual environment in Bash (Linux or [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about)):</span></span>
```bash
pip install virtualenv
virtualenv mytestenv
cd mytestenv
source ./bin/activate
```

<span data-ttu-id="265e9-122">Creare un ambiente virtuale in Powershell:</span><span class="sxs-lookup"><span data-stu-id="265e9-122">Create a virtual environment in Powershell:</span></span>
```powershell
pip install virtualenv
virtualenv mytestenv
cd mytestenv
.\Scripts\activate
```

> [!IMPORTANT]
> <span data-ttu-id="265e9-123">Si noti che se si usa [VSCode](https://code.visualstudio.com/) e l'[estensione Python](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python), è possibile avviarla con `code .` e ottenere la configurazione dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="265e9-123">Note that if you use [VSCode](https://code.visualstudio.com/) and the [Python extension](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python),  you can start it using `code .` and get your environment configured.</span></span>

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="265e9-124">Creare una macchina virtuale Linux</span><span class="sxs-lookup"><span data-stu-id="265e9-124">Create a Linux virtual machine</span></span>
<span data-ttu-id="265e9-125">Questo codice crea una nuova VM Linux denominata `testLinuxVM` in un gruppo di risorse `sampleVmResourceGroup` in esecuzione nell'area di Azure Stati Uniti orientali.</span><span class="sxs-lookup"><span data-stu-id="265e9-125">This code creates a new Linux VM with name `testLinuxVM` in a resource group `sampleVmResourceGroup` running in the US East Azure region.</span></span>

<span data-ttu-id="265e9-126">Autenticazione:</span><span class="sxs-lookup"><span data-stu-id="265e9-126">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="265e9-127">Creare un gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="265e9-127">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus --n sampleVmResourceGroup
```

<span data-ttu-id="265e9-128">Creare una rete virtuale e una subnet:</span><span class="sxs-lookup"><span data-stu-id="265e9-128">Create a virtual network and subnet:</span></span>
```azurecli-interactive
az network vnet create -g sampleVmResourceGroup -n azure-sample-vnet --address-prefix 10.0.0.0/16 --subnet-name azure-sample-subnet --subnet-prefix 10.0.0.0/24
```

<span data-ttu-id="265e9-129">Creare un indirizzo IP pubblico:</span><span class="sxs-lookup"><span data-stu-id="265e9-129">Create a public IP address:</span></span>
```azurecli-interactive
az network public-ip create -g sampleVmResourceGroup -n azure-sample-ip --allocation-method Dynamic --version IPv6
```
<span data-ttu-id="265e9-130">Creare un client di interfaccia di rete:</span><span class="sxs-lookup"><span data-stu-id="265e9-130">Create a network interface client:</span></span>
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

<span data-ttu-id="265e9-131">Al termine del programma, verificare la macchina virtuale nella sottoscrizione con l'interfaccia della riga di comando di Azure 2.0:</span><span class="sxs-lookup"><span data-stu-id="265e9-131">When the program finishes, verify the virtual machine in your subscription with the Azure CLI 2.0:</span></span>

```azurecli-interactive
az vm list --resource-group sampleVmResourceGroup
```

<span data-ttu-id="265e9-132">Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare la VM e le risorse.</span><span class="sxs-lookup"><span data-stu-id="265e9-132">Once you've verified that the code worked, use the CLI to delete the VM and its resources.</span></span>

```azurecli-interactive
az group delete --name sampleVmResourceGroup
```

## <a name="deploy-a-web-app-from-a-github-repo"></a><span data-ttu-id="265e9-133">Distribuire un'app Web da un repository di GitHub</span><span class="sxs-lookup"><span data-stu-id="265e9-133">Deploy a web app from a GitHub repo</span></span>
<span data-ttu-id="265e9-134">Questo codice distribuisce un'applicazione Web Flask dal ramo `master` in un repository GitHub in una nuova [app Web del servizio app di Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) in esecuzione nel livello gratuito.</span><span class="sxs-lookup"><span data-stu-id="265e9-134">This code deploys a Flask web application from the `master` branch in a GitHub repo in to a new [Azure App Service Web App](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) running in the free tier.</span></span> 

<span data-ttu-id="265e9-135">Prima di iniziare, creare una copia tramite fork di https://github.com/Azure-Samples/python-docs-hello-world</span><span class="sxs-lookup"><span data-stu-id="265e9-135">Before you begin: Fork https://github.com/Azure-Samples/python-docs-hello-world</span></span>

<span data-ttu-id="265e9-136">Autenticazione:</span><span class="sxs-lookup"><span data-stu-id="265e9-136">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="265e9-137">Creare un gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="265e9-137">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus -n sampleWebResourceGroup
```

<span data-ttu-id="265e9-138">Creare un piano di servizio app gratuito:</span><span class="sxs-lookup"><span data-stu-id="265e9-138">Create a free app service plan:</span></span>
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

<span data-ttu-id="265e9-139">Aprire un browser che punta all'applicazione usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="265e9-139">Open a browser pointed to the application using the CLI:</span></span>
```azurecli-interactive
az appservice web browse --resource-group sampleWebResourceGroup --name YOUR_APP_NAME
```

<span data-ttu-id="265e9-140">Rimuovere l'app Web e il piano dalla sottoscrizione dopo avere verificato la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="265e9-140">Remove the web app and plan from your subscription once you've verified the deployment.</span></span> 
```azurecli-interactive
az group delete --name sampleWebResourceGroup
```


## <a name="connect-to-a-sql-database"></a><span data-ttu-id="265e9-141">Connettersi al database SQL</span><span class="sxs-lookup"><span data-stu-id="265e9-141">Connect to a SQL database</span></span>
<span data-ttu-id="265e9-142">Questo codice crea un nuovo database SQL con una regola del firewall che consente l'accesso remoto e quindi vi si connette usando il driver Microsoft ODBC.</span><span class="sxs-lookup"><span data-stu-id="265e9-142">This code creates a new SQL database with a firewall rule allowing remote access, and then connected to it using the Microsoft ODBC driver.</span></span> <span data-ttu-id="265e9-143">Pyodbc usa il driver ODBC di Microsoft su Linux per connettersi ai database SQL.</span><span class="sxs-lookup"><span data-stu-id="265e9-143">Pyodbc uses the Microsoft ODBC Driver on Linux to connect to SQL Databases.</span></span> 

<span data-ttu-id="265e9-144">Autenticazione:</span><span class="sxs-lookup"><span data-stu-id="265e9-144">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="265e9-145">Creare un gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="265e9-145">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus -n azure-sample-group
```

<span data-ttu-id="265e9-146">Creare un server SQL:</span><span class="sxs-lookup"><span data-stu-id="265e9-146">Create a SQL server:</span></span>
```azurecli-interactive
az sql server create --admin-password HusH_Sec4et --admin-user mysecretname -l eastus -n samplesqlserver123 -g azure-sample-group
```

<span data-ttu-id="265e9-147">Aggiungere una regola del firewall:</span><span class="sxs-lookup"><span data-stu-id="265e9-147">Add firewall rule:</span></span>
```azurecli-interactive
az sql server firewall-rule create --end-ip-address 167.220.0.235 --name firewall_rule_name_123.123.123.123 --resource-group azure-sample-group --server samplesqlserver123 --start-ip-address 123.123.123.123
```

<span data-ttu-id="265e9-148">Creare un database SQL:</span><span class="sxs-lookup"><span data-stu-id="265e9-148">Create a SQL database:</span></span>
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

<span data-ttu-id="265e9-149">Dopo avere verificato che il codice abbia funzionato, usare l'interfaccia della riga di comando per eliminare il database SQL e le rispettive risorse.</span><span class="sxs-lookup"><span data-stu-id="265e9-149">Once you've verified that the code worked, use the CLI to delete the SQL database and its resources.</span></span>

```azurecli-interactive
az group delete --name azure-sample-group
```

## <a name="write-a-blob-into-a-new-storage-account"></a><span data-ttu-id="265e9-150">Scrivere un BLOB in un nuovo account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="265e9-150">Write a blob into a new storage account</span></span>

<span data-ttu-id="265e9-151">Autenticazione:</span><span class="sxs-lookup"><span data-stu-id="265e9-151">Authenticate:</span></span>
```azcli
az login
```
<span data-ttu-id="265e9-152">Creare un gruppo di risorse:</span><span class="sxs-lookup"><span data-stu-id="265e9-152">Create a resource group:</span></span>
```azurecli-interactive
az group create -l eastus -n sampleStorageResourceGroup
```

<span data-ttu-id="265e9-153">Creare un account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="265e9-153">Create a storage account:</span></span>
```azurecli-interactive
az storage account create -n samplestorageaccountname -g sampleStorageResourceGroup -l eastus --sku Standard_RAGRS
```

<span data-ttu-id="265e9-154">Questo codice crea un [account di archiviazione di Azure](https://docs.microsoft.com/azure/storage/storage-introduction) e quindi usa le librerie di archiviazione di Azure per Python per creare un nuovo file HTML nel cloud.</span><span class="sxs-lookup"><span data-stu-id="265e9-154">This code creates an [Azure storage account](https://docs.microsoft.com/azure/storage/storage-introduction) and then uses the Azure Storage libraries for Python to create a new html file in the cloud.</span></span> 
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
<span data-ttu-id="265e9-155">Per verificare che il contenuto sia stato caricato correttamente, passare all'URL stampato.</span><span class="sxs-lookup"><span data-stu-id="265e9-155">To verify content successfully uploaded, navigate to the url printed.</span></span> 

<span data-ttu-id="265e9-156">Pulire l'account di archiviazione usando l'interfaccia della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="265e9-156">Clean up the storage account using the CLI:</span></span>
```azurecli-interactive
az group delete --name sampleStorageResourceGroup
```

## <a name="explore-more-samples"></a><span data-ttu-id="265e9-157">Esplorare altri esempi</span><span class="sxs-lookup"><span data-stu-id="265e9-157">Explore more samples</span></span>

<span data-ttu-id="265e9-158">Per altre informazioni su come usare le librerie di gestione di Azure per Python per gestire le risorse e l'automazione delle attività, vedere il codice di esempio per [macchine virtuali](python-sdk-azure-web-apps-samples.md), [app Web](python-sdk-azure-web-apps-samples.md) e [database SQL](python-sdk-azure-sql-database-samples.md).</span><span class="sxs-lookup"><span data-stu-id="265e9-158">To learn more about how to use the Azure management libraries for Python to manage resources and automate tasks, see our sample code for [virtual machines](python-sdk-azure-web-apps-samples.md), [web apps](python-sdk-azure-web-apps-samples.md) and [SQL database](python-sdk-azure-sql-database-samples.md).</span></span>


## <a name="reference"></a><span data-ttu-id="265e9-159">riferimento</span><span class="sxs-lookup"><span data-stu-id="265e9-159">Reference</span></span>

<span data-ttu-id="265e9-160">Le [informazioni di riferimento](/python/api/overview/azure/?view=azure-python) sono disponibili per tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="265e9-160">A [reference](/python/api/overview/azure/?view=azure-python) is available for all packages.</span></span>

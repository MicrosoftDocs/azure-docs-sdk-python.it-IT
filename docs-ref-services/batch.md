---
title: Librerie di Azure Batch per Python
description: Documentazione di riferimento per le librerie Batch per Python
keywords: Azure, Python, SDK, API, Batch, elaborazione, pianificazione, a esecuzione prolungata
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: bbc691a8db6597c77575900b4e2a06f34ebb179c
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534358"
---
# <a name="azure-batch-libraries-for-python"></a><span data-ttu-id="b3322-104">Librerie di Azure Batch per Python</span><span class="sxs-lookup"><span data-stu-id="b3322-104">Azure Batch libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="b3322-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b3322-105">Overview</span></span>

<span data-ttu-id="b3322-106">Eseguire in modo efficiente applicazioni di calcolo a prestazioni elevate su larga scala nel cloud con [Azure Batch](/azure/batch/batch-technical-overview).</span><span class="sxs-lookup"><span data-stu-id="b3322-106">Run large-scale parallel and high-performance computing applications efficiently in the cloud with [Azure Batch](/azure/batch/batch-technical-overview).</span></span>

<span data-ttu-id="b3322-107">Per iniziare a usare Azure Batch, vedere [Creare un account Batch nel portale di Azure](/azure/batch/batch-account-create-portal).</span><span class="sxs-lookup"><span data-stu-id="b3322-107">To get started with Azure Batch, see [Create a Batch account with the Azure portal](/azure/batch/batch-account-create-portal).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="b3322-108">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="b3322-108">Install the libraries</span></span>

## <a name="client-library"></a><span data-ttu-id="b3322-109">Libreria client</span><span class="sxs-lookup"><span data-stu-id="b3322-109">Client library</span></span>
<span data-ttu-id="b3322-110">Le librerie client di Azure Batch consentono di configurare i nodi e i pool di calcolo, definire le attività e configurarle per l'esecuzione nei processi e infine configurare un gestore di processi per controllare e monitorare l'esecuzione dei processi.</span><span class="sxs-lookup"><span data-stu-id="b3322-110">The Azure Batch client libraries let you configure compute nodes and pools, define tasks and configure them to run in jobs, and set up a job manager to control and monitor job execution.</span></span> <span data-ttu-id="b3322-111">[Altre informazioni](/azure/batch/batch-api-basics) sull'uso di questi oggetti per l'esecuzione di soluzioni di calcolo parallele su larga scala.</span><span class="sxs-lookup"><span data-stu-id="b3322-111">[Learn more](/azure/batch/batch-api-basics) about using these objects to run large-scale parallel compute solutions.</span></span>

```bash
pip install azure-batch
```
### <a name="example"></a><span data-ttu-id="b3322-112">Esempio</span><span class="sxs-lookup"><span data-stu-id="b3322-112">Example</span></span>

<span data-ttu-id="b3322-113">Configurare un pool di nodi di calcolo di Linux in un account Batch:</span><span class="sxs-lookup"><span data-stu-id="b3322-113">Set up a pool of Linux compute nodes in a batch account:</span></span>

```python
# create the batch client for an account using its URI and keys
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

## <a name="management-api"></a><span data-ttu-id="b3322-114">API di gestione</span><span class="sxs-lookup"><span data-stu-id="b3322-114">Management API</span></span>
<span data-ttu-id="b3322-115">Usare le librerie di gestione di Azure Batch per creare ed eliminare account Batch, leggere e rigenerare chiavi di account Batch e gestire l'archiviazione di account Batch.</span><span class="sxs-lookup"><span data-stu-id="b3322-115">Use the Azure Batch management libraries to create and delete batch accounts, read and regenerate batch account keys, and manage batch account storage.</span></span>

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="b3322-116">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="b3322-116">Explore the Client APIs</span></span>](/python/api/overview/azure/batch/client)

### <a name="example"></a><span data-ttu-id="b3322-117">Esempio</span><span class="sxs-lookup"><span data-stu-id="b3322-117">Example</span></span>
<span data-ttu-id="b3322-118">Creare un account Azure Batch e configurare una nuova applicazione e un account di archiviazione di Azure corrispondente.</span><span class="sxs-lookup"><span data-stu-id="b3322-118">Create an Azure Batch account and configure a new application and Azure storage account for it.</span></span>

```python
from azure.mgmt.batch import BatchManagementClient
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

LOCATION ='eastus'
GROUP_NAME ='batchresourcegroup'
STORAGE_ACCOUNT_NAME ='batchstorageaccount'

# Create Resource group
print('Create Resource Group')
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})

# Create a storage account
storage_async_operation = storage_client.storage_accounts.create(
    GROUP_NAME,
    STORAGE_ACCOUNT_NAME,
    StorageAccountCreateParameters(
        sku=Sku(SkuName.standard_ragrs),
        kind=Kind.storage,
        location=LOCATION
    )
)
storage_account = storage_async_operation.result()

# Create a Batch Account, specifying the storage account we want to link
storage_resource = storage_account.id
batch_account = azure.mgmt.batch.models.BatchAccountCreateParameters(
    location=LOCATION,
    auto_storage=azure.mgmt.batch.models.AutoStorageBaseProperties(storage_resource)
)
creating = batch_client.account.create('MyBatchAccount', LOCATION, batch_account)
creating.wait()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="b3322-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="b3322-119">Explore the Management APIs</span></span>](/python/api/overview/azure/batch/management)

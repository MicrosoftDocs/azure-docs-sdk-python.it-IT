---
title: Librerie di Azure Batch per Python
description: Documentazione di riferimento per le librerie Batch per Python
keywords: Azure, Python, SDK, API, Batch, elaborazione, pianificazione, a esecuzione prolungata
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/31/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: batch
ms.openlocfilehash: de5f3a98b1712ff9bdcc417daf10719178819364
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
---
# <a name="azure-batch-libraries-for-python"></a>Librerie di Azure Batch per Python

## <a name="overview"></a>Panoramica

Eseguire in modo efficiente applicazioni di calcolo a prestazioni elevate su larga scala nel cloud con [Azure Batch](/azure/batch/batch-technical-overview).   

Per iniziare a usare Azure Batch, vedere [Creare un account Batch nel portale di Azure](/azure/batch/batch-account-create-portal).

## <a name="install-the-libraries"></a>Installare le librerie

## <a name="client-library"></a>Libreria client
Le librerie client di Azure Batch consentono di configurare i nodi e i pool di calcolo, definire le attività e configurarle per l'esecuzione nei processi e infine configurare un gestore di processi per controllare e monitorare l'esecuzione dei processi. [Altre informazioni](/azure/batch/batch-api-basics) sull'uso di questi oggetti per l'esecuzione di soluzioni di calcolo parallele su larga scala.

```bash
pip install azure-batch
```
### <a name="example"></a>Esempio

Configurare un pool di nodi di calcolo di Linux in un account Batch:

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

## <a name="management-api"></a>API di gestione
Usare le librerie di gestione di Azure Batch per creare ed eliminare account Batch, leggere e rigenerare chiavi di account Batch e gestire l'archiviazione di account Batch.

```bash
pip install azure-mgmt-batch
```
> [!div class="nextstepaction"]
> [Esplorare le API client](/python/api/overview/azure/batch/client)

### <a name="example"></a>Esempio
Creare un account Azure Batch e configurare una nuova applicazione e un account di archiviazione di Azure corrispondente.

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
> [Esplorare le API di gestione](/python/api/overview/azure/batch/management)
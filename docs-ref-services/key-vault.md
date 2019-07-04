---
title: Librerie di Azure Key Vault per Python
description: Documentazione di riferimento per le librerie client Python per Azure Key Vault
author: sptramer
manager: carmonm
ms.author: sttramer
ms.date: 06/10/2019
ms.topic: conceptual
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: f4661ee389c13ce8546e7b5cc8866ab7b216d3b0
ms.sourcegitcommit: 92fa5dbcfd9a20f4a49da5f4bdc03045783d3495
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/15/2019
ms.locfileid: "67149333"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="ea338-103">Librerie di Azure Key Vault per Python</span><span class="sxs-lookup"><span data-stu-id="ea338-103">Azure Key Vault libraries for Python</span></span>

<span data-ttu-id="ea338-104">[Azure Key Vault](/azure/key-vault/) è il sistema di archiviazione e gestione dei segreti, dei certificati e delle chiavi di crittografia di Azure.</span><span class="sxs-lookup"><span data-stu-id="ea338-104">[Azure Key Vault](/azure/key-vault/) is Azure's storage and management system for cryptographic keys, secrets, and certificate management.</span></span> <span data-ttu-id="ea338-105">L'API di Python SDK per Key Vault è suddivisa tra le librerie client e le librerie di gestione.</span><span class="sxs-lookup"><span data-stu-id="ea338-105">The Python SDK API for Key Vault is split between client libraries and management libraries.</span></span>

<span data-ttu-id="ea338-106">Usare la libreria client per:</span><span class="sxs-lookup"><span data-stu-id="ea338-106">Use the client library to:</span></span>
- <span data-ttu-id="ea338-107">Accedere, aggiornare o eliminare elementi archiviati in un Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ea338-107">Access, update, or delete items stored in an Azure Key Vault</span></span>
- <span data-ttu-id="ea338-108">Ottenere i metadati per i certificati archiviati</span><span class="sxs-lookup"><span data-stu-id="ea338-108">Get metadata for stored certificates</span></span>
- <span data-ttu-id="ea338-109">Verificare le firme in base alle chiavi simmetriche in Key Vault</span><span class="sxs-lookup"><span data-stu-id="ea338-109">Verify signatures against symmetric keys in Key Vault</span></span>

<span data-ttu-id="ea338-110">Usare la libreria di gestione per:</span><span class="sxs-lookup"><span data-stu-id="ea338-110">Use the management library to:</span></span>
- <span data-ttu-id="ea338-111">Creare, aggiornare o eliminare nuovi archivi Key Vault</span><span class="sxs-lookup"><span data-stu-id="ea338-111">Create, update, or delete new Key Vault stores</span></span>
- <span data-ttu-id="ea338-112">Controllare i criteri di accesso all'insieme di credenziali</span><span class="sxs-lookup"><span data-stu-id="ea338-112">Control vault access policies</span></span>
- <span data-ttu-id="ea338-113">Elencare gli insiemi di credenziali per sottoscrizione o gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="ea338-113">List vaults by subscription or resource group</span></span>
- <span data-ttu-id="ea338-114">Controllare la disponibilità dei nomi di insiemi di credenziali</span><span class="sxs-lookup"><span data-stu-id="ea338-114">Check for vault name availability</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="ea338-115">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="ea338-115">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="ea338-116">Libreria client</span><span class="sxs-lookup"><span data-stu-id="ea338-116">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="ea338-117">Esempi</span><span class="sxs-lookup"><span data-stu-id="ea338-117">Examples</span></span>

<span data-ttu-id="ea338-118">Negli esempi seguenti si usa l'autenticazione basata su entità servizio, che costituisce il metodo di accesso consigliato per le applicazioni che si connettono ad Azure.</span><span class="sxs-lookup"><span data-stu-id="ea338-118">The following examples use service principal authentication, which is the recommended sign in method for applications that connect to Azure.</span></span> <span data-ttu-id="ea338-119">Per informazioni sull'autenticazione basata su entità servizio, vedere [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span><span class="sxs-lookup"><span data-stu-id="ea338-119">To learn about service principal authentication, see [Authenticate with the Azure SDK for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)</span></span>

<span data-ttu-id="ea338-120">Recuperare la parte pubblica di una chiave asimmetrica da un insieme di credenziali:</span><span class="sxs-lookup"><span data-stu-id="ea338-120">Retrieve the public portion of an asymmetric key from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# KEY_VERSION is required, and can be obtained with the KeyVaultClient.get_key_versions(self, vault_url, key_name) API
key_bundle = client.get_key(VAULT_URL, KEY_NAME, KEY_VERSION)
key = key_bundle.key
```

<span data-ttu-id="ea338-121">Recuperare un segreto da un insieme di credenziali:</span><span class="sxs-lookup"><span data-stu-id="ea338-121">Retrieve a secret from a vault:</span></span>

```python
from azure.keyvault import KeyVaultClient
from azure.common.credentials import ServicePrincipalCredentials

credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

client = KeyVaultClient(credentials)

# VAULT_URL must be in the format 'https://<vaultname>.vault.azure.net'
# SECRET_VERSION is required, and can be obtained with the KeyVaultClient.get_secret_versions(self, vault_url, secret_id) API
secret_bundle = client.get_secret(VAULT_URL, SECRET_ID, SECRET_VERSION)
secret = secret_bundle.value
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ea338-122">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="ea338-122">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a><span data-ttu-id="ea338-123">Libreria di gestione</span><span class="sxs-lookup"><span data-stu-id="ea338-123">Management library</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="ea338-124">Esempio</span><span class="sxs-lookup"><span data-stu-id="ea338-124">Example</span></span>

<span data-ttu-id="ea338-125">Il seguente esempio mostra come creare un'istanza di Azure Key Vault:</span><span class="sxs-lookup"><span data-stu-id="ea338-125">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient
from azure.common.credentials import ServicePrincipalCredentials


credentials = ServicePrincipalCredentials(
    client_id = '...',
    secret = '...',
    tenant = '...'
)

# Even when using service principal credentials, a subscription ID is required. For service principals,
# this should be the subscription used to create the service principal. Storing a token like a valid
# subscription ID in code is not recommended and only shown here for example purposes.
SUBSCRIPTION_ID = '...'
client = KeyVaultManagementClient(credentials, SUBSCRIPTION_ID)

# The object ID and organization ID (tenant) of the user, application, or service principal for access policies.
# These values can be found through the Azure CLI or the Portal.
ALLOW_OBJECT_ID = '...'
ALLOW_TENANT_ID = '...'

RESOURCE_GROUP = '...'
VAULT_NAME = '...'

# Vault properties may also be created by using the azure.mgmt.keyvault.models.VaultCreateOrUpdateParameters
# class, rather than a map. 
operation = client.vaults.create_or_update(
    RESOURCE_GROUP,
    VAULT_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': TENANT_ID,
            'access_policies': [{
                'object_id': OBJECT_ID,
                'tenant_id': ALLOW_TENANT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)

vault = operation.result()
print(f'New vault URI: {vault.properties.vault_uri}')
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="ea338-126">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="ea338-126">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="ea338-127">Esempi</span><span class="sxs-lookup"><span data-stu-id="ea338-127">Samples</span></span>
* <span data-ttu-id="ea338-128">[Gestire più Azure Key Vault][1]</span><span class="sxs-lookup"><span data-stu-id="ea338-128">[Manage Azure Key Vaults][1]</span></span> 
* <span data-ttu-id="ea338-129">[Ripristino di Azure Key Vault][2]</span><span class="sxs-lookup"><span data-stu-id="ea338-129">[Azure Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="ea338-130">Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) degli esempi di codice per Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="ea338-130">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 

---
title: Librerie di Azure Key Vault per Python
description: Documentazione di riferimento per le librerie client Python per Azure Key Vault
author: lisawong19
keywords: Azure, Python, SDK, API, chiavi, Key Vault, autenticazione, segreto, chiave, sicurezza
manager: douge
ms.author: liwong
ms.date: 07/18/2017
ms.topic: article
ms.devlang: python
ms.service: keyvault
ms.openlocfilehash: 3e7d9970f5799708c6822493106aec5466de52d9
ms.sourcegitcommit: 86f7f40295271ef94272642efb89b471aae99a2c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35719642"
---
# <a name="azure-key-vault-libraries-for-python"></a><span data-ttu-id="360ef-104">Librerie di Azure Key Vault per Python</span><span class="sxs-lookup"><span data-stu-id="360ef-104">Azure Key Vault libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="360ef-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="360ef-105">Overview</span></span>

<span data-ttu-id="360ef-106">Creare, aggiornare ed eliminare le chiavi e i segreti contenuti in Azure Key Vault con le librerie client.</span><span class="sxs-lookup"><span data-stu-id="360ef-106">Create, update, and delete keys and secrets in Azure Key Vault with the client libraries.</span></span>

<span data-ttu-id="360ef-107">Usare le librerie di gestione di Azure Key Vault per creare insiemi di credenziali delle chiavi, autorizzare le applicazioni e gestire le autorizzazioni.</span><span class="sxs-lookup"><span data-stu-id="360ef-107">Use the Azure Key Vault management libraries to create key vaults, authorize applications, and manage permissions.</span></span> 

<span data-ttu-id="360ef-108">Altre informazioni su [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="360ef-108">Learn more about [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="360ef-109">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="360ef-109">Install the libraries</span></span>

### <a name="client-library"></a><span data-ttu-id="360ef-110">Libreria client</span><span class="sxs-lookup"><span data-stu-id="360ef-110">Client library</span></span>

```bash
pip install azure-keyvault
```

## <a name="examples"></a><span data-ttu-id="360ef-111">Esempi</span><span class="sxs-lookup"><span data-stu-id="360ef-111">Examples</span></span>

<span data-ttu-id="360ef-112">Recuperare una [chiave Web JSON](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) da un insieme di credenziali delle chiavi.</span><span class="sxs-lookup"><span data-stu-id="360ef-112">Retrieve a [JSON web key](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) from a Key Vault.</span></span>

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```

<span data-ttu-id="360ef-113">Analogamente, è possibile usare il frammento di codice seguente per recuperare un segreto da un insieme di credenziali:</span><span class="sxs-lookup"><span data-stu-id="360ef-113">Similarly, you can use the following snippet to retrieve a secret from the vault:</span></span>

```
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callback(server, resource, scope):
    credentials = ServicePrincipalCredentials(
        client_id = '',
        secret = '',
        tenant = '',
        resource = "https://vault.azure.net"
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callback))

secret_bundle = client.get_secret("https://VAULT_ID.vault.azure.net/", "SECRET_ID", "SECRET_VERSION")

print(secret_bundle.value)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="360ef-114">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="360ef-114">Explore the Client APIs</span></span>](/python/api/overview/azure/keyvault/client)

### <a name="management-api"></a><span data-ttu-id="360ef-115">API di gestione</span><span class="sxs-lookup"><span data-stu-id="360ef-115">Management API</span></span>

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a><span data-ttu-id="360ef-116">Esempio</span><span class="sxs-lookup"><span data-stu-id="360ef-116">Example</span></span>
<span data-ttu-id="360ef-117">Il seguente esempio mostra come creare un'istanza di Azure Key Vault:</span><span class="sxs-lookup"><span data-stu-id="360ef-117">The following example shows how to create an Azure Key Vault.</span></span> 

```python
from azure.mgmt.keyvault import KeyVaultManagementClient

GROUP_NAME = 'your_resource_group_name'
KV_NAME = 'your_key_vault_name'
#The object ID of the User or Application for access policies. Find this number in the portal
OBJECT_ID = '00000000-0000-0000-0000-000000000000'

kv_client = KeyVaultManagementClient(credentials, subscription_id)

vault = kv_client.vaults.create_or_update(
    GROUP_NAME,
    KV_NAME,
    {
        'location': 'eastus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': os.environ['AZURE_TENANT_ID'],
            'access_policies': [{
                'tenant_id': os.environ['AZURE_TENANT_ID'],
                'object_id': OBJECT_ID,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="360ef-118">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="360ef-118">Explore the Management APIs</span></span>](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [<span data-ttu-id="360ef-119">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="360ef-119">Explore the Management APIs</span></span>](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a><span data-ttu-id="360ef-120">Esempi</span><span class="sxs-lookup"><span data-stu-id="360ef-120">Samples</span></span>
* <span data-ttu-id="360ef-121">[Gestire gli insiemi di credenziali delle chiavi][1]</span><span class="sxs-lookup"><span data-stu-id="360ef-121">[Manage Key Vaults][1]</span></span> 
* <span data-ttu-id="360ef-122">[Ripristino di Key Vault][2]</span><span class="sxs-lookup"><span data-stu-id="360ef-122">[Key Vault recovery][2]</span></span>

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

<span data-ttu-id="360ef-123">Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) degli esempi di codice per Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="360ef-123">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) of Azure Key Vault samples.</span></span> 

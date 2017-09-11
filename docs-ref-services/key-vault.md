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
ms.openlocfilehash: 3eac46eb4d5d19273ead9f19b739f6fb6d72e5cc
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-libraries-for-python"></a>Librerie di Azure Key Vault per Python

## <a name="overview"></a>Panoramica

Creare, aggiornare ed eliminare le chiavi e i segreti contenuti in Azure Key Vault con le librerie client.

Usare le librerie di gestione di Azure Key Vault per creare insiemi di credenziali delle chiavi, autorizzare le applicazioni e gestire le autorizzazioni. 

Altre informazioni su [Azure Key Vault](/azure/key-vault/key-vault-whatis).

## <a name="install-the-libraries"></a>Installare le librerie

### <a name="client-library"></a>Libreria client
```bash
pip install azure-keyvault
```

## <a name="example"></a>Esempio
Recuperare una [chiave Web JSON](https://tools.ietf.org/html/draft-ietf-jose-json-web-key-18) da un insieme di credenziali delle chiavi.

```python
from azure.keyvault import KeyVaultClient, KeyVaultAuthentication
from azure.common.credentials import ServicePrincipalCredentials

credentials = None

def auth_callack(server, resource, scope):
    credentials = credentials or ServicePrincipalCredentials(
        client_id = '', #client id
        secret = '',
        tenant = '',
        resource = resource
    )
    token = credentials.token
    return token['token_type'], token['access_token']

client = KeyVaultClient(KeyVaultAuthentication(auth_callack))

key_bundle = client.get_key(vault_url, key_name, key_version)
json_key = key_bundle.key
```
[!div class="nextstepaction"]
[Esplorare le API client](/python/api/overview/azure/keyvault/clientlibrary)

### <a name="management-api"></a>API di gestione
```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>Esempio
Il seguente esempio mostra come creare un'istanza di Azure Key Vault: 

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
> [Esplorare le API di gestione](/python/api/azure.mgmt.keyvault)

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/keyvault/managementlibrary)

## <a name="samples"></a>Esempi
* [Gestire gli insiemi di credenziali delle chiavi][1] 
* [Ripristino di Key Vault][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) degli esempi di codice per Azure Key Vault. 
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
# <a name="azure-key-vault-libraries-for-python"></a>Librerie di Azure Key Vault per Python

[Azure Key Vault](/azure/key-vault/) è il sistema di archiviazione e gestione dei segreti, dei certificati e delle chiavi di crittografia di Azure. L'API di Python SDK per Key Vault è suddivisa tra le librerie client e le librerie di gestione.

Usare la libreria client per:
- Accedere, aggiornare o eliminare elementi archiviati in un Azure Key Vault
- Ottenere i metadati per i certificati archiviati
- Verificare le firme in base alle chiavi simmetriche in Key Vault

Usare la libreria di gestione per:
- Creare, aggiornare o eliminare nuovi archivi Key Vault
- Controllare i criteri di accesso all'insieme di credenziali
- Elencare gli insiemi di credenziali per sottoscrizione o gruppo di risorse
- Controllare la disponibilità dei nomi di insiemi di credenziali

## <a name="install-the-libraries"></a>Installare le librerie

### <a name="client-library"></a>Libreria client

```bash
pip install azure-keyvault
```

## <a name="examples"></a>Esempi

Negli esempi seguenti si usa l'autenticazione basata su entità servizio, che costituisce il metodo di accesso consigliato per le applicazioni che si connettono ad Azure. Per informazioni sull'autenticazione basata su entità servizio, vedere [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate)

Recuperare la parte pubblica di una chiave asimmetrica da un insieme di credenziali:

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

Recuperare un segreto da un insieme di credenziali:

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
> [Esplorare le API client](/python/api/overview/azure/keyvault/client)

### <a name="management-library"></a>Libreria di gestione

```bash
pip install azure-mgmt-keyvault
```

### <a name="example"></a>Esempio

Il seguente esempio mostra come creare un'istanza di Azure Key Vault: 

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
> [Esplorare le API di gestione](/python/api/overview/azure/keyvault/management)

## <a name="samples"></a>Esempi
* [Gestire più Azure Key Vault][1] 
* [Ripristino di Azure Key Vault][2]

[1]: https://azure.microsoft.com/resources/samples/key-vault-python-manage/
[2]: https://azure.microsoft.com/resources/samples/key-vault-recovery-python/

Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=key+vault) degli esempi di codice per Azure Key Vault. 

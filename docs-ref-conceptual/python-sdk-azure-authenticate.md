---
title: Eseguire l'autenticazione con le librerie di gestione di Azure per Python
description: "Eseguire l'autenticazione con un'entità servizio nelle librerie di gestione di Azure per Python"
keywords: "Azure, Python, SDK, API, autenticazione, active directory, entità servizio"
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 07/24/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 000397b573700aa92572a6252b6d84da8945a1e5
ms.sourcegitcommit: 79afc8a1b427e26ecea7bdc0b7b3c898f143360f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2017
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a>Eseguire l'autenticazione con le librerie di gestione di Azure per Python

Sono disponibili alcune opzioni per autenticare l'applicazione con Azure quando si usano le librerie di gestione Python per creare e gestire le risorse.

## <a name="mgmt-auth-token"></a>Eseguire l'autenticazione con le credenziali del token

Archiviare le credenziali in modo sicuro nel file di configurazione, nel Registro di sistema o in Azure Key Vault.

L'esempio seguente usa un'[entità servizio](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) per l'autenticazione.

> [!NOTE]
> È possibile creare un'entità servizio tramite l'interfaccia della riga di comando di Azure 2.0
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```

```python
    from azure.common.credentials import ServicePrincipalCredentials

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID
    )
```

> [NOTA] Per connettersi a uno dei cloud sovrani di Azure, usare il parametro `cloud_environment`.

```python
    from azure.common.credentials import ServicePrincipalCredentials
    from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    credentials = ServicePrincipalCredentials(
        client_id = CLIENT,
        secret = KEY,
        tenant = TENANT_ID,
        cloud_environment = AZURE_CHINA_CLOUD
    )
```

Se è necessario un controllo maggiore, è consigliabile usare [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) e il wrapper ADAL per SDK. Per un elenco ed esempi di tutti gli scenari disponibili, vedere il sito Web di ADAL. Ad esempio, per l'autenticazione dell'entità servizio:

```python
    import adal
    from msrestazure.azure_active_directory import AdalAuthentication
    from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

    # Tenant ID for your Azure Subscription
    TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'

    # Your Service Principal App ID
    CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'

    # Your Service Principal Password
    KEY = 'password'

    LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
    RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

    context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
    credentials = AdalAuthentication(
        context.acquire_token_with_client_credentials,
        RESOURCE,
        CLIENT,
        KEY
    )
```

Tutte le chiamate ADAL valide possono essere usate con la classe `AdalAuthentication`.

Creare quindi un oggetto client per iniziare a usare l'API:

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> [NOTA] Quando si usa un cloud sovrano di Azure, è necessario specificare anche l'URL di base appropriato, tramite i vincoli in `msrestazure.azure_cloud`, durante la creazione del client di gestione. Ad esempio, per il cloud di Azure Cina:
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id)
> ```


## <a name="mgmt-auth-file"></a>Autenticazione basata su file

Il modo più semplice per eseguire l'autenticazione consiste nel creare un file JSON che include le credenziali per un'entità servizio di Azure. È possibile usare il comando seguente dell'interfaccia della riga di comando per creare una nuova entità servizio e questo file contemporaneamente:

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

Salvare questo file nel sistema in una posizione sicura e leggibile dal codice. Impostare una variabile di ambiente con il percorso completo del file nella shell:

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

Se si vuole creare manualmente il file, seguire questo formato:

```json
{
    "clientId": "ad735158-65ca-11e7-ba4d-ecb1d756380e",
    "clientSecret": "b70bb224-65ca-11e7-810c-ecb1d756380e",
    "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
    "tenantId": "c81da1d8-65ca-11e7-b1d1-ecb1d756380e",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

È quindi possibile creare qualsiasi client usando una factory di client:
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a>Eseguire l'autenticazione con l'identità del servizio gestito 
L'identità del servizio gestito offre a una risorsa in Azure un modo semplice per usare l'SDK/interfaccia della riga di comando senza dover creare credenziali specifiche.

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

    # Create MSI Authentication
    credentials = MSIAuthentication()


    # Create a Subscription Client
    subscription_client = SubscriptionClient(credentials)
    subscription = next(subscription_client.subscriptions.list())
    subscription_id = subscription.subscription_id

    # Create a Resource Management client
    resource_client = ResourceManagementClient(credentials, subscription_id)

    
    # List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
    for resource_group in resource_client.resource_groups.list():
        print(resource_group.name)

```

## <a name="mgmt-auth-cli"></a>Autenticazione basata sull'interfaccia della riga di comando

L'SDK consente di creare un client tramite la sottoscrizione attiva dell'interfaccia della riga di comando.

> [!IMPORTANT]
> Questo approccio è consigliato come esperienza di avvio rapido per gli sviluppatori. Per finalità di produzione, usare [ADAL](#authenticate-with-token-credentials) o il proprio sistema di credenziali.
> Eventuali modifiche apportate alla configurazione dell'interfaccia della riga di comando influiranno sull'esecuzione dell'SDK.

Per definire le credenziali attive, usare [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).
L'ID sottoscrizione predefinito corrisponde a quello disponibile oppure è possibile definirlo usando [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a>Eseguire l'autenticazione con le credenziali del token (legacy)

Nella versione precedente dell'SDK non è ancora disponibile ADAL e viene quindi fornita una classe `UserPassCredentials`. Questo approccio viene considerato deprecato e non deve essere più usato.

Questo esempio mostra uno scenario con utente/password. Non è supportata l'autenticazione a due fattori.

```python
    from azure.common.credentials import UserPassCredentials

    credentials = UserPassCredentials(
        'user@domain.com',
        'my_smart_password',
    )
```

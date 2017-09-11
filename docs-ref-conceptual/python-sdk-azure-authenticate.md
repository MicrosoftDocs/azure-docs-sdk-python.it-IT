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
ms.openlocfilehash: 1dba0bdd9b543c11b31f3001737038e7e99daf08
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="b38c5-104">Eseguire l'autenticazione con le librerie di gestione di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="b38c5-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="b38c5-105">Sono disponibili alcune opzioni per autenticare l'applicazione con Azure quando si usano le librerie di gestione Python per creare e gestire le risorse.</span><span class="sxs-lookup"><span data-stu-id="b38c5-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <span data-ttu-id="b38c5-106"><a name="mgmt-auth-token"></a>Eseguire l'autenticazione con le credenziali del token</span><span class="sxs-lookup"><span data-stu-id="b38c5-106"><a name="mgmt-auth-token"></a>Authenticate with token credentials</span></span>

<span data-ttu-id="b38c5-107">Archiviare le credenziali in modo sicuro nel file di configurazione, nel Registro di sistema o in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="b38c5-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="b38c5-108">L'esempio seguente usa un'[entità servizio](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="b38c5-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="b38c5-109">È possibile creare un'entità servizio tramite l'interfaccia della riga di comando di Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="b38c5-109">You can create a Service Principal via the Azure CLI 2.0</span></span>
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

> <span data-ttu-id="b38c5-110">[Nota] Per connettersi a uno dei cloud sovrani di Azure, usare il parametro `cloud_environment`.</span><span class="sxs-lookup"><span data-stu-id="b38c5-110">[Note!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>

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

<span data-ttu-id="b38c5-111">Se è necessario un controllo maggiore, è consigliabile usare [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) e il wrapper ADAL per SDK.</span><span class="sxs-lookup"><span data-stu-id="b38c5-111">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="b38c5-112">Per un elenco ed esempi di tutti gli scenari disponibili, vedere il sito Web di ADAL.</span><span class="sxs-lookup"><span data-stu-id="b38c5-112">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="b38c5-113">Ad esempio, per l'autenticazione dell'entità servizio:</span><span class="sxs-lookup"><span data-stu-id="b38c5-113">For instance for service principal authentication:</span></span>

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

<span data-ttu-id="b38c5-114">Tutte le chiamate ADAL valide possono essere usate con la classe `AdalAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="b38c5-114">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="b38c5-115">Creare quindi un oggetto client per iniziare a usare l'API:</span><span class="sxs-lookup"><span data-stu-id="b38c5-115">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="b38c5-116">[Nota] Quando si usa un cloud sovrano di Azure, è necessario specificare anche l'URL di base appropriato, tramite i vincoli in `msrestazure.azure_cloud`, durante la creazione del client di gestione.</span><span class="sxs-lookup"><span data-stu-id="b38c5-116">[Note!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="b38c5-117">Ad esempio, per il cloud di Azure Cina:</span><span class="sxs-lookup"><span data-stu-id="b38c5-117">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id)
> ```

## <span data-ttu-id="b38c5-118"><a name="mgmt-auth-file"></a>Autenticazione basata su file</span><span class="sxs-lookup"><span data-stu-id="b38c5-118"><a name="mgmt-auth-file"></a>File based authentication</span></span>

<span data-ttu-id="b38c5-119">Il modo più semplice per eseguire l'autenticazione consiste nel creare un file JSON che include le credenziali per un'entità servizio di Azure.</span><span class="sxs-lookup"><span data-stu-id="b38c5-119">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="b38c5-120">È possibile usare il comando seguente dell'interfaccia della riga di comando per creare una nuova entità servizio e questo file contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="b38c5-120">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="b38c5-121">Salvare questo file nel sistema in una posizione sicura e leggibile dal codice.</span><span class="sxs-lookup"><span data-stu-id="b38c5-121">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="b38c5-122">Impostare una variabile di ambiente con il percorso completo del file nella shell:</span><span class="sxs-lookup"><span data-stu-id="b38c5-122">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="b38c5-123">Se si vuole creare manualmente il file, seguire questo formato:</span><span class="sxs-lookup"><span data-stu-id="b38c5-123">If you want to create the file yourself, please follow this format:</span></span>

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

<span data-ttu-id="b38c5-124">È quindi possibile creare qualsiasi client usando una factory di client:</span><span class="sxs-lookup"><span data-stu-id="b38c5-124">You can then create any client using the client factory:</span></span>
```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```


## <span data-ttu-id="b38c5-125"><a name="mgmt-auth-cli"></a>Autenticazione basata sull'interfaccia della riga di comando</span><span class="sxs-lookup"><span data-stu-id="b38c5-125"><a name="mgmt-auth-cli"></a>CLI-based authentication</span></span>

<span data-ttu-id="b38c5-126">L'SDK consente di creare un client tramite la sottoscrizione attiva dell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="b38c5-126">The SDK is able to create a client using your CLI active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b38c5-127">Questo approccio è consigliato come esperienza di avvio rapido per gli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="b38c5-127">This should be used as quick start developer experience.</span></span> <span data-ttu-id="b38c5-128">Per finalità di produzione, usare [ADAL](#authenticate-with-token-credentials) o il proprio sistema di credenziali.</span><span class="sxs-lookup"><span data-stu-id="b38c5-128">For production purposes, use [ADAL](#authenticate-with-token-credentials) or your own credentials system.</span></span>
> <span data-ttu-id="b38c5-129">Eventuali modifiche apportate alla configurazione dell'interfaccia della riga di comando influiranno sull'esecuzione dell'SDK.</span><span class="sxs-lookup"><span data-stu-id="b38c5-129">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="b38c5-130">Per definire le credenziali attive, usare [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b38c5-130">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="b38c5-131">L'ID sottoscrizione predefinito corrisponde a quello disponibile oppure è possibile definirlo usando [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="b38c5-131">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <span data-ttu-id="b38c5-132"><a name="mgmt-auth-legacy"></a>Eseguire l'autenticazione con le credenziali del token (legacy)</span><span class="sxs-lookup"><span data-stu-id="b38c5-132"><a name="mgmt-auth-legacy"></a>Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="b38c5-133">Nella versione precedente dell'SDK non è ancora disponibile ADAL e viene quindi fornita una classe `UserPassCredentials`.</span><span class="sxs-lookup"><span data-stu-id="b38c5-133">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="b38c5-134">Questo approccio viene considerato deprecato e non deve essere più usato.</span><span class="sxs-lookup"><span data-stu-id="b38c5-134">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="b38c5-135">Questo esempio mostra uno scenario con utente/password.</span><span class="sxs-lookup"><span data-stu-id="b38c5-135">This sample shows user/password scenario.</span></span> <span data-ttu-id="b38c5-136">Non è supportata l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="b38c5-136">This does not support 2FA.</span></span>

```python
    from azure.common.credentials import UserPassCredentials

    credentials = UserPassCredentials(
        'user@domain.com',
        'my_smart_password',
    )
```

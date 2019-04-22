---
title: Eseguire l'autenticazione con le librerie di gestione di Azure per Python
description: Eseguire l'autenticazione con un'entità servizio nelle librerie di gestione di Azure per Python
keywords: Azure, Python, SDK, API, autenticazione, active directory, entità servizio
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/11/2019
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 51f26b120cefffd2d7f4af9c2b6b2cb532bc6006
ms.sourcegitcommit: 375a1f9180eb1323fe2af0a7e28fd4676973c68e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59586809"
---
# <a name="authenticate-with-the-azure-management-libraries-for-python"></a><span data-ttu-id="9e0ad-104">Eseguire l'autenticazione con le librerie di gestione di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="9e0ad-104">Authenticate with the Azure Management Libraries for Python</span></span>

<span data-ttu-id="9e0ad-105">Sono disponibili alcune opzioni per autenticare l'applicazione con Azure quando si usano le librerie di gestione Python per creare e gestire le risorse.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-105">Several options are available to authenticate your application with Azure when using the Python management libraries to create and manage resources.</span></span>

## <a name="mgmt-auth-token"></a><span data-ttu-id="9e0ad-106">Eseguire l'autenticazione con le credenziali del token</span><span class="sxs-lookup"><span data-stu-id="9e0ad-106">Authenticate with token credentials</span></span>

<span data-ttu-id="9e0ad-107">Archiviare le credenziali in modo sicuro nel file di configurazione, nel Registro di sistema o in Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-107">Store the credentials securely in a configuration file, the registry, or Azure KeyVault.</span></span>

<span data-ttu-id="9e0ad-108">L'esempio seguente usa un'[entità servizio](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-108">The following example uses a [Service Principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) for authentication.</span></span>

> [!NOTE]
> <span data-ttu-id="9e0ad-109">Per cerare un'entità servizio con l'interfaccia della riga di comando di Azure, usare il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-109">To create a service principal with the Azure CLI, use the following command:</span></span>
>
> ```bash
> az ad sp create-for-rbac --name "MY-PRINCIPAL-NAME" --password "STRONG-SECRET-PASSWORD"
> ```
>
> <span data-ttu-id="9e0ad-110">Per altre informazioni sulla configurazione di entità servizio con l'interfaccia della riga di comando, vedere [Creare un'entità servizio di Azure con l'interfaccia della riga di comando di Azure](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9e0ad-110">To learn more about setting up service princpals with the CLI, see [Create an Azure service principal with Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials

# Tenant ID for your Azure subscription
TENANT_ID = '<Your tenant ID>'

# Your service principal App ID
CLIENT = '<Your service principal ID>'

# Your service principal password
KEY = '<Your service principal password>'

credentials = ServicePrincipalCredentials(
    client_id = CLIENT,
    secret = KEY,
    tenant = TENANT_ID
)
```

> <span data-ttu-id="9e0ad-111">[NOTA] Per connettersi a uno dei cloud sovrani di Azure, usare il parametro `cloud_environment`.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-111">[NOTE!] To connect to one of the Azure sovereign clouds, use the `cloud_environment` parameter.</span></span>
>
> ```python
> from azure.common.credentials import ServicePrincipalCredentials
> from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
> 
> # Tenant ID for your Azure Subscription
> TENANT_ID = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
> 
> # Your Service Principal App ID
> CLIENT = 'a2ab11af-01aa-4759-8345-7803287dbd39'
> 
> # Your Service Principal Password
> KEY = 'password'
> 
> credentials = ServicePrincipalCredentials(
>     client_id = CLIENT,
>     secret = KEY,
>     tenant = TENANT_ID,
>     cloud_environment = AZURE_CHINA_CLOUD
> )
> ```

<span data-ttu-id="9e0ad-112">Se è necessario un controllo maggiore, è consigliabile usare [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) e il wrapper ADAL per SDK.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-112">If you need more control, it is recommended to use [ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-python) and the SDK ADAL wrapper.</span></span> <span data-ttu-id="9e0ad-113">Per un elenco ed esempi di tutti gli scenari disponibili, vedere il sito Web di ADAL.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-113">Please refer to the ADAL website for all the available scenarios list and samples.</span></span> <span data-ttu-id="9e0ad-114">Ad esempio, per l'autenticazione dell'entità servizio:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-114">For instance for service principal authentication:</span></span>

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

<span data-ttu-id="9e0ad-115">Tutte le chiamate ADAL valide possono essere usate con la classe `AdalAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-115">All ADAL valid calls can be used with the `AdalAuthentication` class.</span></span>

<span data-ttu-id="9e0ad-116">Creare quindi un oggetto client per iniziare a usare l'API:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-116">Next, create a client object to start working with the API:</span></span>

```python
from azure.mgmt.compute import ComputeManagementClient

# Your Azure Subscription ID
subscription_id = '33333333-3333-3333-3333-333333333333'

client = ComputeManagementClient(credentials, subscription_id)
```

> <span data-ttu-id="9e0ad-117">[NOTA] Quando si usa un cloud sovrano di Azure, è necessario specificare anche l'URL di base appropriato, tramite i vincoli in `msrestazure.azure_cloud`, durante la creazione del client di gestione.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-117">[NOTE!] When using an Azure sovereign cloud you must also specify the appropriate base URL (via the constants in `msrestazure.azure_cloud`) when creating the management client.</span></span> <span data-ttu-id="9e0ad-118">Ad esempio, per il cloud di Azure Cina:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-118">For example for Azure China Cloud:</span></span>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```


## <a name="mgmt-auth-file"></a><span data-ttu-id="9e0ad-119">Autenticazione basata su file</span><span class="sxs-lookup"><span data-stu-id="9e0ad-119">File based authentication</span></span>

<span data-ttu-id="9e0ad-120">Il modo più semplice per eseguire l'autenticazione consiste nel creare un file JSON che include le credenziali per un'entità servizio di Azure.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-120">The simplest way to authenticate is to create a JSON file that contains credentials for an Azure Service Principal.</span></span> <span data-ttu-id="9e0ad-121">È possibile usare il comando seguente dell'interfaccia della riga di comando per creare una nuova entità servizio e questo file contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-121">You can use the following CLI command to create a new Service Principal and this file at the same time:</span></span>

```bash
az ad sp create-for-rbac --sdk-auth > mycredentials.json
```

<span data-ttu-id="9e0ad-122">Salvare questo file nel sistema in una posizione sicura e leggibile dal codice.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-122">Save this file in a secure location on your system where your code can read it.</span></span> <span data-ttu-id="9e0ad-123">Impostare una variabile di ambiente con il percorso completo del file nella shell:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-123">Set an environment variable with the full path to the file in your shell:</span></span>

```bash
export AZURE_AUTH_LOCATION=~/.azure/azure_credentials.json
```

<span data-ttu-id="9e0ad-124">Se si vuole creare manualmente il file, seguire questo formato:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-124">If you want to create the file yourself, please follow this format:</span></span>

```json
{
    "clientId": "<Service principal ID>",
    "clientSecret": "<Service principal secret/password>",
    "subscriptionId": "<Subscription associated with the service principal>",
    "tenantId": "<The service principal's tenant>",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
}
```

<span data-ttu-id="9e0ad-125">È quindi possibile creare qualsiasi client usando una factory di client:</span><span class="sxs-lookup"><span data-stu-id="9e0ad-125">You can then create any client using the client factory:</span></span>

```python
from azure.common.client_factory import get_client_from_auth_file
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_auth_file(ComputeManagementClient)
```

## <a name="mgmt-auth-msi"></a><span data-ttu-id="9e0ad-126">Autenticazione con identità gestite di Azure</span><span class="sxs-lookup"><span data-stu-id="9e0ad-126">Authenticate with Azure Managed Identities</span></span>
<span data-ttu-id="9e0ad-127">L'identità del gestita di Azure offre a una risorsa in Azure un modo semplice per usare l'SDK/interfaccia della riga di comando senza dover creare credenziali specifiche.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-127">Azure Managed Identity is a simple way for a resource in Azure to use SDK/CLI without the need to create specific credentials.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="9e0ad-128">Per usare le identità gestite è necessario connettersi ad Azure da una risorsa di Azure, ad esempio una funzione di Azure o una macchina virtuale eseguita in Azure.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-128">To use managed identities, you must be connecting to Azure from an Azure resource, such as an Azure Function or a VM running in Azure.</span></span> <span data-ttu-id="9e0ad-129">Per altre informazioni sulla configurazione di un'identità gestita per una risorsa, vedere [Configurare le identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) e [Come usare le identità gestite per le risorse di Azure](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).</span><span class="sxs-lookup"><span data-stu-id="9e0ad-129">To learn how to configure a managed identity for a resource, see [Configure managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm) and [How to use managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in).</span></span>

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

## <a name="mgmt-auth-cli"></a><span data-ttu-id="9e0ad-130">Autenticazione basata sull'interfaccia della riga di comando</span><span class="sxs-lookup"><span data-stu-id="9e0ad-130">CLI-based authentication</span></span>

<span data-ttu-id="9e0ad-131">L'SDK consente di creare un client tramite la sottoscrizione attiva dell'interfaccia della riga di comando di Azure.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-131">The SDK is able to create a client using the Azure CLI's active subscription.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9e0ad-132">Questo approccio è consigliato come esperienza di avvio rapido per gli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-132">This should be used as quick start developer experience.</span></span> <span data-ttu-id="9e0ad-133">Per finalità di produzione, usare [ADAL](#mgmt-auth-legacy) o il proprio sistema di credenziali.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-133">For production purposes, use [ADAL](#mgmt-auth-legacy) or your own credentials system.</span></span>
> <span data-ttu-id="9e0ad-134">Eventuali modifiche apportate alla configurazione dell'interfaccia della riga di comando influiranno sull'esecuzione dell'SDK.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-134">Any change to your CLI configuration will impact the SDK execution.</span></span>

<span data-ttu-id="9e0ad-135">Per definire le credenziali attive, usare [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9e0ad-135">To define active credentials, use [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).</span></span>
<span data-ttu-id="9e0ad-136">L'ID sottoscrizione predefinito corrisponde a quello disponibile oppure è possibile definirlo usando [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="9e0ad-136">Default subscription ID is either the only one you have, or you can define it using [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli)</span></span>

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```

## <a name="mgmt-auth-legacy"></a><span data-ttu-id="9e0ad-137">Eseguire l'autenticazione con le credenziali del token (legacy)</span><span class="sxs-lookup"><span data-stu-id="9e0ad-137">Authenticate with token credentials (legacy)</span></span>

<span data-ttu-id="9e0ad-138">Nella versione precedente dell'SDK non è ancora disponibile ADAL e viene quindi fornita una classe `UserPassCredentials`.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-138">In previous version of the SDK, ADAL was not yet available and we provided a `UserPassCredentials` class.</span></span> <span data-ttu-id="9e0ad-139">Questo approccio viene considerato deprecato e non deve essere più usato.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-139">This is considered deprecated and should not be used anymore.</span></span>

<span data-ttu-id="9e0ad-140">Questo esempio mostra uno scenario con utente/password.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-140">This sample shows user/password scenario.</span></span> <span data-ttu-id="9e0ad-141">Non è supportata l'autenticazione a due fattori.</span><span class="sxs-lookup"><span data-stu-id="9e0ad-141">This does not support 2FA.</span></span>

```python
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```

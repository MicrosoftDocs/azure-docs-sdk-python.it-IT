---
title: Mutli-cloud
description: Usare Azure in tutte le aree
author: lmazuel
ms.author: lmazuel
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 6d2ba0580f8b6dda857b48ed5235a8c969a051f5
ms.sourcegitcommit: 7066ace94076483bae7d54172605f431e47bd5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/05/2018
---
# <a name="multi-cloud---use-azure-on-all-regions"></a><span data-ttu-id="d8d95-103">Multi-cloud - Usare Azure in tutte le aree</span><span class="sxs-lookup"><span data-stu-id="d8d95-103">Multi-cloud - use Azure on all regions</span></span>

<span data-ttu-id="d8d95-104">È possibile usare Azure SDK per Python per connettersi a tutte le aree in cui è disponibile [Azure](https://azure.microsoft.com/regions/services).</span><span class="sxs-lookup"><span data-stu-id="d8d95-104">You can use the Azure SDK for Python to connect to all regions where Azure is [available](https://azure.microsoft.com/regions/services).</span></span>

<span data-ttu-id="d8d95-105">Per impostazione predefinita, Azure SDK per Python è configurato per la connessione ad Azure pubblico.</span><span class="sxs-lookup"><span data-stu-id="d8d95-105">By default, the Azure SDK for Python is configured to connect to public Azure.</span></span>

## <a name="using-predeclared-cloud-definition"></a><span data-ttu-id="d8d95-106">Uso di una definizione di cloud predichiarata</span><span class="sxs-lookup"><span data-stu-id="d8d95-106">Using predeclared cloud definition</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d8d95-107">La versione del pacchetto `msrestazure` deve essere pari o superiore a 0.4.11 per questa sezione.</span><span class="sxs-lookup"><span data-stu-id="d8d95-107">The `msrestazure` package must be superior or equals to 0.4.11 for this section.</span></span>

<span data-ttu-id="d8d95-108">È possibile usare il modulo `azure_cloud` di `msrestazure`</span><span class="sxs-lookup"><span data-stu-id="d8d95-108">You can use the `azure_cloud` module of `msrestazure`</span></span>

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=AZURE_CHINA_CLOUD
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager
)
``` 
  
<span data-ttu-id="d8d95-109">Le definizioni di cloud disponibili sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="d8d95-109">Available cloud definition are</span></span>
  - <span data-ttu-id="d8d95-110">AZURE_PUBLIC_CLOUD</span><span class="sxs-lookup"><span data-stu-id="d8d95-110">AZURE_PUBLIC_CLOUD</span></span>
  - <span data-ttu-id="d8d95-111">AZURE_CHINA_CLOUD</span><span class="sxs-lookup"><span data-stu-id="d8d95-111">AZURE_CHINA_CLOUD</span></span>
  - <span data-ttu-id="d8d95-112">AZURE_US_GOV_CLOUD</span><span class="sxs-lookup"><span data-stu-id="d8d95-112">AZURE_US_GOV_CLOUD</span></span>
  - <span data-ttu-id="d8d95-113">AZURE_GERMAN_CLOUD</span><span class="sxs-lookup"><span data-stu-id="d8d95-113">AZURE_GERMAN_CLOUD</span></span>

## <a name="using-your-own-cloud-definition-eg-azure-stack"></a><span data-ttu-id="d8d95-114">Uso di una definizione di cloud personalizzata, ad esempio Azure Stack</span><span class="sxs-lookup"><span data-stu-id="d8d95-114">Using your own cloud definition (e.g. Azure Stack)</span></span>
<span data-ttu-id="d8d95-115">Azure Resource Manager mette a disposizione un endpoint metadati per semplificare l'operazione:</span><span class="sxs-lookup"><span data-stu-id="d8d95-115">ARM has a metadata endpoint to help you:</span></span>

```python
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

mystack_cloud = get_cloud_from_metadata_endpoint("https://myazurestack-arm-endpoint.com")
credentials = UserPassCredentials(
    login,
    password,
    cloud_environment=mystack_cloud
)
client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=mystack_cloud.endpoints.resource_manager
)
```
## <a name="using-adal"></a><span data-ttu-id="d8d95-116">Uso di ADAL</span><span class="sxs-lookup"><span data-stu-id="d8d95-116">Using ADAL</span></span>

<span data-ttu-id="d8d95-117">Per connettersi a un'altra area, è necessario prendere in considerazione alcuni aspetti:</span><span class="sxs-lookup"><span data-stu-id="d8d95-117">To connect to another region, a few things have to be considered:</span></span>

- <span data-ttu-id="d8d95-118">A quale endpoint è necessario chiedere un token (autenticazione)?</span><span class="sxs-lookup"><span data-stu-id="d8d95-118">What is the endpoint where to ask for a token (authentication)?</span></span>
- <span data-ttu-id="d8d95-119">In quale endpoint verrà usato questo token (utilizzo)?</span><span class="sxs-lookup"><span data-stu-id="d8d95-119">What is the endpoint where I will use this token (usage)?</span></span>

<span data-ttu-id="d8d95-120">Questo è un esempio generico:</span><span class="sxs-lookup"><span data-stu-id="d8d95-120">This is a generic example:</span></span>

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Public Azure - default values
authentication_endpoint = 'https://login.microsoftonline.com/'
azure_endpoint = 'https://management.azure.com/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-government"></a><span data-ttu-id="d8d95-121">Azure Government</span><span class="sxs-lookup"><span data-stu-id="d8d95-121">Azure Government</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Government
authentication_endpoint = 'https://login-us.microsoftonline.com/'
azure_endpoint = 'https://management.usgovcloudapi.net/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-germany"></a><span data-ttu-id="d8d95-122">Azure Germania</span><span class="sxs-lookup"><span data-stu-id="d8d95-122">Azure Germany</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure Germany
authentication_endpoint = 'https://login.microsoftonline.de/'
azure_endpoint = 'https://management.microsoftazure.de/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

### <a name="azure-china"></a><span data-ttu-id="d8d95-123">Azure Cina</span><span class="sxs-lookup"><span data-stu-id="d8d95-123">Azure China</span></span>
```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure China
authentication_endpoint = 'https://login.chinacloudapi.cn/'
azure_endpoint = 'https://management.chinacloudapi.cn/'
    
context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(
    context.acquire_token_with_client_credentials,
    azure_endpoint,
    client_id,
    password
)
subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(
    credentials,
    subscription_id,
    base_url=azure_endpoint
)
```

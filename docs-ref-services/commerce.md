---
title: Librerie di e-commerce di Azure per Python
description: Informazioni di riferimento sulle librerie di e-commerce di Azure per Python
keywords: Azure, Python, SDK, API, e-commerce
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 27b826c1f11aca0d8c49c4e8eab4277b857eea37
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277233"
---
# <a name="azure-commerce-libraries-for-python"></a><span data-ttu-id="c5da8-104">Librerie di e-commerce di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="c5da8-104">Azure Commerce libraries for python</span></span>

## <a name="management-apipythonapioverviewazurecommercemanagement"></a>[<span data-ttu-id="c5da8-105">API di gestione</span><span class="sxs-lookup"><span data-stu-id="c5da8-105">Management API</span></span>](/python/api/overview/azure/commerce/management)

```bash
pip install azure-mgmt-commerce
```
## <a name="create-the-commerce-client"></a><span data-ttu-id="c5da8-106">Creare il client di e-commerce</span><span class="sxs-lookup"><span data-stu-id="c5da8-106">Create the commerce client</span></span>

<span data-ttu-id="c5da8-107">Il codice seguente crea un'istanza del client di gestione.</span><span class="sxs-lookup"><span data-stu-id="c5da8-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="c5da8-108">Sarà necessario specificare il proprio ``subscription_id``, recuperabile dall'[elenco delle sottoscrizioni](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span><span class="sxs-lookup"><span data-stu-id="c5da8-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="c5da8-109">Vedere [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) (Autenticazione di gestione risorse) per informazioni dettagliate sulla gestione dell'autenticazione di Azure Active Directory con Python SDK e sulla creazione di un'istanza di ``Credentials``.</span><span class="sxs-lookup"><span data-stu-id="c5da8-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.commerce import UsageManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

commerce_client = UsageManagementClient(
    credentials,
    subscription_id
)
``` 

## <a name="get-rate-card"></a><span data-ttu-id="c5da8-110">Ottenere il tariffario pubblicitario</span><span class="sxs-lookup"><span data-stu-id="c5da8-110">Get rate card</span></span>

```python
# OfferDurableID: https://azure.microsoft.com/en-us/support/legal/offer-details/
rate = commerce_client.rate_card.get(
    "OfferDurableId eq 'MS-AZR-0062P' and Currency eq 'USD' and Locale eq 'en-US' and RegionInfo eq 'US'"
)
```

## <a name="get-usage"></a><span data-ttu-id="c5da8-111">Ottenere l'utilizzo</span><span class="sxs-lookup"><span data-stu-id="c5da8-111">Get Usage</span></span>

```python
from datetime import date, timedelta

# Takes onky dates in full ISO8601 with 'T00:00:00Z'
# Return an iterator like object: https://docs.python.org/3/library/stdtypes.html#iterator-types
usage_iterator = commerce_client.usage_aggregates.list(
    str(date.today() - timedelta(days=1))+'T00:00:00Z',
    str(date.today())+'T00:00:00Z'
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="c5da8-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="c5da8-112">Explore the Management APIs</span></span>](/python/api/overview/azure/commerce/management)
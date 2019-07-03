---
title: Librerie di autorizzazione di Azure per Python
description: Informazioni di riferimento sulle librerie di autorizzazione di Azure per Python
keywords: Azure, Python, SDK, API, autorizzazione
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: c7c4af34e82f2baad39635ecd20f1b9c48900b41
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534361"
---
# <a name="azure-authorization-libraries-for-python"></a><span data-ttu-id="9e819-104">Librerie di autorizzazione di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="9e819-104">Azure Authorization libraries for python</span></span>

## <a name="management-apipythonapioverviewazureauthorizationmanagement"></a>[<span data-ttu-id="9e819-105">API di gestione</span><span class="sxs-lookup"><span data-stu-id="9e819-105">Management API</span></span>](/python/api/overview/azure/authorization/management)

```bash
pip install azure-mgmt-authorization
```

## <a name="create-the-management-client"></a><span data-ttu-id="9e819-106">Creare il client di gestione</span><span class="sxs-lookup"><span data-stu-id="9e819-106">Create the management client</span></span>

<span data-ttu-id="9e819-107">Il codice seguente crea un'istanza del client di gestione.</span><span class="sxs-lookup"><span data-stu-id="9e819-107">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="9e819-108">Sarà necessario specificare il proprio ``subscription_id``, recuperabile dall'[elenco delle sottoscrizioni](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span><span class="sxs-lookup"><span data-stu-id="9e819-108">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="9e819-109">Vedere [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) (Autenticazione di gestione risorse) per informazioni dettagliate sulla gestione dell'autenticazione di Azure Active Directory con Python SDK e sulla creazione di un'istanza di ``Credentials``.</span><span class="sxs-lookup"><span data-stu-id="9e819-109">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.authorization import AuthorizationManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password'       # Your password
)

authorization_client = AuthorizationManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-permissions-for-a-resource-group"></a><span data-ttu-id="9e819-110">Verificare le autorizzazioni per un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="9e819-110">Check permissions for a resource group</span></span>

<span data-ttu-id="9e819-111">Il codice seguente verifica le autorizzazioni in un dato gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="9e819-111">The following code checks permissions in a given resource group.</span></span> <span data-ttu-id="9e819-112">Per creare o gestire i gruppi di risorse,vedere [Gestione risorse](/python/api/overview/azure/azure.mgmt.resource).</span><span class="sxs-lookup"><span data-stu-id="9e819-112">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

group_name = 'myresourcegroup'
permissions = self.authorization_client.permissions.list_for_resource_group(
    group_name
)
# permissions is a iterable of Permissions instances
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="9e819-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="9e819-113">Explore the Management APIs</span></span>](/python/api/overview/azure/authorization/management)

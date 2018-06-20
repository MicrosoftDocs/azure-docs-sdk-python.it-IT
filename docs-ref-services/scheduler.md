---
title: Librerie dell'Utilità di pianificazione di Azure per Python
description: Informazioni di riferimento sulle librerie dell'Utilità di pianificazione di Azure per Python
keywords: Azure, Python, SDK, API, Utilità di pianificazione
author: lisawong19
ms.author: liwong
manager: mbaldwin
ms.date: 02/21/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 3d2691ae1ba84c41f25de2b099aacefaa92152ed
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551614"
---
# <a name="azure-scheduler-libraries-for-python"></a><span data-ttu-id="48edb-104">Librerie dell'Utilità di pianificazione di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="48edb-104">Azure Scheduler libraries for python</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="48edb-105">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="48edb-105">Install the libraries</span></span>

## <a name="management"></a><span data-ttu-id="48edb-106">Gestione</span><span class="sxs-lookup"><span data-stu-id="48edb-106">Management</span></span>

```bash
pip install azure-mgmt-scheduler
```
## <a name="example"></a><span data-ttu-id="48edb-107">Esempio</span><span class="sxs-lookup"><span data-stu-id="48edb-107">Example</span></span>

### <a name="create-the-management-client"></a><span data-ttu-id="48edb-108">Creare il client di gestione</span><span class="sxs-lookup"><span data-stu-id="48edb-108">Create the management client</span></span>

<span data-ttu-id="48edb-109">Il codice seguente crea un'istanza del client di gestione.</span><span class="sxs-lookup"><span data-stu-id="48edb-109">The following code creates an instance of the management client.</span></span>

<span data-ttu-id="48edb-110">Sarà necessario specificare il proprio ``subscription_id``, recuperabile dall'[elenco delle sottoscrizioni](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span><span class="sxs-lookup"><span data-stu-id="48edb-110">You will need to provide your ``subscription_id`` which can be retrieved from [your subscription list](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).</span></span>

<span data-ttu-id="48edb-111">Vedere [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) (Autenticazione di gestione risorse) per informazioni dettagliate sulla gestione dell'autenticazione di Azure Active Directory con Python SDK e sulla creazione di un'istanza di ``Credentials``.</span><span class="sxs-lookup"><span data-stu-id="48edb-111">See [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) for details on handling Azure Active Directory authentication with the Python SDK, and creating a ``Credentials`` instance.</span></span>

```python
from azure.mgmt.scheduler import SchedulerManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

scheduler_client = SchedulerManagementClient(
    credentials,
    subscription_id
)
```

### <a name="create-a-job-collection"></a><span data-ttu-id="48edb-112">Creare una raccolta di processi</span><span class="sxs-lookup"><span data-stu-id="48edb-112">Create a job collection</span></span>

<span data-ttu-id="48edb-113">Il codice seguente crea una raccolta di processi in un gruppo di risorse esistente.</span><span class="sxs-lookup"><span data-stu-id="48edb-113">The following code creates a job collection under an existing resource group.</span></span>
<span data-ttu-id="48edb-114">Per creare o gestire i gruppi di risorse,vedere [Gestione risorse](/python/api/overview/azure/azure.mgmt.resource).</span><span class="sxs-lookup"><span data-stu-id="48edb-114">To create or manage resource groups, see [Resource Management](/python/api/overview/azure/azure.mgmt.resource).</span></span>

```python
from azure.mgmt.scheduler.models import JobCollectionDefinition, JobCollectionProperties, Sku

group_name = 'myresourcegroup'
job_collection_name = "myjobcollection"
scheduler_client.job_collections.create_or_update(
    group_name,
    job_collection_name,
    JobCollectionDefinition(
        location = "West US",
        properties = JobCollectionProperties(
            sku = Sku(
                name="Free"
            )
        )
    )
)
# scheduler_client is a JobCollectionDefinition instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="48edb-115">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="48edb-115">Explore the Management APIs</span></span>](/python/api/overview/azure/scheduler/management)
---
title: Librerie della Griglia di eventi di Azure per Python
description: 
keywords: Azure, Python, SDK, API, Griglia di eventi
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: a50a203a0733f25f2a88d6f4a43c6bddc388d3e7
ms.sourcegitcommit: 79afc8a1b427e26ecea7bdc0b7b3c898f143360f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2017
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="975f6-103">Librerie della Griglia di eventi per Python</span><span class="sxs-lookup"><span data-stu-id="975f6-103">Event Grid libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="975f6-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="975f6-104">Overview</span></span>
<span data-ttu-id="975f6-105">Griglia di eventi di Azure è un servizio di routing di eventi intelligente completamente gestito che consente un uso degli eventi uniforme tramite un modello di pubblicazione-sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="975f6-105">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

## <a name="management-api"></a><span data-ttu-id="975f6-106">API di gestione</span><span class="sxs-lookup"><span data-stu-id="975f6-106">Management API</span></span>
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="975f6-107">Esempio</span><span class="sxs-lookup"><span data-stu-id="975f6-107">Example</span></span>
<span data-ttu-id="975f6-108">L'esempio seguente crea un argomento personalizzato, sottoscrive l'argomento e attiva l'evento per la visualizzazione del risultato.</span><span class="sxs-lookup"><span data-stu-id="975f6-108">The following creates a custom topic, subscribes to the topic, and triggers the event to view the result.</span></span> <span data-ttu-id="975f6-109">RequestBin è uno strumento open source di terze parti che consente di creare un endpoint e visualizza le richieste che gli vengono inviate.</span><span class="sxs-lookup"><span data-stu-id="975f6-109">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="975f6-110">Passare a [RequestBin](https://requestb.in/) e fare clic su **Create a RequestBin** (Crea un RequestBin).</span><span class="sxs-lookup"><span data-stu-id="975f6-110">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="975f6-111">Copiare l'URL del contenitore, necessario per sottoscrivere l'argomento.</span><span class="sxs-lookup"><span data-stu-id="975f6-111">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.eventgrid import EventGridManagementClient
import requests

RESOURCE_GROUP_NAME = 'gridResourceGroup'
TOPIC_NAME = 'gridTopic1234'
LOCATION = 'westus2'
SUBSCRIPTION_ID = 'YOUR_SUBSCRIPTION_ID'
SUBSCRIPTION_NAME = 'gridSubscription'
REQUEST_BIN_URL = 'YOUR_REQUEST_BIN_URL'

# create resource group
resource_client = ResourceManagementClient(credentials, SUBSCRIPTION_ID)
resource_client.resource_groups.create_or_update(
    RESOURCE_GROUP_NAME,
    {
        'location': LOCATION
    }
)

event_client = EventGridManagementClient(credentials, SUBSCRIPTION_ID)

# create a custom topic
event_client.topics.create_or_update(RESOURCE_GROUP_NAME, TOPIC_NAME, LOCATION)

# subscribe to a topic
scope = '/subscriptions/'+SUBSCRIPTION_ID+'/resourceGroups/'+RESOURCE_GROUP_NAME+'/providers/Microsoft.EventGrid/topics/'+TOPIC_NAME
event_client.event_subscriptions.create(scope, SUBSCRIPTION_NAME,
    {
        'destination': {
            'endpoint_url': REQUEST_BIN_URL
        }
    }
)

# send an event to topic
# get endpoint url
url = event_client.event_subscriptions.get_full_url(scope, SUBSCRIPTION_NAME).endpoint_url
# get key
key = event_client.topics.list_shared_access_keys(RESOURCE_GROUP_NAME,TOPIC_NAME).key1
headers = {'aeg-sas-key': key}
s = requests.get('https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/customevent.json')
r = requests.post(url, data=s, headers=headers)
print(r.status_code)
print(r.content)
```
<span data-ttu-id="975f6-112">Passare all'URL di RequestBin creato in precedenza per visualizzare l'evento appena inviato.</span><span class="sxs-lookup"><span data-stu-id="975f6-112">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="975f6-113">Pulire le risorse</span><span class="sxs-lookup"><span data-stu-id="975f6-113">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="975f6-114">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="975f6-114">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/managementlibrary)


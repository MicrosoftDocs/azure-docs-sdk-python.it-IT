---
title: Librerie della Griglia di eventi di Azure per Python
description: ''
keywords: Azure, Python, SDK, API, Griglia di eventi
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/21/2017
ms.topic: article
ms.devlang: python
ms.service: event-grid
ms.openlocfilehash: bfaa1908295eb77531e399f1337acdeee512005f
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276835"
---
# <a name="event-grid-libraries-for-python"></a><span data-ttu-id="3e0bd-103">Librerie della Griglia di eventi per Python</span><span class="sxs-lookup"><span data-stu-id="3e0bd-103">Event Grid libraries for Python</span></span>


<span data-ttu-id="3e0bd-104">Griglia di eventi di Azure è un servizio di routing di eventi intelligente completamente gestito che consente un uso degli eventi uniforme tramite un modello di pubblicazione-sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-104">Azure Event Grid is a fully-managed intelligent event routing service that allows for uniform event consumption using a publish-subscribe model.</span></span>

<span data-ttu-id="3e0bd-105">[Altre informazioni](/azure/event-grid/overview) su Griglia di eventi di Azure e introduzione all'[esercitazione sugli eventi di archiviazione BLOB di Azure](/azure/storage/blobs/storage-blob-event-quickstart).</span><span class="sxs-lookup"><span data-stu-id="3e0bd-105">[Learn more](/azure/event-grid/overview) about Azure Event Grid and get started with the [Azure Blob storage event tutorial](/azure/storage/blobs/storage-blob-event-quickstart).</span></span> 

## <a name="publish-sdk"></a><span data-ttu-id="3e0bd-106">SDK di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="3e0bd-106">Publish SDK</span></span>

<span data-ttu-id="3e0bd-107">Eseguire l'autenticazione, creare, gestire e pubblicare eventi negli argomenti usando l'SDK di pubblicazione di Griglia di eventi di Azure.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-107">Authenticate, create, handle, and publish events to topics using the Azure Event Grid publish SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="3e0bd-108">Installazione</span><span class="sxs-lookup"><span data-stu-id="3e0bd-108">Installation</span></span> 

<span data-ttu-id="3e0bd-109">Installare il pacchetto usando [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="3e0bd-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-eventgrid
```

### <a name="example"></a><span data-ttu-id="3e0bd-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="3e0bd-110">Example</span></span> 

<span data-ttu-id="3e0bd-111">Il codice seguente pubblica un evento in un argomento.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-111">The following code publishes an event to a topic.</span></span> <span data-ttu-id="3e0bd-112">È possibile recuperare la chiave e l'endpoint dell'argomento dal portale di Azure o tramite l'interfaccia della riga di comando di Azure:</span><span class="sxs-lookup"><span data-stu-id="3e0bd-112">You can retrieve the topic key and endpoint through the Azure Portal or through the Azure CLI:</span></span>

```azurecli-interactive
endpoint=$(az eventgrid topic show --name <topic_name> -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name <topic_name> -g gridResourceGroup --query "key1" --output tsv)
```

```python
from datetime import datetime
from azure.eventgrid import EventGridClient
from msrest.authentication import TopicCredentials

def publish_event(self):

        credentials = TopicCredentials(
            self.settings.EVENT_GRID_KEY
        )
        event_grid_client = EventGridClient(credentials)
        event_grid_client.publish_events(
            "your-endpoint-here",
            events=[{
                'id' : "dbf93d79-3859-4cac-8055-51e3b6b54bea",
                'subject' : "Sample subject",
                'data': {
                    'key': 'Sample Data'
                },
                'event_type': 'SampleEventType',
                'event_time': datetime(2018, 5, 2),
                'data_version': 1
            }]
        )
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3e0bd-113">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="3e0bd-113">Explore the Client APIs</span></span>](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a><span data-ttu-id="3e0bd-114">SDK di gestione</span><span class="sxs-lookup"><span data-stu-id="3e0bd-114">Management SDK</span></span>

<span data-ttu-id="3e0bd-115">Creare, aggiornare o eliminare istanze, argomenti e sottoscrizioni di Griglia di eventi con l'SDK di gestione.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-115">Create, update, or delete Event Grid instances, topics, and subscriptions with the management SDK.</span></span>

### <a name="installation"></a><span data-ttu-id="3e0bd-116">Installazione</span><span class="sxs-lookup"><span data-stu-id="3e0bd-116">Installation</span></span> 

<span data-ttu-id="3e0bd-117">Installare il pacchetto usando [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="3e0bd-117">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a><span data-ttu-id="3e0bd-118">Esempio</span><span class="sxs-lookup"><span data-stu-id="3e0bd-118">Example</span></span>

<span data-ttu-id="3e0bd-119">L'esempio seguente crea un argomento personalizzato e iscrive un endpoint all'argomento.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-119">The following creates a custom topic and subscribes an endpoint to the topic.</span></span> <span data-ttu-id="3e0bd-120">Il codice invia quindi un evento all'argomento tramite HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-120">The code then sends an event to the topic through HTTPS.</span></span>
<span data-ttu-id="3e0bd-121">RequestBin è uno strumento open source di terze parti che consente di creare un endpoint e visualizza le richieste che gli vengono inviate.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-121">RequestBin is an open source, third-party tool that enables you to create an endpoint, and view requests that are sent to it.</span></span> <span data-ttu-id="3e0bd-122">Passare a [RequestBin](https://requestb.in/) e fare clic su **Create a RequestBin** (Crea un RequestBin).</span><span class="sxs-lookup"><span data-stu-id="3e0bd-122">Go to [RequestBin](https://requestb.in/), and click **Create a RequestBin**.</span></span> <span data-ttu-id="3e0bd-123">Copiare l'URL del contenitore, necessario per sottoscrivere l'argomento.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-123">Copy the bin URL, because you need it when subscribing to the topic.</span></span>

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
<span data-ttu-id="3e0bd-124">Passare all'URL di RequestBin creato in precedenza per visualizzare l'evento appena inviato.</span><span class="sxs-lookup"><span data-stu-id="3e0bd-124">Browse to the RequestBin URL created earlier to see the event just sent.</span></span>

<span data-ttu-id="3e0bd-125">Pulire le risorse</span><span class="sxs-lookup"><span data-stu-id="3e0bd-125">Clean up resources</span></span>
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="3e0bd-126">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="3e0bd-126">Explore the Management APIs</span></span>](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a><span data-ttu-id="3e0bd-127">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="3e0bd-127">Learn more</span></span>

<span data-ttu-id="3e0bd-128">[Receive events using the Event Grid SDK](/azure/event-grid/receive-events) (Ricevere eventi tramite l'SDK di Griglia di eventi)</span><span class="sxs-lookup"><span data-stu-id="3e0bd-128">[Receive events using the Event Grid SDK](/azure/event-grid/receive-events)</span></span>

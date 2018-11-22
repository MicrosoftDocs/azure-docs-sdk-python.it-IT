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
# <a name="event-grid-libraries-for-python"></a>Librerie della Griglia di eventi per Python


Griglia di eventi di Azure è un servizio di routing di eventi intelligente completamente gestito che consente un uso degli eventi uniforme tramite un modello di pubblicazione-sottoscrizione.

[Altre informazioni](/azure/event-grid/overview) su Griglia di eventi di Azure e introduzione all'[esercitazione sugli eventi di archiviazione BLOB di Azure](/azure/storage/blobs/storage-blob-event-quickstart). 

## <a name="publish-sdk"></a>SDK di pubblicazione

Eseguire l'autenticazione, creare, gestire e pubblicare eventi negli argomenti usando l'SDK di pubblicazione di Griglia di eventi di Azure.

### <a name="installation"></a>Installazione 

Installare il pacchetto usando [pip](https://pip.pypa.io/en/stable/quickstart/):

```bash
pip install azure-eventgrid
```

### <a name="example"></a>Esempio 

Il codice seguente pubblica un evento in un argomento. È possibile recuperare la chiave e l'endpoint dell'argomento dal portale di Azure o tramite l'interfaccia della riga di comando di Azure:

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
> [Esplorare le API client](/python/api/overview/azure/eventgrid/client)

## <a name="management-sdk"></a>SDK di gestione

Creare, aggiornare o eliminare istanze, argomenti e sottoscrizioni di Griglia di eventi con l'SDK di gestione.

### <a name="installation"></a>Installazione 

Installare il pacchetto usando [pip](https://pip.pypa.io/en/stable/quickstart/):

```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a>Esempio

L'esempio seguente crea un argomento personalizzato e iscrive un endpoint all'argomento. Il codice invia quindi un evento all'argomento tramite HTTPS.
RequestBin è uno strumento open source di terze parti che consente di creare un endpoint e visualizza le richieste che gli vengono inviate. Passare a [RequestBin](https://requestb.in/) e fare clic su **Create a RequestBin** (Crea un RequestBin). Copiare l'URL del contenitore, necessario per sottoscrivere l'argomento.

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
Passare all'URL di RequestBin creato in precedenza per visualizzare l'evento appena inviato.

Pulire le risorse
```azurecli-interactive
az group delete --name gridResourceGroup
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/eventgrid/management)

## <a name="learn-more"></a>Altre informazioni

[Receive events using the Event Grid SDK](/azure/event-grid/receive-events) (Ricevere eventi tramite l'SDK di Griglia di eventi)

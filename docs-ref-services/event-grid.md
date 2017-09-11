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
ms.openlocfilehash: 81c4e74b00ac59c789c5a0b83eaa10652ec6d8ac
ms.sourcegitcommit: db4608e494cb4340649bce98ba9fb4504d3686bb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="service-bus-libraries-for-python"></a>Librerie del bus di servizio per Python

## <a name="overview"></a>Panoramica
Griglia di eventi di Azure è un servizio di routing di eventi intelligente completamente gestito che consente un uso degli eventi uniforme tramite un modello di pubblicazione-sottoscrizione.

## <a name="management-api"></a>API di gestione
```bash
pip install azure-mgmt-eventgrid
```

### <a name="example"></a>Esempio
L'esempio seguente crea un argomento personalizzato, sottoscrive l'argomento e attiva l'evento per la visualizzazione del risultato. RequestBin è uno strumento open source di terze parti che consente di creare un endpoint e visualizza le richieste che gli vengono inviate. Passare a [RequestBin](https://requestb.in/) e fare clic su **Create a RequestBin** (Crea un RequestBin). Copiare l'URL del contenitore, necessario per sottoscrivere l'argomento.

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
> [Esplorare le API di gestione](/python/api/overview/azure/eventgrid/managementlibrary)


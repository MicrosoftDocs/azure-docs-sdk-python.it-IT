---
title: Librerie del bus di servizio per Python
description: Documentazione di riferimento per le librerie di gestione e client Python per il bus di servizio
keywords: Azure, Python, SDK, API, messaggistica, pubsub, pub-sub, message broker
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/21/2018
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 6c0bc66fbe8194b5b8f34ee8e29945b03ba8c242
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/24/2018
---
# <a name="service-bus-libraries-for-python"></a>Librerie del bus di servizio per Python

## <a name="overview"></a>Panoramica

Il bus di servizio di Microsoft Azure supporta un set di tecnologie middleware orientate ai messaggi e basate sul cloud, incluso l'accodamento dei messaggi affidabile e la messaggistica di pubblicazione e sottoscrizione permanente. 

## <a name="install-the-libraries"></a>Installare le librerie
```bash
pip install azure-mgmt-servicebus
```

## <a name="servicebus-queues"></a>Code del bus di servizio
Le code del bus di servizio sono un'alternativa alle code di archiviazione e possono essere utili negli scenari in cui sono necessarie funzionalità di messaggistica più avanzate (messaggi di dimensioni maggiori, ordinamento dei messaggi, letture distruttive a operazione singola, recapito pianificato) con stile di recapito push (tramite polling prolungato).

Il servizio può usare l'autenticazione con firma di accesso condiviso oppure l'autenticazione ACS.

Gli spazi dei nomi del bus di servizio creati con il portale di Azure dopo agosto 2014 non supportano più l'autenticazione ACS. È possibile creare spazi dei nomi compatibili con ACS con Azure SDK.

### <a name="shared-access-signature-authentication"></a>Autenticazione con firma di accesso condiviso

Per usare l'autenticazione con firma di accesso condiviso, creare il servizio del bus di servizio con il codice seguente:

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a>Autenticazione ACS

Per usare l'autenticazione ACS, creare il servizio del bus di servizio con il codice seguente:

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a>Invio e ricezione di messaggi

È possibile usare il metodo **create\_queue** per rendere disponibile una coda:

```python
sbs.create_queue('taskqueue')
```
È quindi possibile chiamare il metodo **send\_queue\_message** per inserire il messaggio nella coda:

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
È quindi possibile chiamare il metodo **send\_queue\_message_batch** per inviare più messaggi contemporaneamente:

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
È quindi possibile chiamare il metodo **receive\_queue\_message** per rimuovere il messaggio dalla coda.

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a>Argomenti del bus di servizio

Gli argomenti di bus di servizio sono un'astrazione basata sulle code del bus di servizio e semplificano l'implementazione di scenari di pubblicazione/sottoscrizione.

Per creare un argomento sul lato server è possibile usare il metodo **create\_topic**:

```python
sbs.create_topic('taskdiscussion')
```
Per inviare un messaggio a un argomento è possibile usare il metodo **send\_topic\_message**:

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

Per inviare più messaggi contemporaneamente è possibile usare il metodo **send\_topic\_message_batch**:

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

Si noti che un messaggio str avrà la codifica UTF-8 in Python 3 e sarà necessario gestire la codifica manualmente in Python 2.

Un client può quindi creare una sottoscrizione e iniziare a utilizzare i messaggi chiamando il metodo **create\_subscription** seguito dal metodo **receive\_subscription\_message**. Si noti che tutti i messaggi inviati prima della creazione della sottoscrizione non verranno ricevuti.

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a>Hub eventi

Hub eventi consente la raccolta di flussi di eventi a velocità effettiva elevata da diversi set di dispositivi e servizi.

Per creare un hub eventi è possibile usare il metodo **create\_event\_hub**:

```python
sbs.create_event_hub('myhub')
```
Per inviare un evento:

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
Il contenuto di un evento è il messaggio o la stringa con codifica JSON contenente più messaggi.

## <a name="advanced-features"></a>Funzionalità avanzate

### <a name="broker-properties-and-user-properties"></a>Proprietà Broker e proprietà User

Questa sezione descrive come usare le proprietà Broker e User definite [qui](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):

```python
sent_msg = Message(b'This is the third message',
                    broker_properties={'Label': 'M3'},
                    custom_properties={'Priority': 'Medium',
                                        'Customer': 'ABC'}
            )
```
È possibile usare valori datetime, int, float o booleani

```python
props = {'hello': 'world',
            'number': 42,
            'active': True,
            'deceased': False,
            'large': 8555111000,
            'floating': 3.14,
            'dob': datetime(2011, 12, 14),
            'double_quote_message': 'This "should" work fine',
            'quote_message': "This 'should' work fine"}
sent_msg = Message(b'message with properties', custom_properties=props)
```
Per motivi di compatibilità con la versione precedente di questa libreria, è possibile definire `broker_properties` anche come stringa JSON.
In questo caso sarà necessario scrivere una stringa JSON valida; Python non eseguirà alcuna verifica prima dell'invio all'API Rest.

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                    broker_properties = broker_properties
)
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/servicebus/management)
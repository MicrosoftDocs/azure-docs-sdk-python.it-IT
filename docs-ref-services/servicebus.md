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
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="e66fc-104">Librerie del bus di servizio per Python</span><span class="sxs-lookup"><span data-stu-id="e66fc-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="e66fc-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e66fc-105">Overview</span></span>

<span data-ttu-id="e66fc-106">Il bus di servizio di Microsoft Azure supporta un set di tecnologie middleware orientate ai messaggi e basate sul cloud, incluso l'accodamento dei messaggi affidabile e la messaggistica di pubblicazione e sottoscrizione permanente.</span><span class="sxs-lookup"><span data-stu-id="e66fc-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="e66fc-107">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="e66fc-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="servicebus-queues"></a><span data-ttu-id="e66fc-108">Code del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="e66fc-108">ServiceBus Queues</span></span>
<span data-ttu-id="e66fc-109">Le code del bus di servizio sono un'alternativa alle code di archiviazione e possono essere utili negli scenari in cui sono necessarie funzionalità di messaggistica più avanzate (messaggi di dimensioni maggiori, ordinamento dei messaggi, letture distruttive a operazione singola, recapito pianificato) con stile di recapito push (tramite polling prolungato).</span><span class="sxs-lookup"><span data-stu-id="e66fc-109">ServiceBus Queues are an alternative to Storage Queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

<span data-ttu-id="e66fc-110">Il servizio può usare l'autenticazione con firma di accesso condiviso oppure l'autenticazione ACS.</span><span class="sxs-lookup"><span data-stu-id="e66fc-110">The service can use Shared Access Signature authentication, or ACS authentication.</span></span>

<span data-ttu-id="e66fc-111">Gli spazi dei nomi del bus di servizio creati con il portale di Azure dopo agosto 2014 non supportano più l'autenticazione ACS.</span><span class="sxs-lookup"><span data-stu-id="e66fc-111">Service bus namespaces created using the Azure portal after August 2014 no longer support ACS authentication.</span></span> <span data-ttu-id="e66fc-112">È possibile creare spazi dei nomi compatibili con ACS con Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="e66fc-112">You can create ACS compatible namespaces with the Azure SDK.</span></span>

### <a name="shared-access-signature-authentication"></a><span data-ttu-id="e66fc-113">Autenticazione con firma di accesso condiviso</span><span class="sxs-lookup"><span data-stu-id="e66fc-113">Shared Access Signature Authentication</span></span>

<span data-ttu-id="e66fc-114">Per usare l'autenticazione con firma di accesso condiviso, creare il servizio del bus di servizio con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e66fc-114">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

### <a name="acs-authentication"></a><span data-ttu-id="e66fc-115">Autenticazione ACS</span><span class="sxs-lookup"><span data-stu-id="e66fc-115">ACS Authentication</span></span>

<span data-ttu-id="e66fc-116">Per usare l'autenticazione ACS, creare il servizio del bus di servizio con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e66fc-116">To use ACS authentication, create the service bus service with:</span></span>

```python
from azure.servicebus import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
### <a name="sending-and-receiving-messages"></a><span data-ttu-id="e66fc-117">Invio e ricezione di messaggi</span><span class="sxs-lookup"><span data-stu-id="e66fc-117">Sending and Receiving Messages</span></span>

<span data-ttu-id="e66fc-118">È possibile usare il metodo **create\_queue** per rendere disponibile una coda:</span><span class="sxs-lookup"><span data-stu-id="e66fc-118">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```
<span data-ttu-id="e66fc-119">È quindi possibile chiamare il metodo **send\_queue\_message** per inserire il messaggio nella coda:</span><span class="sxs-lookup"><span data-stu-id="e66fc-119">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
<span data-ttu-id="e66fc-120">È quindi possibile chiamare il metodo **send\_queue\_message_batch** per inviare più messaggi contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="e66fc-120">The **send\_queue\_message_batch** method can then be called to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
<span data-ttu-id="e66fc-121">È quindi possibile chiamare il metodo **receive\_queue\_message** per rimuovere il messaggio dalla coda.</span><span class="sxs-lookup"><span data-stu-id="e66fc-121">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

## <a name="servicebus-topics"></a><span data-ttu-id="e66fc-122">Argomenti del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="e66fc-122">ServiceBus Topics</span></span>

<span data-ttu-id="e66fc-123">Gli argomenti di bus di servizio sono un'astrazione basata sulle code del bus di servizio e semplificano l'implementazione di scenari di pubblicazione/sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="e66fc-123">ServiceBus topics are an abstraction on top of ServiceBus Queues that make pub/sub scenarios easy to implement.</span></span>

<span data-ttu-id="e66fc-124">Per creare un argomento sul lato server è possibile usare il metodo **create\_topic**:</span><span class="sxs-lookup"><span data-stu-id="e66fc-124">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```
<span data-ttu-id="e66fc-125">Per inviare un messaggio a un argomento è possibile usare il metodo **send\_topic\_message**:</span><span class="sxs-lookup"><span data-stu-id="e66fc-125">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="e66fc-126">Per inviare più messaggi contemporaneamente è possibile usare il metodo **send\_topic\_message_batch**:</span><span class="sxs-lookup"><span data-stu-id="e66fc-126">The **send\_topic\_message_batch** method can be used to send several messages at once:</span></span>

```python
from azure.servicebus import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="e66fc-127">Si noti che un messaggio str avrà la codifica UTF-8 in Python 3 e sarà necessario gestire la codifica manualmente in Python 2.</span><span class="sxs-lookup"><span data-stu-id="e66fc-127">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="e66fc-128">Un client può quindi creare una sottoscrizione e iniziare a utilizzare i messaggi chiamando il metodo **create\_subscription** seguito dal metodo **receive\_subscription\_message**.</span><span class="sxs-lookup"><span data-stu-id="e66fc-128">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="e66fc-129">Si noti che tutti i messaggi inviati prima della creazione della sottoscrizione non verranno ricevuti.</span><span class="sxs-lookup"><span data-stu-id="e66fc-129">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

## <a name="event-hub"></a><span data-ttu-id="e66fc-130">Hub eventi</span><span class="sxs-lookup"><span data-stu-id="e66fc-130">Event Hub</span></span>

<span data-ttu-id="e66fc-131">Hub eventi consente la raccolta di flussi di eventi a velocità effettiva elevata da diversi set di dispositivi e servizi.</span><span class="sxs-lookup"><span data-stu-id="e66fc-131">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="e66fc-132">Per creare un hub eventi è possibile usare il metodo **create\_event\_hub**:</span><span class="sxs-lookup"><span data-stu-id="e66fc-132">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```
<span data-ttu-id="e66fc-133">Per inviare un evento:</span><span class="sxs-lookup"><span data-stu-id="e66fc-133">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```
<span data-ttu-id="e66fc-134">Il contenuto di un evento è il messaggio o la stringa con codifica JSON contenente più messaggi.</span><span class="sxs-lookup"><span data-stu-id="e66fc-134">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

## <a name="advanced-features"></a><span data-ttu-id="e66fc-135">Funzionalità avanzate</span><span class="sxs-lookup"><span data-stu-id="e66fc-135">Advanced features</span></span>

### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="e66fc-136">Proprietà Broker e proprietà User</span><span class="sxs-lookup"><span data-stu-id="e66fc-136">Broker Properties and User Properties</span></span>

<span data-ttu-id="e66fc-137">Questa sezione descrive come usare le proprietà Broker e User definite [qui](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span><span class="sxs-lookup"><span data-stu-id="e66fc-137">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                    broker_properties={'Label': 'M3'},
                    custom_properties={'Priority': 'Medium',
                                        'Customer': 'ABC'}
            )
```
<span data-ttu-id="e66fc-138">È possibile usare valori datetime, int, float o booleani</span><span class="sxs-lookup"><span data-stu-id="e66fc-138">You can use datetime, int, float or boolean</span></span>

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
<span data-ttu-id="e66fc-139">Per motivi di compatibilità con la versione precedente di questa libreria, è possibile definire `broker_properties` anche come stringa JSON.</span><span class="sxs-lookup"><span data-stu-id="e66fc-139">For compatibility reason with old version of this library, `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="e66fc-140">In questo caso sarà necessario scrivere una stringa JSON valida; Python non eseguirà alcuna verifica prima dell'invio all'API Rest.</span><span class="sxs-lookup"><span data-stu-id="e66fc-140">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                    broker_properties = broker_properties
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e66fc-141">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="e66fc-141">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/management)
---
title: Librerie del bus di servizio per Python
description: Documentazione di riferimento per le librerie di gestione e client Python per il bus di servizio
keywords: Azure, Python, SDK, API, messaggistica, pubsub, pub-sub, message broker
author: annatisch
ms.author: antisch
manager: mayurid
ms.date: 01/15/2019
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: 540a2a248f7a2abcc83517bc4a4ef96ef03e8a9f
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747741"
---
# <a name="service-bus-libraries-for-python"></a>Librerie del bus di servizio per Python

Il bus di servizio di Microsoft Azure supporta un set di tecnologie middleware orientate ai messaggi e basate sul cloud, incluso l'accodamento dei messaggi affidabile e la messaggistica di pubblicazione e sottoscrizione permanente.

* [Codice sorgente dell'SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [Documentazione di riferimento sull'SDK](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a>Novità della versione 0.50.0
A partire dalla versione 0.50.0 è disponibile una nuova API basata su AMQP per l'invio e la ricezione di messaggi. Questo aggiornamento include **modifiche di rilievo**.
Per determinare se l'aggiornamento è attualmente opportuno per le proprie esigenze, vedere [Migrazione dalla versione 0.21.1 alla versione 0.50.0](#migration-from-v0211-to-v0500).

La nuova API basata su AMQP offre maggiore affidabilità nel passaggio dei messaggi, migliori prestazioni e l'estensione del supporto di funzionalità per il futuro.
La nuova API offre anche il supporto di operazioni asincrone (basato su asyncio) per l'invio, la ricezione e la gestione dei messaggi.

Per informazioni sulle operazioni basate su HTTP legacy, vedere [Uso delle operazioni basate su HTTP dell'API legacy](#using-http-based-operations-of-the-legacy-api).


## <a name="prerequisites"></a>Prerequisiti

* Sottoscrizione di Azure. [Creare un account gratuito](https://azure.microsoft.com/free/)
* [Credenziali di gestione e spazio dei nomi del bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal) di Azure
* [Python 2.7-3.7](https://www.python.org/downloads/)


## <a name="installation"></a>Installazione
```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a>Connettersi al bus di servizio di Azure

### <a name="get-credentials"></a>Ottenere le credenziali
Usare il frammento dell'interfaccia della riga di comando di Azure riportato di seguito per popolare una variabile di ambiente con la stringa di connessione del bus di servizio (questo valore è disponibile anche nel portale di Azure). Il frammento è presentato nel formato per la shell Bash.

```Bash
RES_GROUP=<resource-group-name>
NAMESPACE=<servicebus-namespace>

export SB_CONN_STR=$(az servicebus namespace authorization-rule keys list \
 --resource-group $RES_GROUP \
 --namespace-name $NAMESPACE \
 --name RootManageSharedAccessKey \
 --query primaryConnectionString \
 --output tsv)
```

### <a name="create-client"></a>Creare il client
Dopo aver popolato la variabile di ambiente `SB_CONN_STR`, è possibile creare ServiceBusClient.
```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```
Se si vogliono usare operazioni asincrone, usare lo spazio dei nomi `azure.servicebus.aio`.
```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```


## <a name="service-bus-queues"></a>Code del bus di servizio
Le code del bus di servizio sono un'alternativa alle code di archiviazione e possono essere utili negli scenari in cui sono necessarie funzionalità di messaggistica più avanzate (come messaggi di dimensioni maggiori, ordinamento dei messaggi, letture distruttive a operazione singola e recapito pianificato) con recapito di tipo push (tramite polling prolungato).

### <a name="create-queue"></a>Creare una coda
Il codice seguente crea una nuova coda nello spazio dei nomi del bus di servizio. Se nello spazio dei nomi esiste già una coda con lo stesso nome, verrà generato un errore. 
```python
sb_client.create_queue("MyQueue")
```
È anche possibile specificare parametri facoltativi per configurare il comportamento della coda.
```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a>Ottenere un client di coda
È possibile usare QueueClient per inviare e ricevere messaggi dalla coda, nonché per altre operazioni.
```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a>Invio di messaggi
Il client di coda può inviare uno o più messaggi contemporaneamente:
```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```
Ogni chiamata a QueueClient.send creerà una nuova connessione al servizio. Per riusare la stessa connessione per più chiamate di invio, è possibile aprire un mittente:
```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```
Se si usa un client asincrono, le operazioni precedenti useranno la sintassi di async:
```python
from azure.servicebus.aio import Message

message = Message("Hello World")
await queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
async with queue_client.get_sender() as sender:
    await sender.send(message_one)
    await sender.send(message_two)
```


### <a name="receiving-messages"></a>Ricezione di messaggi
È possibile ricevere messaggi da una coda in un iteratore continuo. La modalità predefinita per la ricezione dei messaggi è [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), con cui ogni messaggio deve essere completato in modo esplicito per essere rimosso dalla coda.
```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```
La connessione al servizio rimarrà aperta fino alla fine dell'iteratore.
Se l'iterazione del flusso di messaggi viene effettuata solo parzialmente, è consigliabile eseguire il ricevitore in un'istruzione `with` affinché la connessione venga chiusa:
```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
Se si usa un client asincrono, le operazioni precedenti useranno la sintassi di async:
```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a>Argomenti e sottoscrizioni del bus di servizio

Gli argomenti e le sottoscrizioni del bus di servizio sono un'astrazione basata sulle code del bus di servizio e offrono una forma di comunicazione uno-a-molti, con un schema di pubblicazione/sottoscrizione. I messaggi vengono inviati a un argomento e recapitati a una o più sottoscrizioni associate, supportando la scalabilità per un numero elevato di destinatari.

### <a name="create-topic"></a>Creare un argomento
Il codice seguente crea un nuovo argomento nello spazio dei nomi del bus di servizio. Se esiste già un argomento con lo stesso nome, verrà generato un errore. 
```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a>Ottenere un client di argomento
È possibile usare TopicClient per inviare messaggi all'argomento, nonché per altre operazioni.
```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a>Creare la sottoscrizione
Il codice seguente crea una nuova sottoscrizione per l'argomento specificato nello spazio dei nomi del bus di servizio.
```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a>Ottenere un client di sottoscrizione
È possibile usare SubscriptionClient per ricevere messaggi dall'argomento, nonché per altre operazioni.
```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a>Migrazione dalla versione 0.21.1 alla versione 0.50.0
Nella versione 0.50.0 sono state introdotte importanti modifiche di rilievo.
Nella versione 0.50.0 è ancora disponibile l'API basata su HTTP originale, ma si trova in un nuovo spazio dei nomi: `azure.servicebus.control_client`.

### <a name="should-i-upgrade"></a>È consigliabile eseguire l'aggiornamento?
Il nuovo pacchetto (versione 0.50.0) non offre nessun miglioramento rispetto alla versione 0.21.1 nelle operazioni basate su HTTP. L'API basata su HTTP è identica tranne per il fatto che si trova ora in un nuovo spazio dei nomi. Per questo motivo, se si vogliono usare solo operazioni basate su HTTP (`create_queue`, `delete_queue` e così via), l'aggiornamento non offrirà attualmente alcun vantaggio aggiuntivo.


### <a name="how-do-i-migrate-my-code-to-the-new-version"></a>Come si esegue la migrazione del codice alla nuova versione?
Per trasferire il codice scritto per la versione 0.21.0 alla versione 0.50.0 è sufficiente modificare lo spazio dei nomi di importazione:

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a>Uso delle operazioni basate su HTTP dell'API legacy
Le informazioni seguenti illustrano l'API legacy e sono destinate a coloro che vogliono trasferire il codice esistente alla versione 0.50.0 senza apportare modifiche aggiuntive. Queste informazioni di riferimento possono essere usate anche come indicazioni per gli utenti della versione 0.21.1.
Se si scrive nuovo codice, è consigliabile usare la nuova API descritta sopra.

### <a name="service-bus-queues"></a>Code del bus di servizio

#### <a name="shared-access-signature-sas-authentication"></a>Autenticazione con firma di accesso condiviso

Per usare l'autenticazione con firma di accesso condiviso, creare il servizio del bus di servizio con il codice seguente:

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a>Autenticazione con il Servizio di controllo di accesso (ACS)
Il Servizio di controllo di accesso non è più supportato nei nuovi spazi dei nomi del bus di servizio. È consigliabile eseguire la [migrazione delle applicazioni all'autenticazione con firma di accesso condiviso](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).
Per usare l'autenticazione ACS in uno spazio dei nomi precedente del bus di servizio, creare ServiceBusService con:

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```
#### <a name="sending-and-receiving-messages"></a>Invio e ricezione di messaggi

È possibile usare il metodo **create\_queue** per rendere disponibile una coda:

```python
sbs.create_queue('taskqueue')
```
È quindi possibile chiamare il metodo **send\_queue\_message** per inserire il messaggio nella coda:

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```
È quindi possibile chiamare il metodo **send\_queue\_message_batch** per inviare più messaggi contemporaneamente:

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```
È quindi possibile chiamare il metodo **receive\_queue\_message** per rimuovere il messaggio dalla coda.

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a>Argomenti del bus di servizio

Per creare un argomento sul lato server è possibile usare il metodo **create\_topic**:

```python
sbs.create_topic('taskdiscussion')
```
Per inviare un messaggio a un argomento è possibile usare il metodo **send\_topic\_message**:

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

Per inviare più messaggi contemporaneamente è possibile usare il metodo **send\_topic\_message_batch**:

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

Si noti che un messaggio str avrà la codifica UTF-8 in Python 3 e sarà necessario gestire la codifica manualmente in Python 2.

Un client può quindi creare una sottoscrizione e iniziare a utilizzare i messaggi chiamando il metodo **create\_subscription** seguito dal metodo **receive\_subscription\_message**. Si noti che tutti i messaggi inviati prima della creazione della sottoscrizione non verranno ricevuti.

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a>Hub eventi

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

### <a name="advanced-features"></a>Funzionalità avanzate

#### <a name="broker-properties-and-user-properties"></a>Proprietà broker e proprietà utente

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

## <a name="next-steps"></a>Passaggi successivi
* [documentazione sul bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging)
* [Codice sorgente dell'SDK](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [Documentazione di riferimento sull'SDK](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [Altri esempi](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)

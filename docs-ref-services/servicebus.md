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
ms.openlocfilehash: 014f34b6ba3d3a2a8ce15afcd826195d6051f98a
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376762"
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="f0f40-104">Librerie del bus di servizio per Python</span><span class="sxs-lookup"><span data-stu-id="f0f40-104">Service Bus libraries for Python</span></span>

<span data-ttu-id="f0f40-105">Il bus di servizio di Microsoft Azure supporta un set di tecnologie middleware orientate ai messaggi e basate sul cloud, incluso l'accodamento dei messaggi affidabile e la messaggistica di pubblicazione e sottoscrizione permanente.</span><span class="sxs-lookup"><span data-stu-id="f0f40-105">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span>

* [<span data-ttu-id="f0f40-106">Codice sorgente dell'SDK</span><span class="sxs-lookup"><span data-stu-id="f0f40-106">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="f0f40-107">Documentazione di riferimento sull'SDK</span><span class="sxs-lookup"><span data-stu-id="f0f40-107">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)

## <a name="whats-new-in-v0500"></a><span data-ttu-id="f0f40-108">Novità della versione 0.50.0</span><span class="sxs-lookup"><span data-stu-id="f0f40-108">What's new in v0.50.0?</span></span>

<span data-ttu-id="f0f40-109">A partire dalla versione 0.50.0 è disponibile una nuova API basata su AMQP per l'invio e la ricezione di messaggi.</span><span class="sxs-lookup"><span data-stu-id="f0f40-109">As of version 0.50.0 a new AMQP-based API is available for sending and receiving messages.</span></span> <span data-ttu-id="f0f40-110">Questo aggiornamento include **modifiche di rilievo**.</span><span class="sxs-lookup"><span data-stu-id="f0f40-110">This update involves **breaking changes**.</span></span>

<span data-ttu-id="f0f40-111">Per determinare se l'aggiornamento è attualmente opportuno per le proprie esigenze, vedere [Migrazione dalla versione 0.21.1 alla versione 0.50.0](#migration-from-v0211-to-v0500).</span><span class="sxs-lookup"><span data-stu-id="f0f40-111">Please read [Migration from v0.21.1 to v0.50.0](#migration-from-v0211-to-v0500) to determine if upgrading is right for you at this time.</span></span>

<span data-ttu-id="f0f40-112">La nuova API basata su AMQP offre maggiore affidabilità nel passaggio dei messaggi, migliori prestazioni e l'estensione del supporto di funzionalità per il futuro.</span><span class="sxs-lookup"><span data-stu-id="f0f40-112">The new AMQP-based API offers improved message passing reliability, performance and expanded feature support going forward.</span></span>
<span data-ttu-id="f0f40-113">La nuova API offre anche il supporto di operazioni asincrone (basato su asyncio) per l'invio, la ricezione e la gestione dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="f0f40-113">The new API also offers support for asynchronous operations (based on asyncio) for sending, receiving and handling messages.</span></span>

<span data-ttu-id="f0f40-114">Per informazioni sulle operazioni basate su HTTP legacy, vedere [Uso delle operazioni basate su HTTP dell'API legacy](#using-http-based-operations-of-the-legacy-api).</span><span class="sxs-lookup"><span data-stu-id="f0f40-114">For documentation on the legacy HTTP-based operations please see [Using HTTP-based operations of the legacy API](#using-http-based-operations-of-the-legacy-api).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0f40-115">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f0f40-115">Prerequisites</span></span>

* <span data-ttu-id="f0f40-116">Sottoscrizione di Azure. [Creare un account gratuito](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="f0f40-116">Azure subscription - [Create a free account](https://azure.microsoft.com/free/)</span></span>
* <span data-ttu-id="f0f40-117">[Credenziali di gestione e spazio dei nomi del bus di servizio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal) di Azure</span><span class="sxs-lookup"><span data-stu-id="f0f40-117">Azure [Service Bus namespace and management credentials](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal)</span></span>
* [<span data-ttu-id="f0f40-118">Python 2.7-3.7</span><span class="sxs-lookup"><span data-stu-id="f0f40-118">Python 2.7-3.7</span></span>](https://www.python.org/downloads/)

## <a name="installation"></a><span data-ttu-id="f0f40-119">Installazione</span><span class="sxs-lookup"><span data-stu-id="f0f40-119">Installation</span></span>

```bash
pip install azure-servicebus
```

## <a name="connect-to-azure-service-bus"></a><span data-ttu-id="f0f40-120">Connettersi al bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="f0f40-120">Connect to Azure Service Bus</span></span>

### <a name="get-credentials"></a><span data-ttu-id="f0f40-121">Ottenere le credenziali</span><span class="sxs-lookup"><span data-stu-id="f0f40-121">Get credentials</span></span>

<span data-ttu-id="f0f40-122">Usare il frammento dell'interfaccia della riga di comando di Azure riportato di seguito per popolare una variabile di ambiente con la stringa di connessione del bus di servizio (questo valore è disponibile anche nel portale di Azure).</span><span class="sxs-lookup"><span data-stu-id="f0f40-122">Use the Azure CLI snippet below to populate an environment variable with the Service Bus connection string (you can also find this value in the Azure portal).</span></span> <span data-ttu-id="f0f40-123">Il frammento è presentato nel formato per la shell Bash.</span><span class="sxs-lookup"><span data-stu-id="f0f40-123">The snippet is formatted for the Bash shell.</span></span>

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

### <a name="create-client"></a><span data-ttu-id="f0f40-124">Creare il client</span><span class="sxs-lookup"><span data-stu-id="f0f40-124">Create client</span></span>

<span data-ttu-id="f0f40-125">Dopo aver popolato la variabile di ambiente `SB_CONN_STR`, è possibile creare ServiceBusClient.</span><span class="sxs-lookup"><span data-stu-id="f0f40-125">Once you've populated the `SB_CONN_STR` environment variable, you can create the ServiceBusClient.</span></span>

```python
import os
from azure.servicebus import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

<span data-ttu-id="f0f40-126">Se si vogliono usare operazioni asincrone, usare lo spazio dei nomi `azure.servicebus.aio`.</span><span class="sxs-lookup"><span data-stu-id="f0f40-126">If you wish to use asynchronous operations, please use the `azure.servicebus.aio` namespace.</span></span>

```python
import os
from azure.servicebus.aio import ServiceBusClient

connection_str = os.environ['SB_CONN_STR']

sb_client = ServiceBusClient.from_connection_string(connection_str)
```

## <a name="service-bus-queues"></a><span data-ttu-id="f0f40-127">Code del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="f0f40-127">Service Bus queues</span></span>

<span data-ttu-id="f0f40-128">Le code del bus di servizio sono un'alternativa alle code di archiviazione e possono essere utili negli scenari in cui sono necessarie funzionalità di messaggistica più avanzate (come messaggi di dimensioni maggiori, ordinamento dei messaggi, letture distruttive a operazione singola e recapito pianificato) con recapito di tipo push (tramite polling prolungato).</span><span class="sxs-lookup"><span data-stu-id="f0f40-128">Service Bus queues are an alternative to Storage queues that might be useful in scenarios where more advanced messaging features are needed (larger message sizes, message ordering, single-operation destructive reads, scheduled delivery) using push-style delivery (using long polling).</span></span>

### <a name="create-queue"></a><span data-ttu-id="f0f40-129">Crea coda</span><span class="sxs-lookup"><span data-stu-id="f0f40-129">Create queue</span></span>

<span data-ttu-id="f0f40-130">Il codice seguente crea una nuova coda nello spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f0f40-130">This creates a new queue within the Service Bus namespace.</span></span> <span data-ttu-id="f0f40-131">Se nello spazio dei nomi esiste già una coda con lo stesso nome, verrà generato un errore.</span><span class="sxs-lookup"><span data-stu-id="f0f40-131">If a queue of the same name already exists within the namespace an error will be raised.</span></span>

```python
sb_client.create_queue("MyQueue")
```

<span data-ttu-id="f0f40-132">È anche possibile specificare parametri facoltativi per configurare il comportamento della coda.</span><span class="sxs-lookup"><span data-stu-id="f0f40-132">Optional parameters to configure the queue behavior can also be specified.</span></span>

```python
sb_client.create_queue(
    "MySessionQueue",
    requires_session=True  # Create a sessionful queue
    max_delivery_count=5  # Max delivery attempts per message
)
```

### <a name="get-a-queue-client"></a><span data-ttu-id="f0f40-133">Ottenere un client di coda</span><span class="sxs-lookup"><span data-stu-id="f0f40-133">Get a queue client</span></span>

<span data-ttu-id="f0f40-134">È possibile usare QueueClient per inviare e ricevere messaggi dalla coda, nonché per altre operazioni.</span><span class="sxs-lookup"><span data-stu-id="f0f40-134">A QueueClient can be used to send and receive messages from the queue, along with other operations.</span></span>

```python
queue_client = sb_client.get_queue("MyQueue")
```

### <a name="sending-messages"></a><span data-ttu-id="f0f40-135">Invio di messaggi</span><span class="sxs-lookup"><span data-stu-id="f0f40-135">Sending messages</span></span>

<span data-ttu-id="f0f40-136">Il client di coda può inviare uno o più messaggi contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="f0f40-136">The queue client can send one or more messages at a time:</span></span>

```python
from azure.servicebus import Message

message = Message("Hello World")
queue_client.send(message)

message_one = Message("First")
message_two = Message("Second")
queue_client.send([message_one, message_two])
```

<span data-ttu-id="f0f40-137">Ogni chiamata a QueueClient.send creerà una nuova connessione al servizio.</span><span class="sxs-lookup"><span data-stu-id="f0f40-137">Each call to QueueClient.send will create a new service connection.</span></span> <span data-ttu-id="f0f40-138">Per riusare la stessa connessione per più chiamate di invio, è possibile aprire un mittente:</span><span class="sxs-lookup"><span data-stu-id="f0f40-138">To reuse the same connection for multiple send calls, you can open a sender:</span></span>

```python
message_one = Message("First")
message_two = Message("Second")

with queue_client.get_sender() as sender:
    sender.send(message_one)
    sender.send(message_two)
```

<span data-ttu-id="f0f40-139">Se si usa un client asincrono, le operazioni precedenti useranno la sintassi di async:</span><span class="sxs-lookup"><span data-stu-id="f0f40-139">If you are using an asynchronous client, the above operations will use async syntax:</span></span>

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

### <a name="receiving-messages"></a><span data-ttu-id="f0f40-140">Ricezione di messaggi</span><span class="sxs-lookup"><span data-stu-id="f0f40-140">Receiving messages</span></span>

<span data-ttu-id="f0f40-141">È possibile ricevere messaggi da una coda in un iteratore continuo.</span><span class="sxs-lookup"><span data-stu-id="f0f40-141">Messages can be received from a queue as a continuous iterator.</span></span> <span data-ttu-id="f0f40-142">La modalità predefinita per la ricezione dei messaggi è [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), con cui ogni messaggio deve essere completato in modo esplicito per essere rimosso dalla coda.</span><span class="sxs-lookup"><span data-stu-id="f0f40-142">The default mode for message receiving is [PeekLock](https://docs.microsoft.com/rest/api/servicebus/peek-lock-message-non-destructive-read), which requires each message to be explicitly completed in order that it be removed from the queue.</span></span>

```python
messages = queue_client.get_receiver()
for message in messages:
    print(message)
    message.complete()
```

<span data-ttu-id="f0f40-143">La connessione al servizio rimarrà aperta fino alla fine dell'iteratore.</span><span class="sxs-lookup"><span data-stu-id="f0f40-143">The service connection will remain open for the entirety of the iterator.</span></span> <span data-ttu-id="f0f40-144">Se l'iterazione del flusso di messaggi viene effettuata solo parzialmente, è consigliabile eseguire il ricevitore in un'istruzione `with` affinché la connessione venga chiusa:</span><span class="sxs-lookup"><span data-stu-id="f0f40-144">If you find yourself only partially iterating the message stream, you should run the receiver in a `with` statement to ensure the connection is closed:</span></span>

```python
with queue_client.get_receiver() as messages:
    for message in messages:
        print(message)
        message.complete()
        break
```
<span data-ttu-id="f0f40-145">Se si usa un client asincrono, le operazioni precedenti useranno la sintassi di async:</span><span class="sxs-lookup"><span data-stu-id="f0f40-145">If you are using an asynchronous client, the above operations will use async syntax:</span></span>

```python
async with queue_client.get_receiver() as messages:
    async for message in messages:
        print(message)
        await message.complete()
        break
```

## <a name="service-bus-topics-and-subscriptions"></a><span data-ttu-id="f0f40-146">Argomenti e sottoscrizioni del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="f0f40-146">Service Bus topics and subscriptions</span></span>

<span data-ttu-id="f0f40-147">Gli argomenti e le sottoscrizioni del bus di servizio sono un'astrazione basata sulle code del bus di servizio e offrono una forma di comunicazione uno-a-molti, con un schema di pubblicazione/sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="f0f40-147">Service Bus topics and subscriptions are an abstraction on top of Service Bus queues that provide a one-to-many form of communication, in a publish/subscribe pattern.</span></span> <span data-ttu-id="f0f40-148">I messaggi vengono inviati a un argomento e recapitati a una o più sottoscrizioni associate, supportando la scalabilità per un numero elevato di destinatari.</span><span class="sxs-lookup"><span data-stu-id="f0f40-148">Messages are sent to a topic and delivered to one or more associated subscriptions, which is useful for scaling to large numbers of recipients.</span></span>

### <a name="create-topic"></a><span data-ttu-id="f0f40-149">Creare un argomento</span><span class="sxs-lookup"><span data-stu-id="f0f40-149">Create topic</span></span>

<span data-ttu-id="f0f40-150">Il codice seguente crea un nuovo argomento nello spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f0f40-150">This creates a new topic within the Service Bus namespace.</span></span> <span data-ttu-id="f0f40-151">Se esiste già un argomento con lo stesso nome, verrà generato un errore.</span><span class="sxs-lookup"><span data-stu-id="f0f40-151">If a topic of the same name already exists an error will be raised.</span></span>

```python
sb_client.create_topic("MyTopic")
```

### <a name="get-a-topic-client"></a><span data-ttu-id="f0f40-152">Ottenere un client di argomento</span><span class="sxs-lookup"><span data-stu-id="f0f40-152">Get a topic client</span></span>

<span data-ttu-id="f0f40-153">È possibile usare TopicClient per inviare messaggi all'argomento, nonché per altre operazioni.</span><span class="sxs-lookup"><span data-stu-id="f0f40-153">A TopicClient can be used to send messages to the topic, along with other operations.</span></span>

```python
topic_client = sb_client.get_topic("MyTopic")
```

### <a name="create-subscription"></a><span data-ttu-id="f0f40-154">Creare la sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="f0f40-154">Create subscription</span></span>

<span data-ttu-id="f0f40-155">Il codice seguente crea una nuova sottoscrizione per l'argomento specificato nello spazio dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f0f40-155">This creates a new subscription for the specified topic within the Service Bus namespace.</span></span>

```python
sb_client.create_subscription("MyTopic", "MySubscription")
```

### <a name="get-a-subscription-client"></a><span data-ttu-id="f0f40-156">Ottenere un client di sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="f0f40-156">Get a subscription client</span></span>

<span data-ttu-id="f0f40-157">È possibile usare SubscriptionClient per ricevere messaggi dall'argomento, nonché per altre operazioni.</span><span class="sxs-lookup"><span data-stu-id="f0f40-157">A SubscriptionClient can be used to receive messages from the topic, along with other operations.</span></span>

```python
topic_client = sb_client.get_subscription("MyTopic", "MySubscription")
```

## <a name="migration-from-v0211-to-v0500"></a><span data-ttu-id="f0f40-158">Migrazione dalla versione 0.21.1 alla versione 0.50.0</span><span class="sxs-lookup"><span data-stu-id="f0f40-158">Migration from v0.21.1 to v0.50.0</span></span>

<span data-ttu-id="f0f40-159">Nella versione 0.50.0 sono state introdotte importanti modifiche di rilievo.</span><span class="sxs-lookup"><span data-stu-id="f0f40-159">Major breaking changes were introduced in version 0.50.0.</span></span> <span data-ttu-id="f0f40-160">Nella versione 0.50.0 è ancora disponibile l'API basata su HTTP originale, ma si trova in un nuovo spazio dei nomi: `azure.servicebus.control_client`.</span><span class="sxs-lookup"><span data-stu-id="f0f40-160">The original HTTP-based API is still available in v0.50.0 - however it now exists under a new namesapce: `azure.servicebus.control_client`.</span></span>

### <a name="should-i-upgrade"></a><span data-ttu-id="f0f40-161">È consigliabile eseguire l'aggiornamento?</span><span class="sxs-lookup"><span data-stu-id="f0f40-161">Should I upgrade?</span></span>

<span data-ttu-id="f0f40-162">Il nuovo pacchetto (versione 0.50.0) non offre nessun miglioramento rispetto alla versione 0.21.1 nelle operazioni basate su HTTP.</span><span class="sxs-lookup"><span data-stu-id="f0f40-162">The new package (v0.50.0) offers no improvements in HTTP-based operations over v0.21.1.</span></span> <span data-ttu-id="f0f40-163">L'API basata su HTTP è identica tranne per il fatto che si trova ora in un nuovo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="f0f40-163">The HTTP-based API is identical except that it now exists under a new namespace.</span></span> <span data-ttu-id="f0f40-164">Per questo motivo, se si vogliono usare solo operazioni basate su HTTP (`create_queue`, `delete_queue` e così via), l'aggiornamento non offrirà attualmente alcun vantaggio aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="f0f40-164">For this reason if you only wish to use HTTP-based operations (`create_queue`, `delete_queue` etc) - there will be no additional benefit in upgrading at this time.</span></span>

### <a name="how-do-i-migrate-my-code-to-the-new-version"></a><span data-ttu-id="f0f40-165">Come si esegue la migrazione del codice alla nuova versione?</span><span class="sxs-lookup"><span data-stu-id="f0f40-165">How do I migrate my code to the new version?</span></span>

<span data-ttu-id="f0f40-166">Per trasferire il codice scritto per la versione 0.21.0 alla versione 0.50.0 è sufficiente modificare lo spazio dei nomi di importazione:</span><span class="sxs-lookup"><span data-stu-id="f0f40-166">Code written against v0.21.0 can be ported to version 0.50.0 by simply changing the import namespace:</span></span>

```python
# from azure.servicebus import ServiceBusService  <- This will now raise an ImportError
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

## <a name="using-http-based-operations-of-the-legacy-api"></a><span data-ttu-id="f0f40-167">Uso delle operazioni basate su HTTP dell'API legacy</span><span class="sxs-lookup"><span data-stu-id="f0f40-167">Using HTTP-based operations of the legacy API</span></span>

<span data-ttu-id="f0f40-168">Le informazioni seguenti illustrano l'API legacy e sono destinate a coloro che vogliono trasferire il codice esistente alla versione 0.50.0 senza apportare modifiche aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="f0f40-168">The following documentation describes the legacy API and should be used for those wishing to port existing code to v0.50.0 without making any additional changes.</span></span> <span data-ttu-id="f0f40-169">Queste informazioni di riferimento possono essere usate anche come indicazioni per gli utenti della versione 0.21.1.</span><span class="sxs-lookup"><span data-stu-id="f0f40-169">This reference can also be used as guidance by those using v0.21.1.</span></span>
<span data-ttu-id="f0f40-170">Se si scrive nuovo codice, è consigliabile usare la nuova API descritta sopra.</span><span class="sxs-lookup"><span data-stu-id="f0f40-170">For those writing new code, we recommend using the new API described above.</span></span>

### <a name="service-bus-queues"></a><span data-ttu-id="f0f40-171">Code del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="f0f40-171">Service Bus queues</span></span>

#### <a name="shared-access-signature-sas-authentication"></a><span data-ttu-id="f0f40-172">Autenticazione con firma di accesso condiviso</span><span class="sxs-lookup"><span data-stu-id="f0f40-172">Shared Access Signature (SAS) authentication</span></span>

<span data-ttu-id="f0f40-173">Per usare l'autenticazione con firma di accesso condiviso, creare il servizio del bus di servizio con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f0f40-173">To use Shared Access Signature authentication, create the service bus service with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

key_name = 'RootManageSharedAccessKey' # SharedAccessKeyName from Azure portal
key_value = '' # SharedAccessKey from Azure portal
sbs = ServiceBusService(service_namespace,
                        shared_access_key_name=key_name,
                        shared_access_key_value=key_value)
```

#### <a name="access-control-service-acs-authentication"></a><span data-ttu-id="f0f40-174">Autenticazione con il Servizio di controllo di accesso (ACS)</span><span class="sxs-lookup"><span data-stu-id="f0f40-174">Access Control Service (ACS) authentication</span></span>

<span data-ttu-id="f0f40-175">Il Servizio di controllo di accesso non è supportato nei nuovi spazi dei nomi del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f0f40-175">ACS is not supported on new Service Bus namespaces.</span></span> <span data-ttu-id="f0f40-176">È consigliabile eseguire la [migrazione delle applicazioni all'autenticazione con firma di accesso condiviso](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span><span class="sxs-lookup"><span data-stu-id="f0f40-176">We recommend [migrating applications to SAS authentication](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas).</span></span> <span data-ttu-id="f0f40-177">Per usare l'autenticazione ACS in uno spazio dei nomi precedente del bus di servizio, creare ServiceBusService con:</span><span class="sxs-lookup"><span data-stu-id="f0f40-177">To use ACS authentication within an older Service Bus namesapce, create the ServiceBusService with:</span></span>

```python
from azure.servicebus.control_client import ServiceBusService

account_key = '' # DEFAULT KEY from Azure portal
issuer = 'owner' # DEFAULT ISSUER from Azure portal
sbs = ServiceBusService(service_namespace,
                        account_key=account_key,
                        issuer=issuer)
```

#### <a name="sending-and-receiving-messages"></a><span data-ttu-id="f0f40-178">Invio e ricezione di messaggi</span><span class="sxs-lookup"><span data-stu-id="f0f40-178">Sending and receiving messages</span></span>

<span data-ttu-id="f0f40-179">È possibile usare il metodo **create\_queue** per rendere disponibile una coda:</span><span class="sxs-lookup"><span data-stu-id="f0f40-179">The **create\_queue** method can be used to ensure a queue exists:</span></span>

```python
sbs.create_queue('taskqueue')
```

<span data-ttu-id="f0f40-180">È quindi possibile chiamare il metodo **send\_queue\_message** per inserire il messaggio nella coda:</span><span class="sxs-lookup"><span data-stu-id="f0f40-180">The **send\_queue\_message** method can then be called to insert the message into the queue:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message('Hello World!')
sbs.send_queue_message('taskqueue', msg)
```

<span data-ttu-id="f0f40-181">È quindi possibile chiamare il metodo **send\_queue\_message_batch** per inviare più messaggi contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="f0f40-181">The **send\_queue\_message_batch** method can then be called to  send several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
```

<span data-ttu-id="f0f40-182">È quindi possibile chiamare il metodo **receive\_queue\_message** per rimuovere il messaggio dalla coda.</span><span class="sxs-lookup"><span data-stu-id="f0f40-182">It is then possible to call the **receive\_queue\_message** method to dequeue the message.</span></span>

```python
msg = sbs.receive_queue_message('taskqueue')
```

### <a name="service-bus-topics"></a><span data-ttu-id="f0f40-183">Argomenti del bus di servizio</span><span class="sxs-lookup"><span data-stu-id="f0f40-183">Service Bus topics</span></span>

<span data-ttu-id="f0f40-184">Per creare un argomento sul lato server è possibile usare il metodo **create\_topic**:</span><span class="sxs-lookup"><span data-stu-id="f0f40-184">The **create\_topic** method can be used to create a server-side topic:</span></span>

```python
sbs.create_topic('taskdiscussion')
```

<span data-ttu-id="f0f40-185">Per inviare un messaggio a un argomento è possibile usare il metodo **send\_topic\_message**:</span><span class="sxs-lookup"><span data-stu-id="f0f40-185">The **send\_topic\_message** method can be used to send a message to a topic:</span></span>

```python
from azure.servicebus.control_client import Message

msg = Message(b'Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
```

<span data-ttu-id="f0f40-186">Per inviare più messaggi contemporaneamente è possibile usare il metodo **send\_topic\_message_batch**:</span><span class="sxs-lookup"><span data-stu-id="f0f40-186">The **send\_topic\_message_batch** method can be used to send  several messages at once:</span></span>

```python
from azure.servicebus.control_client import Message

msg1 = Message(b'Hello World!')
msg2 = Message(b'Hello World again!')
sbs.send_topic_message_batch('taskdiscussion', [msg1, msg2])
```

<span data-ttu-id="f0f40-187">Si noti che un messaggio str avrà la codifica UTF-8 in Python 3 e sarà necessario gestire la codifica manualmente in Python 2.</span><span class="sxs-lookup"><span data-stu-id="f0f40-187">Please consider that in Python 3 a str message will be utf-8 encoded and you should have to manage your encoding yourself in Python 2.</span></span>

<span data-ttu-id="f0f40-188">Un client può quindi creare una sottoscrizione e iniziare a utilizzare i messaggi chiamando il metodo **create\_subscription** seguito dal metodo **receive\_subscription\_message**.</span><span class="sxs-lookup"><span data-stu-id="f0f40-188">A client can then create a subscription and start consuming messages by calling the **create\_subscription** method followed by the **receive\_subscription\_message** method.</span></span> <span data-ttu-id="f0f40-189">Si noti che tutti i messaggi inviati prima della creazione della sottoscrizione non verranno ricevuti.</span><span class="sxs-lookup"><span data-stu-id="f0f40-189">Please note that any messages sent before the subscription is created will not be received.</span></span>

```python
from azure.servicebus.control_client import Message

sbs.create_subscription('taskdiscussion', 'client1')
msg = Message('Hello World!')
sbs.send_topic_message('taskdiscussion', msg)
msg = sbs.receive_subscription_message('taskdiscussion', 'client1')
```

### <a name="event-hub"></a><span data-ttu-id="f0f40-190">Hub eventi</span><span class="sxs-lookup"><span data-stu-id="f0f40-190">Event Hub</span></span>

<span data-ttu-id="f0f40-191">Hub eventi consente la raccolta di flussi di eventi a velocità effettiva elevata da diversi set di dispositivi e servizi.</span><span class="sxs-lookup"><span data-stu-id="f0f40-191">Event Hubs enable the collection of event streams at high throughput, from a diverse set of devices and services.</span></span>

<span data-ttu-id="f0f40-192">Per creare un hub eventi è possibile usare il metodo **create\_event\_hub**:</span><span class="sxs-lookup"><span data-stu-id="f0f40-192">The **create\_event\_hub** method can be used to create an event hub:</span></span>

```python
sbs.create_event_hub('myhub')
```

<span data-ttu-id="f0f40-193">Per inviare un evento:</span><span class="sxs-lookup"><span data-stu-id="f0f40-193">To send an event:</span></span>

```python
sbs.send_event('myhub', '{ "DeviceId":"dev-01", "Temperature":"37.0" }')
```

<span data-ttu-id="f0f40-194">Il contenuto di un evento è il messaggio o la stringa con codifica JSON contenente più messaggi.</span><span class="sxs-lookup"><span data-stu-id="f0f40-194">The event content is the event message or JSON-encoded string that contains multiple messages.</span></span>

### <a name="advanced-features"></a><span data-ttu-id="f0f40-195">Funzionalità avanzate</span><span class="sxs-lookup"><span data-stu-id="f0f40-195">Advanced features</span></span>

#### <a name="broker-properties-and-user-properties"></a><span data-ttu-id="f0f40-196">Proprietà broker e proprietà utente</span><span class="sxs-lookup"><span data-stu-id="f0f40-196">Broker properties and user properties</span></span>

<span data-ttu-id="f0f40-197">Questa sezione descrive come usare le proprietà Broker e User definite [qui](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span><span class="sxs-lookup"><span data-stu-id="f0f40-197">This section describes how to use Broker and User properties defined [here](https://docs.microsoft.com/rest/api/servicebus/message-headers-and-properties):</span></span>

```python
sent_msg = Message(b'This is the third message',
                   broker_properties={'Label': 'M3'},
                   custom_properties={'Priority': 'Medium',
                                      'Customer': 'ABC'}
            )
```

<span data-ttu-id="f0f40-198">È possibile usare valori datetime, int, float o booleani</span><span class="sxs-lookup"><span data-stu-id="f0f40-198">You can use datetime, int, float or boolean</span></span>

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

<span data-ttu-id="f0f40-199">Per motivi di compatibilità con la versione precedente di questa libreria, è possibile definire `broker_properties` anche come stringa JSON.</span><span class="sxs-lookup"><span data-stu-id="f0f40-199">For compatibility reason with old version of this library,  `broker_properties` could also be defined as a JSON string.</span></span>
<span data-ttu-id="f0f40-200">In questo caso sarà necessario scrivere una stringa JSON valida; Python non eseguirà alcuna verifica prima dell'invio all'API Rest.</span><span class="sxs-lookup"><span data-stu-id="f0f40-200">If this situation, you're responsible to write a valid JSON string, no check will be made by Python before sending to the RestAPI.</span></span>

```python
broker_properties = '{"ForcePersistence": false, "Label": "My label"}'
sent_msg = Message(b'receive message',
                   broker_properties = broker_properties
)
```

## <a name="next-steps"></a><span data-ttu-id="f0f40-201">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="f0f40-201">Next Steps</span></span>

* [<span data-ttu-id="f0f40-202">documentazione sul bus di servizio</span><span class="sxs-lookup"><span data-stu-id="f0f40-202">Service Bus documentation</span></span>](https://docs.microsoft.com/azure/service-bus-messaging)
* [<span data-ttu-id="f0f40-203">Codice sorgente dell'SDK</span><span class="sxs-lookup"><span data-stu-id="f0f40-203">SDK source code</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus)
* [<span data-ttu-id="f0f40-204">Documentazione di riferimento sull'SDK</span><span class="sxs-lookup"><span data-stu-id="f0f40-204">SDK reference documentation</span></span>](https://docs.microsoft.com/python/api/overview/azure/servicebus/client?view=azure-python)
* [<span data-ttu-id="f0f40-205">Altri esempi</span><span class="sxs-lookup"><span data-stu-id="f0f40-205">Additional samples</span></span>](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-servicebus/examples)

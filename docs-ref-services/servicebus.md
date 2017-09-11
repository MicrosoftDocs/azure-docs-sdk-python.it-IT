---
title: Librerie del bus di servizio per Python
description: Documentazione di riferimento per le librerie di gestione e client Python per il bus di servizio
keywords: Azure, Python, SDK, API, messaggistica, pubsub, pub-sub, message broker
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: service-bus
ms.openlocfilehash: bf7be945f2c7a3daea93ff4e5b770459c00632c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="service-bus-libraries-for-python"></a><span data-ttu-id="4a56d-104">Librerie del bus di servizio per Python</span><span class="sxs-lookup"><span data-stu-id="4a56d-104">Service Bus libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="4a56d-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4a56d-105">Overview</span></span>

<span data-ttu-id="4a56d-106">Il bus di servizio di Microsoft Azure supporta un set di tecnologie middleware orientate ai messaggi e basate sul cloud, incluso l'accodamento dei messaggi affidabile e la messaggistica di pubblicazione e sottoscrizione permanente.</span><span class="sxs-lookup"><span data-stu-id="4a56d-106">Microsoft Azure Service Bus supports a set of cloud-based, message-oriented middleware technologies including reliable message queuing and durable publish/subscribe messaging.</span></span> 

## <a name="install-the-libraries"></a><span data-ttu-id="4a56d-107">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="4a56d-107">Install the libraries</span></span>
```bash
pip install azure-mgmt-servicebus
```

## <a name="example"></a><span data-ttu-id="4a56d-108">Esempio</span><span class="sxs-lookup"><span data-stu-id="4a56d-108">Example</span></span>
<span data-ttu-id="4a56d-109">Inviare messaggi a una coda.</span><span class="sxs-lookup"><span data-stu-id="4a56d-109">Send messages to a queue.</span></span>

```python
from azure.servicebus import Message

msg1 = Message('Hello World!')
msg2 = Message('Hello World again!')
sbs.send_queue_message_batch('taskqueue', [msg1, msg2])
# dequeue the message
msg = sbs.receive_queue_message('taskqueue')
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="4a56d-110">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="4a56d-110">Explore the Management APIs</span></span>](/python/api/overview/azure/servicebus/managementlibrary)


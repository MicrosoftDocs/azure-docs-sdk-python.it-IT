---
title: Librerie di Hub di notifica di Azure per Python
description: Informazioni di riferimento sulle librerie di Hub di notifica di Azure per Python
keywords: Azure, Python, SDK, API, Hub di notifica
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 02/22/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 66b452a40fd524672f4dad92a9d1bd0ffb77a99d
ms.sourcegitcommit: d7c26ac167cf6a6491358ac3153f268bc90e55e9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/24/2018
ms.locfileid: "29551584"
---
# <a name="azure-notification-hubs-libraries-for-python"></a>Librerie di Hub di notifica di Azure per Python

## <a name="management-apipythonapioverviewazurenotificationhubsmanagement"></a>[API di gestione](/python/api/overview/azure/notificationhubs/management)

```bash
pip install azure-mgmt-notificationhubs
```

## <a name="create-the-management-client"></a>Creare il client di gestione

Il codice seguente crea un'istanza del client di gestione.

Sarà necessario specificare il proprio ``subscription_id``, recuperabile dall'[elenco delle sottoscrizioni](https://manage.windowsazure.com/#Workspaces/AdminTasks/SubscriptionMapping).

Vedere [Resource Management Authentication](/python/azure/python-sdk-azure-authenticate) (Autenticazione di gestione risorse) per informazioni dettagliate sulla gestione dell'autenticazione di Azure Active Directory con Python SDK e sulla creazione di un'istanza di ``Credentials``.

```python
from azure.mgmt.notificationhubs import NotificationHubsManagementClient
from azure.common.credentials import UserPassCredentials

# Replace this with your subscription id
subscription_id = '33333333-3333-3333-3333-333333333333'

# See above for details on creating different types of AAD credentials
credentials = UserPassCredentials(
    'user@domain.com',  # Your user
    'my_password',      # Your password
)

redis_client = NotificationHubsManagementClient(
    credentials,
    subscription_id
)
```

## <a name="check-namespace-availability"></a>Verificare la disponibilità dello spazio dei nomi

Il codice seguente verifica la disponibilità dello spazio dei nomi di un hub di notifica.
```python
from azure.mgmt.notificationhubs.models import CheckAvailabilityParameters

account_name = 'mynotificationhub'
output = notificationhubs_client.namespaces.check_availability(
    azure.mgmt.notificationhubs.models.CheckAvailabilityParameters(
        name = account_name
    )
)
# output is a CheckAvailibilityResource instance
print(output.is_availiable) # Yes, it's 'availiable', it's a typo in the REST API
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/notificationhubs/management)

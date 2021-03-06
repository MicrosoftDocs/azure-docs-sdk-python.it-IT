---
title: Librerie delle risorse di Azure per Python
description: ''
keywords: Azure, Python, SDK, API, risorse
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/19/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: resources
ms.openlocfilehash: d708a5e7296b166b6e55b9b7b0d995e72e264267
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534376"
---
# <a name="azure-resources-libraries-for-python"></a>Librerie delle risorse di Azure per Python 

## <a name="overview"></a>Panoramica 
Gestire le risorse di Azure nei gruppi di risorse.

| Pacchetto  |  DESCRIZIONE |
|---|---|
|[azure.mgmt.resource.features][1]|Azure Feature Exposure Control (AFEC) fornisce un meccanismo che consente ai provider di risorse di controllare l'esposizione delle funzionalità agli utenti.|
|[azure.mgmt.resource.links][2]|Le risorse di Azure possono essere collegate tra loro per formare relazioni logiche. È possibile stabilire collegamenti tra le risorse appartenenti a gruppi di risorse diversi.|
|[azure.mgmt.resource.locks][3]|Le risorse di Azure possono essere bloccate per impedire ad altri utenti dell'organizzazione di eliminare o modificare le risorse.|
|[azure.mgmt.resource.managedapplications][4]|Applicazioni gestite da ARM (appliance).|
|[azure.mgmt.resource.policy][5]|Per gestire e controllare l'accesso alle risorse, è possibile definire criteri personalizzati e assegnarli a un ambito.|
|[azure.mgmt.resource.resources][6]| Fornisce operazioni per l'uso di risorse e gruppi di risorse.|
|[azure.mgmt.resource.subscriptions][7]|Tutti i gruppi di risorse e tutte le risorse esistono all'interno delle sottoscrizioni. Queste operazioni consentono di ottenere informazioni sulle sottoscrizioni e sui tenant.|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a>Installare le librerie 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a>Esempio
L'esempio seguente illustra come creare un gruppo di risorse. 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

Esplorare altro [codice Python di esempio](https://azure.microsoft.com/resources/samples/?platform=python) da usare nelle app. 
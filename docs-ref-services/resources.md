---
title: Librerie delle risorse di Azure per Python
description: Informazioni di riferimento sulle librerie delle risorse di Azure per Python
keywords: Azure, Python, SDK, API, risorse
author: lisawong19
ms.author: liwong
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: f331a88ceac73449c9c1aff7bccbbaff4c4ba7be
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277378"
---
# <a name="azure-resources-libraries-for-python"></a>Librerie delle risorse di Azure per Python

## <a name="overview"></a>Panoramica 
[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) consente di distribuire, monitorare e gestire le risorse nei gruppi.

## <a name="management-api"></a>API di gestione
Usare l'API di gestione per creare gruppi di risorse e distribuire le risorse dai modelli.

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a>Esempio 
Creare un nuovo gruppo di risorse nell'area di Azure Stati Uniti orientali.

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a>Esempi
[Manage Azure resources and resource groups (Gestire risorse e gruppi di risorse di Azure)](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) degli esempi di Azure Resource Manager.

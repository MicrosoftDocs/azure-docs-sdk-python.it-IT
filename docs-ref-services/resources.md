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
ms.openlocfilehash: d924a8aea18d9b59ec8c78d85dce8fe0ce7c8d6c
ms.sourcegitcommit: d521a7350216461eb2fa68152c4975f55152f831
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2017
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

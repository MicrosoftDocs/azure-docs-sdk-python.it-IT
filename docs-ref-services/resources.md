---
title: Librerie delle risorse di Azure per Python
description: Informazioni di riferimento sulle librerie delle risorse di Azure per Python
keywords: Azure, Python, SDK, API, risorse
author: lisawong19
ms.author: routlaw
manager: routlaw
ms.date: 08/11/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: cef1f2bad7dcb3ff73aeae9c56000fb949541df9
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534222"
---
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="4536e-104">Librerie delle risorse di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="4536e-104">Azure Resources libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="4536e-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4536e-105">Overview</span></span> 
<span data-ttu-id="4536e-106">[Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) consente di distribuire, monitorare e gestire le risorse nei gruppi.</span><span class="sxs-lookup"><span data-stu-id="4536e-106">Deploy, monitor, and manage resources in groups with [Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

## <a name="management-api"></a><span data-ttu-id="4536e-107">API di gestione</span><span class="sxs-lookup"><span data-stu-id="4536e-107">Management API</span></span>
<span data-ttu-id="4536e-108">Usare l'API di gestione per creare gruppi di risorse e distribuire le risorse dai modelli.</span><span class="sxs-lookup"><span data-stu-id="4536e-108">Use the management API to create resource groups and deploy resources from templates.</span></span>

```bash
pip install azure-mgmt-resource
```
### <a name="example"></a><span data-ttu-id="4536e-109">Esempio</span><span class="sxs-lookup"><span data-stu-id="4536e-109">Example</span></span> 
<span data-ttu-id="4536e-110">Creare un nuovo gruppo di risorse nell'area di Azure Stati Uniti orientali.</span><span class="sxs-lookup"><span data-stu-id="4536e-110">Create a new resource group in the Azure Eastern US region.</span></span>

```python
from azure.mgmt.resource import ResourceManagementClient

LOCATION = 'eastus'
GROUP_NAME ='sample_resource_group'

resource_client = ResourceManagementClient(credentials, subscription_id)
resource_client.resource_groups.create_or_update(GROUP_NAME, {'location': LOCATION})
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="4536e-111">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="4536e-111">Explore the Management APIs</span></span>](/python/api/overview/azure/azure.mgmt.resource)

## <a name="samples"></a><span data-ttu-id="4536e-112">Esempi</span><span class="sxs-lookup"><span data-stu-id="4536e-112">Samples</span></span>
[<span data-ttu-id="4536e-113">Manage Azure resources and resource groups (Gestire risorse e gruppi di risorse di Azure)</span><span class="sxs-lookup"><span data-stu-id="4536e-113">Manage Azure resources and resource groups</span></span>](https://github.com/Azure-Samples/resource-manager-python-resources-and-groups)

<span data-ttu-id="4536e-114">Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) degli esempi di Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4536e-114">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=resource) of Azure Resource Manager samples.</span></span>

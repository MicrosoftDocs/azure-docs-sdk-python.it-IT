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
# <a name="azure-resources-libraries-for-python"></a><span data-ttu-id="be0f3-103">Librerie delle risorse di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="be0f3-103">Azure Resources libraries for Python</span></span> 

## <a name="overview"></a><span data-ttu-id="be0f3-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="be0f3-104">Overview</span></span> 
<span data-ttu-id="be0f3-105">Gestire le risorse di Azure nei gruppi di risorse.</span><span class="sxs-lookup"><span data-stu-id="be0f3-105">Manage Azure resources in resource groups.</span></span>

| <span data-ttu-id="be0f3-106">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="be0f3-106">Package</span></span>  |  <span data-ttu-id="be0f3-107">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="be0f3-107">Description</span></span> |
|---|---|
|<span data-ttu-id="be0f3-108">[azure.mgmt.resource.features][1]</span><span class="sxs-lookup"><span data-stu-id="be0f3-108">[azure.mgmt.resource.features][1]</span></span>|<span data-ttu-id="be0f3-109">Azure Feature Exposure Control (AFEC) fornisce un meccanismo che consente ai provider di risorse di controllare l'esposizione delle funzionalità agli utenti.</span><span class="sxs-lookup"><span data-stu-id="be0f3-109">Azure Feature Exposure Control (AFEC) provides a mechanism for the resource providers to control feature exposure to users.</span></span>|
|<span data-ttu-id="be0f3-110">[azure.mgmt.resource.links][2]</span><span class="sxs-lookup"><span data-stu-id="be0f3-110">[azure.mgmt.resource.links][2]</span></span>|<span data-ttu-id="be0f3-111">Le risorse di Azure possono essere collegate tra loro per formare relazioni logiche.</span><span class="sxs-lookup"><span data-stu-id="be0f3-111">Azure resources can be linked together to form logical relationships.</span></span> <span data-ttu-id="be0f3-112">È possibile stabilire collegamenti tra le risorse appartenenti a gruppi di risorse diversi.</span><span class="sxs-lookup"><span data-stu-id="be0f3-112">You can establish links between resources belonging to different resource groups.</span></span>|
|<span data-ttu-id="be0f3-113">[azure.mgmt.resource.locks][3]</span><span class="sxs-lookup"><span data-stu-id="be0f3-113">[azure.mgmt.resource.locks][3]</span></span>|<span data-ttu-id="be0f3-114">Le risorse di Azure possono essere bloccate per impedire ad altri utenti dell'organizzazione di eliminare o modificare le risorse.</span><span class="sxs-lookup"><span data-stu-id="be0f3-114">Azure resources can be locked to prevent other users in your organization from deleting or modifying resources.</span></span>|
|<span data-ttu-id="be0f3-115">[azure.mgmt.resource.managedapplications][4]</span><span class="sxs-lookup"><span data-stu-id="be0f3-115">[azure.mgmt.resource.managedapplications][4]</span></span>|<span data-ttu-id="be0f3-116">Applicazioni gestite da ARM (appliance).</span><span class="sxs-lookup"><span data-stu-id="be0f3-116">ARM managed applications (appliances).</span></span>|
|<span data-ttu-id="be0f3-117">[azure.mgmt.resource.policy][5]</span><span class="sxs-lookup"><span data-stu-id="be0f3-117">[azure.mgmt.resource.policy][5]</span></span>|<span data-ttu-id="be0f3-118">Per gestire e controllare l'accesso alle risorse, è possibile definire criteri personalizzati e assegnarli a un ambito.</span><span class="sxs-lookup"><span data-stu-id="be0f3-118">To manage and control access to your resources, you can define customized policies and assign them at a scope.</span></span>|
|<span data-ttu-id="be0f3-119">[azure.mgmt.resource.resources][6]</span><span class="sxs-lookup"><span data-stu-id="be0f3-119">[azure.mgmt.resource.resources][6]</span></span>| <span data-ttu-id="be0f3-120">Fornisce operazioni per l'uso di risorse e gruppi di risorse.</span><span class="sxs-lookup"><span data-stu-id="be0f3-120">Provides operations for working with resources and resource groups.</span></span>|
|<span data-ttu-id="be0f3-121">[azure.mgmt.resource.subscriptions][7]</span><span class="sxs-lookup"><span data-stu-id="be0f3-121">[azure.mgmt.resource.subscriptions][7]</span></span>|<span data-ttu-id="be0f3-122">Tutti i gruppi di risorse e tutte le risorse esistono all'interno delle sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="be0f3-122">All resource groups and resources exist within subscriptions.</span></span> <span data-ttu-id="be0f3-123">Queste operazioni consentono di ottenere informazioni sulle sottoscrizioni e sui tenant.</span><span class="sxs-lookup"><span data-stu-id="be0f3-123">These operation enable you get information about your subscriptions and tenants.</span></span>|

[1]: /python/api/azure.mgmt.resource.features
[2]: /python/api/azure.mgmt.resource.links
[3]: /python/api/azure.mgmt.resource.locks
[4]: /python/api/azure.mgmt.resource.managedapplications
[5]: /python/api/azure.mgmt.resource.policy
[6]: /python/api/azure.mgmt.resource.resources
[7]: /python/api/azure.mgmt.resource.subscriptions

## <a name="install-the-libraries"></a><span data-ttu-id="be0f3-124">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="be0f3-124">Install the libraries</span></span> 
```bash
pip install azure-mgmt-resource
```

## <a name="example"></a><span data-ttu-id="be0f3-125">Esempio</span><span class="sxs-lookup"><span data-stu-id="be0f3-125">Example</span></span>
<span data-ttu-id="be0f3-126">L'esempio seguente illustra come creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="be0f3-126">The following is an example of how to create a resource group.</span></span> 

```python
from azure.mgmt.resource import ResourceManagementClient
client = ResourceManagementClient(credentials, subscription_id)
client.resource_groups.create(RESOURCE_GROUP_NAME, {'location':'eastus'})
```

<span data-ttu-id="be0f3-127">Esplorare altro [codice Python di esempio](https://azure.microsoft.com/resources/samples/?platform=python) da usare nelle app.</span><span class="sxs-lookup"><span data-stu-id="be0f3-127">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span> 
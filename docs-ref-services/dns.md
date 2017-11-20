---
title: Librerie di DNS di Azure per Python
description: Informazioni di riferimento sulle librerie di DNS di Azure per Python
keywords: Azure, Python, SDK, API, DNS
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 59ae3628b4a810db8fee21aacf46c13054dc8cd3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-dns-libraries-for-python"></a><span data-ttu-id="e8184-104">Librerie di DNS di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="e8184-104">Azure DNS libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="e8184-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e8184-105">Overview</span></span>

<span data-ttu-id="e8184-106">[DNS di Azure](/azure/dns/dns-overview) è un servizio di hosting per domini DNS che fornisce la risoluzione DNS tramite l'infrastruttura di Azure.</span><span class="sxs-lookup"><span data-stu-id="e8184-106">[Azure DNS](/azure/dns/dns-overview) is a hosting service for DNS domains that provides DNS resolution via the Azure infrastructure.</span></span>

<span data-ttu-id="e8184-107">Per iniziare a usare DNS di Azure, vedere [Introduzione a DNS di Azure con il portale di Azure](/azure/dns/dns-getstarted-portal).</span><span class="sxs-lookup"><span data-stu-id="e8184-107">To get started with Azure DNS, see [Get started with Azure DNS using the Azure portal](/azure/dns/dns-getstarted-portal).</span></span>

## <a name="management-apis"></a><span data-ttu-id="e8184-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="e8184-108">Management APIs</span></span>

<span data-ttu-id="e8184-109">Creare e gestire zone e record DNS con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="e8184-109">Create and manage DNS zones and records with the management API.</span></span>

<span data-ttu-id="e8184-110">Installare il pacchetto di gestione con pip.</span><span class="sxs-lookup"><span data-stu-id="e8184-110">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a><span data-ttu-id="e8184-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="e8184-111">Example</span></span>

<span data-ttu-id="e8184-112">Creare una nuova zona DNS.</span><span class="sxs-lookup"><span data-stu-id="e8184-112">Create a new DNS zone.</span></span>

```python
from azure.mgmt.dns import DnsManagementClient

dns_client = DnsManagementClient(credentials, 'your-subscription-id')
zone = dns_client.zones.create_or_update('resource-group',
                                         'zone_name_no_dot',
                                         {
                                            "location": "global"
                                         })

```

> [!div class="nextstepaction"]
> [<span data-ttu-id="e8184-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="e8184-113">Explore the Management APIs</span></span>](/python/api/overview/azure/dns/managementlibrary)

---
title: Librerie della rete CDN di Azure per Python
description: Informazioni di riferimento sulle librerie della rete CDN di Azure per Python
keywords: Azure, Python, SDK, API, rete CDN
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 2dd7703e94a814d85716a7b96994666e32f95565
ms.sourcegitcommit: 3db75daa592da90ea9aa8fd17fb99627a30eb4fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2019
ms.locfileid: "66179877"
---
# <a name="azure-cdn-libraries-for-python"></a><span data-ttu-id="01a97-104">Librerie della rete CDN di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="01a97-104">Azure CDN libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="01a97-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="01a97-105">Overview</span></span>

<span data-ttu-id="01a97-106">La [rete CDN (Content Delivery Network) di Azure](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) consente di fornire cache di contenuto Web per assicurare la disponibilità a larghezza di banda elevata in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="01a97-106">[Azure Content Delivery Network (CDN)](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) allows you to provide web content caches to ensure high-bandwidth availability across the globe.</span></span>

<span data-ttu-id="01a97-107">Per iniziare a usare la rete CDN di Azure, vedere [Introduzione alla rete CDN di Azure](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).</span><span class="sxs-lookup"><span data-stu-id="01a97-107">To get started with Azure CDN, see [Getting started with Azure CDN](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).</span></span>

## <a name="management-apis"></a><span data-ttu-id="01a97-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="01a97-108">Management APIs</span></span>

<span data-ttu-id="01a97-109">Creare, eseguire query e gestire reti CDN di Azure con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="01a97-109">Create, query, and manage Azure CDNs with the management API.</span></span>

<span data-ttu-id="01a97-110">Installare il pacchetto di gestione tramite pip.</span><span class="sxs-lookup"><span data-stu-id="01a97-110">Install the management package via pip.</span></span>

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a><span data-ttu-id="01a97-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="01a97-111">Example</span></span>

<span data-ttu-id="01a97-112">Creazione di un profilo di rete CDN con un singolo endpoint definito:</span><span class="sxs-lookup"><span data-stu-id="01a97-112">Creating a CDN profile with a single defined endpoint:</span></span>

```python
from azure.mgmt.cdn import CdnManagementClient

cdn_client = CdnManagementClient(credentials, 'your-subscription-id')
profile_poller = cdn_client.profiles.create('my-resource-group',
                                            'cdn-name',
                                            {
                                                "location": "some_region", 
                                                "sku": {
                                                    "name": "sku_tier"
                                                } 
                                            })
profile = profile_poller.result()

endpoint_poller = cdn_client.endpoints.create('my-resource-group',
                                          'cdn-name',
                                          'unique-endpoint-name', 
                                          { 
                                              "location": "any_region", 
                                              "origins": [
                                                  {
                                                      "name": "origin_name", 
                                                      "host_name": "url"
                                                  }]
                                          })
endpoint = endpoint_poller.result()
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="01a97-113">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="01a97-113">Explore the Management APIs</span></span>](/python/api/overview/azure/cdn/management)

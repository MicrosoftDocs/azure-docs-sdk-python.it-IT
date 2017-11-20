---
title: Librerie della cache Redis di Azure per Python
description: Documentazione di riferimento per le librerie client Python per la cache Redis
keywords: Azure, Python, Redis, API, SDK, database, NoSQL
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 06/26/2017
ms.topic: article
ms.devlang: python
ms.service: redis
ms.openlocfilehash: 3ba8d972e91fad229c1489800edeca08760254e6
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-redis-cache-libraries-for-python"></a><span data-ttu-id="d7195-104">Librerie della cache Redis di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="d7195-104">Azure Redis Cache libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="d7195-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d7195-105">Overview</span></span>

<span data-ttu-id="d7195-106">La cache Redis di Azure si basa sul noto progetto Redis open source.</span><span class="sxs-lookup"><span data-stu-id="d7195-106">Azure Redis Cache is based on the popular open source Redis project.</span></span> <span data-ttu-id="d7195-107">Consente di accedere a un'istanza sicura e dedicata della cache Redis, gestita da Microsoft e accessibile dalle app di Azure.</span><span class="sxs-lookup"><span data-stu-id="d7195-107">It gives you access to a secure, dedicated Redis instance, managed by Microsoft and accessible from your Azure apps.</span></span>

<span data-ttu-id="d7195-108">Redis è un archivio chiave-valore avanzato, in cui le chiavi possono contenere strutture di dati come stringhe, hash, elenchi, set e set ordinati.</span><span class="sxs-lookup"><span data-stu-id="d7195-108">Redis is an advanced key-value store, where keys can contain data structures such as strings, hashes, lists, sets, and sorted sets.</span></span> <span data-ttu-id="d7195-109">Redis supporta una serie di operazioni atomiche su questi tipi di dati.</span><span class="sxs-lookup"><span data-stu-id="d7195-109">Redis supports a set of atomic operations on these data types.</span></span>

<span data-ttu-id="d7195-110">Altre informazioni sulla [Cache Redis di Azure](https://docs.microsoft.com/azure/redis-cache/).</span><span class="sxs-lookup"><span data-stu-id="d7195-110">Learn more about [Azure Redis Cache](https://docs.microsoft.com/azure/redis-cache/).</span></span>

## <a name="management-api"></a><span data-ttu-id="d7195-111">API di gestione</span><span class="sxs-lookup"><span data-stu-id="d7195-111">Management API</span></span>

<span data-ttu-id="d7195-112">Creare e gestire le risorse Redis nella sottoscrizione con l'API di gestione Redis.</span><span class="sxs-lookup"><span data-stu-id="d7195-112">Create and manage your Redis resources in your subscription with the Redis management API.</span></span>

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a><span data-ttu-id="d7195-113">Esempio</span><span class="sxs-lookup"><span data-stu-id="d7195-113">Example</span></span>

<span data-ttu-id="d7195-114">L'esempio seguente crea una nuova cache Redis:</span><span class="sxs-lookup"><span data-stu-id="d7195-114">The following example creates a new Redis cache:</span></span>

```python
from azure.mgmt.redis import RedisManagementClient
from azure.mgmt.redis.models import Sku, RedisCreateOrUpdateParameters

redis_client = RedisManagementClient(
    credentials,
    subscription_id
)
group_name = 'myresourcegroup'
cache_name = 'mycachename'
redis_cache = redis_client.redis.create_or_update(
    group_name,
    cache_name,
    RedisCreateOrUpdateParameters(
        sku = Sku(name = 'Basic', family = 'C', capacity = '1'),
        location = "East US"
    )
)
# redis_cache is a RedisResourceWithAccessKey instance
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d7195-115">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="d7195-115">Explore the Management APIs</span></span>](/python/api/overview/azure/redis/managementlibrary)


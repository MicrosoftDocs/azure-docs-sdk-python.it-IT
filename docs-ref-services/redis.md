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
ms.openlocfilehash: c2f8ebcbcbd7b71c1fa96e46a8148a3c0b88877f
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479244"
---
# <a name="azure-redis-cache-libraries-for-python"></a>Librerie della cache Redis di Azure per Python

## <a name="overview"></a>Panoramica

La cache Redis di Azure si basa sul noto progetto Redis open source. Consente di accedere a un'istanza sicura e dedicata della cache Redis, gestita da Microsoft e accessibile dalle app di Azure.

Redis è un archivio chiave-valore avanzato, in cui le chiavi possono contenere strutture di dati come stringhe, hash, elenchi, set e set ordinati. Redis supporta una serie di operazioni atomiche su questi tipi di dati.

Altre informazioni sulla [Cache Redis di Azure](https://docs.microsoft.com/azure/redis-cache/).

## <a name="management-api"></a>API di gestione

Creare e gestire le risorse Redis nella sottoscrizione con l'API di gestione Redis.

```bash
pip install redis
pip install azure-mgmt-redis
```

### <a name="example"></a>Esempio

L'esempio seguente crea una nuova cache Redis:

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
> [Esplorare le API di gestione](/python/api/overview/azure/redis/management)


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
ms.openlocfilehash: 06e6c8786ebbd88b7d3996b640af96a23cd5689b
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275535"
---
# <a name="azure-cdn-libraries-for-python"></a>Librerie della rete CDN di Azure per Python

## <a name="overview"></a>Panoramica

La [rete CDN (Content Delivery Network) di Azure](https://docs.microsoft.com/en-us/azure/cdn/cdn-overview) consente di fornire cache di contenuto Web per assicurare la disponibilità a larghezza di banda elevata in tutto il mondo.

Per iniziare a usare la rete CDN di Azure, vedere [Introduzione alla rete CDN di Azure](https://docs.microsoft.com/en-us/azure/cdn/cdn-create-new-endpoint).

## <a name="management-apis"></a>API di gestione

Creare, eseguire query e gestire reti CDN di Azure con l'API di gestione.

Installare il pacchetto di gestione tramite pip.

```bash
pip install azure-mgmt-cdn
```

### <a name="example"></a>Esempio

Creazione di un profilo di rete CDN con un singolo endpoint definito:

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

endpoint_poller = client.endpoints.create('my-resource-group',
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
> [Esplorare le API di gestione](/python/api/overview/azure/cdn/management)

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
# <a name="azure-dns-libraries-for-python"></a>Librerie di DNS di Azure per Python

## <a name="overview"></a>Panoramica

[DNS di Azure](/azure/dns/dns-overview) è un servizio di hosting per domini DNS che fornisce la risoluzione DNS tramite l'infrastruttura di Azure.

Per iniziare a usare DNS di Azure, vedere [Introduzione a DNS di Azure con il portale di Azure](/azure/dns/dns-getstarted-portal).

## <a name="management-apis"></a>API di gestione

Creare e gestire zone e record DNS con l'API di gestione.

Installare il pacchetto di gestione con pip.

```bash
pip install azure-mgmt-dns
```

### <a name="example"></a>Esempio

Creare una nuova zona DNS.

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
> [Esplorare le API di gestione](/python/api/overview/azure/dns/managementlibrary)

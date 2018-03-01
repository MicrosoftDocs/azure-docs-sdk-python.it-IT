---
title: Librerie della rete virtuale di Azure per Python
description: Informazioni di riferimento sulle librerie della rete di Azure per Python
keywords: Azure, Python, SDK, API, rete
author: sptramer
ms.author: sttramer
manager: douge
ms.date: 07/10/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 47252ca3b2f5c6087277bac3735025f0dbabbdd8
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
---
# <a name="azure-network-libraries-for-python"></a>Librerie della rete virtuale di Azure per Python

## <a name="overview"></a>Panoramica

La [Rete virtuale di Azure](/azure/virtual-network/virtual-networks-overview) consente di connettere le risorse di Azure e di connetterle anche alla rete locale.

Per iniziare a usare la rete virtuale di Azure, vedere [Creare la prima rete virtuale](/azure/virtual-network/virtual-network-get-started-vnet-subnet).

## <a name="management-apis"></a>API di gestione

Esaminare, gestire e configurare le reti virtuali di Azure con le API di gestione.

A differenza delle altre API Python di Azure, la versione delle API relative alla rete è indicata esplicitamente e le API sono suddivise in pacchetti separati. Non è necessario importarle singolarmente, perché le informazioni sul pacchetto sono specificate nel costruttore del client.

Installare il pacchetto di gestione con pip.

```bash
pip install azure-mgmt-network
```

### <a name="example"></a>Esempio

Creare una rete virtuale e una subnet associata.

```python
from azure.mgmt.network import NetworkManagementClient

GROUP_NAME = 'resource-group'
VNET_NAME = 'your-vnet-identifier'
LOCATION = 'region'
SUBNET_NAME = 'your-subnet-identifier'

network_client = NetworkManagementClient(credentials, 'your-subscription-id')

async_vnet_creation = network_client.virtual_networks.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    {
        'location': LOCATION,
        'address_space': {
            'address_prefixes': ['10.0.0.0/16']
        }
    }
)
async_vnet_creation.wait()

# Create Subnet
async_subnet_creation = network_client.subnets.create_or_update(
    GROUP_NAME,
    VNET_NAME,
    SUBNET_NAME,
    {'address_prefix': '10.0.0.0/24'}
)
subnet_info = async_subnet_creation.result()
```

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/network/management)

### <a name="samples"></a>Esempi

* [Introduzione ad Azure Resource Manager per i servizi di bilanciamento del carico in Python](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)

Visualizzare l'[elenco completo](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) degli esempi di codice per la Rete virtuale di Azure.

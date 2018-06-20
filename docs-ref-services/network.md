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
ms.locfileid: "29479074"
---
# <a name="azure-network-libraries-for-python"></a><span data-ttu-id="62baa-104">Librerie della rete virtuale di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="62baa-104">Azure Network libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="62baa-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="62baa-105">Overview</span></span>

<span data-ttu-id="62baa-106">La [Rete virtuale di Azure](/azure/virtual-network/virtual-networks-overview) consente di connettere le risorse di Azure e di connetterle anche alla rete locale.</span><span class="sxs-lookup"><span data-stu-id="62baa-106">[Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) allows you to connect Azure resources, and also connect them to your on-premises network.</span></span>

<span data-ttu-id="62baa-107">Per iniziare a usare la rete virtuale di Azure, vedere [Creare la prima rete virtuale](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span><span class="sxs-lookup"><span data-stu-id="62baa-107">To get started with Azure Virtual Network, see [Create your first virtual network](/azure/virtual-network/virtual-network-get-started-vnet-subnet).</span></span>

## <a name="management-apis"></a><span data-ttu-id="62baa-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="62baa-108">Management APIs</span></span>

<span data-ttu-id="62baa-109">Esaminare, gestire e configurare le reti virtuali di Azure con le API di gestione.</span><span class="sxs-lookup"><span data-stu-id="62baa-109">Inspect, manage, and configure Azure virtual networks with the management APIs.</span></span>

<span data-ttu-id="62baa-110">A differenza delle altre API Python di Azure, la versione delle API relative alla rete è indicata esplicitamente e le API sono suddivise in pacchetti separati.</span><span class="sxs-lookup"><span data-stu-id="62baa-110">Unlike other Azure python APIs, the networking APIs are explicitly versioned into separage packages.</span></span> <span data-ttu-id="62baa-111">Non è necessario importarle singolarmente, perché le informazioni sul pacchetto sono specificate nel costruttore del client.</span><span class="sxs-lookup"><span data-stu-id="62baa-111">You do not need to import them individually since the package information is specified in the client constructor.</span></span>

<span data-ttu-id="62baa-112">Installare il pacchetto di gestione con pip.</span><span class="sxs-lookup"><span data-stu-id="62baa-112">Install the management package with pip.</span></span>

```bash
pip install azure-mgmt-network
```

### <a name="example"></a><span data-ttu-id="62baa-113">Esempio</span><span class="sxs-lookup"><span data-stu-id="62baa-113">Example</span></span>

<span data-ttu-id="62baa-114">Creare una rete virtuale e una subnet associata.</span><span class="sxs-lookup"><span data-stu-id="62baa-114">Create a virtual network and an associated subnet.</span></span>

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
> [<span data-ttu-id="62baa-115">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="62baa-115">Explore the Management APIs</span></span>](/python/api/overview/azure/network/management)

### <a name="samples"></a><span data-ttu-id="62baa-116">Esempi</span><span class="sxs-lookup"><span data-stu-id="62baa-116">Samples</span></span>

* [<span data-ttu-id="62baa-117">Introduzione ad Azure Resource Manager per i servizi di bilanciamento del carico in Python</span><span class="sxs-lookup"><span data-stu-id="62baa-117">Getting started with Azure Resource Manager for load balancers in Python</span></span>](https://azure.microsoft.com/en-us/resources/samples/network-python-manage-loadbalancer/)

<span data-ttu-id="62baa-118">Visualizzare l'[elenco completo](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) degli esempi di codice per la Rete virtuale di Azure.</span><span class="sxs-lookup"><span data-stu-id="62baa-118">View the [complete list](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=virtual%20network) of Azure Virtual Network samples.</span></span>

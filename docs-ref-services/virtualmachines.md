---
title: Librerie delle macchine virtuali di Azure per Python
description: ''
keywords: Azure, Python, SDK, API, calcolo, macchine virtuali
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/09/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: compute
ms.openlocfilehash: 78750d5f98ab81401c48493aff98d4268c01850d
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376709"
---
# <a name="azure-virtual-machine-libraries"></a><span data-ttu-id="d6696-103">Librerie delle macchine virtuali di Azure</span><span class="sxs-lookup"><span data-stu-id="d6696-103">Azure virtual machine libraries</span></span>

## <a name="overview"></a><span data-ttu-id="d6696-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d6696-104">Overview</span></span>

<span data-ttu-id="d6696-105">Risorse di calcolo su richiesta e scalabili in esecuzione su Linux o Windows.</span><span class="sxs-lookup"><span data-stu-id="d6696-105">On-demand, scalable computing resources running Linux or Windows.</span></span>

<span data-ttu-id="d6696-106">Per iniziare a usare le macchine virtuali di Azure, vedere [Creare una macchina virtuale Linux con il portale di Azure](/azure/virtual-machines/linux/quick-create-portal).</span><span class="sxs-lookup"><span data-stu-id="d6696-106">To get started with Azure Virtual Machines, see [Create a Linux virtual machine with the Azure portal](/azure/virtual-machines/linux/quick-create-portal).</span></span>

## <a name="management-api"></a><span data-ttu-id="d6696-107">API di gestione</span><span class="sxs-lookup"><span data-stu-id="d6696-107">Management API</span></span>

<span data-ttu-id="d6696-108">Creare, configurare, gestire e ridimensionare le macchine virtuali Windows e Linux in Azure dal codice con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="d6696-108">Create, configure, manage and scale Windows and Linux virtual machines in Azure from your code with the management API.</span></span>

<span data-ttu-id="d6696-109">Installare la libreria tramite pip.</span><span class="sxs-lookup"><span data-stu-id="d6696-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-compute
```

### <a name="example"></a><span data-ttu-id="d6696-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="d6696-110">Example</span></span>

<span data-ttu-id="d6696-111">Creare una nuova macchina virtuale Linux in un gruppo di risorse di Azure esistente con l'autenticazione dell'identità del servizio gestito.</span><span class="sxs-lookup"><span data-stu-id="d6696-111">Create a new Linux virtual machine in an existing Azure resource group with Managed Service Identity(MSI) authentication.</span></span>

```python
VM_PARAMETERS={
        'location': 'LOCATION',
        'os_profile': {
            'computer_name': 'VM_NAME',
            'admin_username': 'USERNAME',
            'admin_password': 'PASSWORD'
        },
        'hardware_profile': {
            'vm_size': 'Standard_DS1_v2'
        },
        'storage_profile': {
            'image_reference': {
                'publisher': 'Canonical',
                'offer': 'UbuntuServer',
                'sku': '16.04.0-LTS',
                'version': 'latest'
            },
        },
        'network_profile': {
            'network_interfaces': [{
                'id': 'NIC_ID',
            }]
        },
    }

def create_vm()
    compute_client.virtual_machines.create_or_update(
        'RESOURCE_GROUP_NAME', 'VM_NAME', VM_PARAMETERS)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="d6696-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="d6696-112">Explore the Management APIs</span></span>](/python/api/overview/azure/virtualmachines/management)

## <a name="samples"></a><span data-ttu-id="d6696-113">Esempi</span><span class="sxs-lookup"><span data-stu-id="d6696-113">Samples</span></span>

* <span data-ttu-id="d6696-114">[Gestire le macchine virtuali][1]</span><span class="sxs-lookup"><span data-stu-id="d6696-114">[Manage virtual machines][1]</span></span>
* <span data-ttu-id="d6696-115">[Eseguire l'autenticazione con l'identità del servizio gestito][2]</span><span class="sxs-lookup"><span data-stu-id="d6696-115">[Authenticate with Managed Service Identity][2]</span></span>
* <span data-ttu-id="d6696-116">[Create a virtual machine with Managed Service Identity Extension][3] (Creare una macchina virtuale con l'estensione dell'identità del servizio gestito)</span><span class="sxs-lookup"><span data-stu-id="d6696-116">[Create a virtual machine with Managed Service Identity Extension][3]</span></span>
* <span data-ttu-id="d6696-117">[Gestire un servizio di bilanciamento del carico][4]</span><span class="sxs-lookup"><span data-stu-id="d6696-117">[Manage a load balancer][4]</span></span>
* <span data-ttu-id="d6696-118">[Creare e configurare dischi gestiti][5]</span><span class="sxs-lookup"><span data-stu-id="d6696-118">[Create and configure managed disks][5]</span></span>
* <span data-ttu-id="d6696-119">[Elencare le immagini][6]</span><span class="sxs-lookup"><span data-stu-id="d6696-119">[List images][6]</span></span> 
* <span data-ttu-id="d6696-120">[Monitorare le macchine virtuali][7]</span><span class="sxs-lookup"><span data-stu-id="d6696-120">[Monitor virtual machines][7]</span></span>

<span data-ttu-id="d6696-121">Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) di esempi di macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="d6696-121">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) of virtual machine samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://github.com/Azure-Samples/resource-manager-python-manage-resources-with-msi
[3]: https://github.com/Azure-Samples/compute-python-msi-vm
[4]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[6]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[7]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md

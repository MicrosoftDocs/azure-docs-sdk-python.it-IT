---
title: Managed Disks
description: Creare, ridimensionare e aggiornare un disco gestito.
author: lisawong19
manager: douge
ms.assetid: 
ms.devlang: python
ms.topic: article
ms.service: Azure
ms.technology: Azure
ms.date: 6/15/2017
ms.author: liwong
ms.openlocfilehash: 776e13ed91482c34e5d637d5eedf2640cd4ca9f4
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="managed-disks"></a><span data-ttu-id="f55dd-103">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="f55dd-103">Managed Disks</span></span>

<span data-ttu-id="f55dd-104">Azure Managed Disks e 1000 VM in un set di scalabilità sono ora [disponibili a livello generale](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-managed-disks-and-larger-scale-sets/) Azure Managed Disks semplifica la gestione dei dischi, offre una scalabilità avanzata e migliore sicurezza e scalabilità.</span><span class="sxs-lookup"><span data-stu-id="f55dd-104">Azure Managed Disks and 1000 VMs in a Scale Set are now [generally available](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-managed-disks-and-larger-scale-sets/) Azure Managed Disks provide a simplified disk Management, enhanced Scalability, better Security and Scale.</span></span> <span data-ttu-id="f55dd-105">Non sono più necessari account di archiviazione per i dischi. I clienti possono quindi ridimensionare senza doversi preoccupare delle limitazioni associate agli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="f55dd-105">It takes away the notion of storage account for disks, enabling customers to scale without worrying about the limitations associated with storage accounts.</span></span> <span data-ttu-id="f55dd-106">Questo post offre una rapida introduzione e informazioni di riferimento sull'utilizzo del servizio da Python.</span><span class="sxs-lookup"><span data-stu-id="f55dd-106">This post provides a quick introduction and reference on consuming the service from Python.</span></span>



<span data-ttu-id="f55dd-107">Dal punto di vista degli sviluppatori, l'esperienza di Managed Disks nell'interfaccia della riga di comando di Azure è analoga all'esperienza dell'interfaccia della riga di comando in altri strumenti multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="f55dd-107">From a developer perspective, the Managed Disks experience in Azure CLI is idomatic to the CLI experience in other cross-platform tools.</span></span> <span data-ttu-id="f55dd-108">È possibile usare [Azure Python](https://azure.microsoft.com/develop/python/) SDK e il [pacchetto azure-mgmt-compute 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) per amministrare Managed Disks.</span><span class="sxs-lookup"><span data-stu-id="f55dd-108">You can use the [Azure Python](https://azure.microsoft.com/develop/python/) SDK and the [azure-mgmt-compute package 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) to administer Managed Disks.</span></span> <span data-ttu-id="f55dd-109">È possibile creare un client di calcolo usando questa [esercitazione](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementcomputenetwork.html).</span><span class="sxs-lookup"><span data-stu-id="f55dd-109">You can create a compute client using this [tutorial](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementcomputenetwork.html).</span></span>


## <a name="standalone-managed-disks"></a><span data-ttu-id="f55dd-110">Dischi gestiti autonomi</span><span class="sxs-lookup"><span data-stu-id="f55dd-110">Standalone Managed Disks</span></span>

<span data-ttu-id="f55dd-111">È possibile creare con facilità dischi gestiti autonomi in molti modi.</span><span class="sxs-lookup"><span data-stu-id="f55dd-111">You can easily create standalone Managed Disks in a variety of ways.</span></span>

### <a name="create-an-empty-managed-disk"></a><span data-ttu-id="f55dd-112">Creare un disco gestito vuoto.</span><span class="sxs-lookup"><span data-stu-id="f55dd-112">Create an empty Managed Disk.</span></span>
```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a><span data-ttu-id="f55dd-113">Creare un disco gestito dall'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="f55dd-113">Create a Managed Disk from Blob Storage.</span></span>
```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a><span data-ttu-id="f55dd-114">Creare un disco gestito da un'immagine personalizzata</span><span class="sxs-lookup"><span data-stu-id="f55dd-114">Create a Managed Disk from your own Image</span></span>
```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')
async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = async_creation.result()
```

## <a name="virtual-machine-with-managed-disks"></a><span data-ttu-id="f55dd-115">Macchina virtuale con dischi gestiti</span><span class="sxs-lookup"><span data-stu-id="f55dd-115">Virtual Machine with Managed Disks</span></span>

<span data-ttu-id="f55dd-116">È possibile creare una macchina virtuale con un disco gestito implicito per un'immagine del disco specifica.</span><span class="sxs-lookup"><span data-stu-id="f55dd-116">You can create a Virtual Machine with an implicit Managed Disk for a specific disk image.</span></span> <span data-ttu-id="f55dd-117">La creazione viene semplificata tramite la creazione implicita dei dischi gestiti, senza la necessità di specificare tutti i dettagli del disco.</span><span class="sxs-lookup"><span data-stu-id="f55dd-117">Creation is simplified with implicit creation of managed disks without specifying all the disk details.</span></span> <span data-ttu-id="f55dd-118">Non è necessario preoccuparsi della creazione e della gestione degli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="f55dd-118">You do not have to worry about creating and managing Storage Accounts.</span></span>

<span data-ttu-id="f55dd-119">Un disco gestito viene creato implicitamente durante la creazione di una VM da un'immagine del sistema operativo in Azure.</span><span class="sxs-lookup"><span data-stu-id="f55dd-119">A Managed Disk is created implicitly when creating VM from an OS image in Azure.</span></span> <span data-ttu-id="f55dd-120">Nel parametro ``storage_profile`` il valore ``os_disk`` è ora facoltativo e non è necessario creare un account di archiviazione come precondizione obbligatoria per la creazione di una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f55dd-120">In the ``storage_profile`` parameter, ``os_disk`` is now optional and you don't have to create a storage account as required precondition to create a Virtual Machine.</span></span>

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
``` 
<span data-ttu-id="f55dd-121">Questo parametro ``storage_profile`` è ora valido.</span><span class="sxs-lookup"><span data-stu-id="f55dd-121">This ``storage_profile`` parameter is now valid.</span></span> <span data-ttu-id="f55dd-122">Per ottenere un esempio completo della procedura di creazione di una VM in Python (incluse le risorse di rete e così via), vedere l'[esercitazione completa per le VM in Python](https://github.com/Azure-Samples/virtual-machines-python-manage).</span><span class="sxs-lookup"><span data-stu-id="f55dd-122">To get a complete example on how to create a VM in Python (including network, etc), check the full [VM tutorial in Python](https://github.com/Azure-Samples/virtual-machines-python-manage).</span></span>

<span data-ttu-id="f55dd-123">È possibile collegare con facilità un disco gestito sottoposto in precedenza a provisioning.</span><span class="sxs-lookup"><span data-stu-id="f55dd-123">You can easily attach a previously provisioned Managed Disk.</span></span>
```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOption.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})
async_update = compute_client.virtual_machines.create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a><span data-ttu-id="f55dd-124">Set di scalabilità di macchine virtuali con Managed Disks</span><span class="sxs-lookup"><span data-stu-id="f55dd-124">Virtual Machine Scale Sets with Managed Disks</span></span>

<span data-ttu-id="f55dd-125">Senza Managed Disks, è necessario creare manualmente un account di archiviazione per tutte le VM da includere nel set di scalabilità e quindi usare il parametro di elenco ``vhd_containers`` per specificare i nomi di tutti gli account di archiviazione per l'API REST del set di scalabilità.</span><span class="sxs-lookup"><span data-stu-id="f55dd-125">Before Managed Disks, you needed to create a storage account manually for all the VMs you wanted inside your Scale Set, and then use the list parameter ``vhd_containers`` to provide all the storage account name to the Scale Set RestAPI.</span></span> <span data-ttu-id="f55dd-126">La guida ufficiale per la transizione è disponibile qui `article <https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`.</span><span class="sxs-lookup"><span data-stu-id="f55dd-126">The official transition guide is available in this `article <https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`__.</span></span>

<span data-ttu-id="f55dd-127">Con Managed Disk non è necessario gestire alcun account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="f55dd-127">Now with Managed Disk, you don't have to manage any storage account at all.</span></span> <span data-ttu-id="f55dd-128">Se si ha familiarità con VMSS Python SDK, il valore di ``storage_profile`` può ora essere identico a quello usato durante la creazione delle VM:</span><span class="sxs-lookup"><span data-stu-id="f55dd-128">If you're are used to the VMSS Python SDK, your ``storage_profile`` can now be exactly the same as the one used in VM creation:</span></span>

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

<span data-ttu-id="f55dd-129">Ecco l'esempio completo:</span><span class="sxs-lookup"><span data-stu-id="f55dd-129">The full sample being:</span></span>

```python
naming_infix = "PyTestInfix"
vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    } 
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
``` 

## <a name="other-operations-with-managed-disks"></a><span data-ttu-id="f55dd-130">Altre operazioni con Managed Disks</span><span class="sxs-lookup"><span data-stu-id="f55dd-130">Other Operations with Managed Disks</span></span>

### <a name="resizing-a-managed-disk"></a><span data-ttu-id="f55dd-131">Ridimensionamento di un disco gestito.</span><span class="sxs-lookup"><span data-stu-id="f55dd-131">Resizing a managed disk.</span></span>

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a><span data-ttu-id="f55dd-132">Aggiornare il tipo di account di archiviazione dei dischi gestiti.</span><span class="sxs-lookup"><span data-stu-id="f55dd-132">Update the Storage Account type of the Managed Disks.</span></span>
```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-blob-storage"></a><span data-ttu-id="f55dd-133">Creare un'immagine dall'archiviazione BLOB.</span><span class="sxs-lookup"><span data-stu-id="f55dd-133">Create an image from Blob Storage.</span></span>
```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a><span data-ttu-id="f55dd-134">Creare uno snapshot di un disco gestito attualmente collegato a una macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="f55dd-134">Create a snapshot of a Managed Disk that is currently attached to a Virtual Machine.</span></span>
```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
async_snapshot_creation = self.compute_client.snapshots.create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```

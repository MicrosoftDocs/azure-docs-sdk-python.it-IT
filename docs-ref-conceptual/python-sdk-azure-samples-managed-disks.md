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
# <a name="managed-disks"></a>Managed Disks

Azure Managed Disks e 1000 VM in un set di scalabilità sono ora [disponibili a livello generale](https://azure.microsoft.com/en-us/blog/announcing-general-availability-of-managed-disks-and-larger-scale-sets/) Azure Managed Disks semplifica la gestione dei dischi, offre una scalabilità avanzata e migliore sicurezza e scalabilità. Non sono più necessari account di archiviazione per i dischi. I clienti possono quindi ridimensionare senza doversi preoccupare delle limitazioni associate agli account di archiviazione. Questo post offre una rapida introduzione e informazioni di riferimento sull'utilizzo del servizio da Python.



Dal punto di vista degli sviluppatori, l'esperienza di Managed Disks nell'interfaccia della riga di comando di Azure è analoga all'esperienza dell'interfaccia della riga di comando in altri strumenti multipiattaforma. È possibile usare [Azure Python](https://azure.microsoft.com/develop/python/) SDK e il [pacchetto azure-mgmt-compute 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) per amministrare Managed Disks. È possibile creare un client di calcolo usando questa [esercitazione](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagementcomputenetwork.html).


## <a name="standalone-managed-disks"></a>Dischi gestiti autonomi

È possibile creare con facilità dischi gestiti autonomi in molti modi.

### <a name="create-an-empty-managed-disk"></a>Creare un disco gestito vuoto.
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

### <a name="create-a-managed-disk-from-blob-storage"></a>Creare un disco gestito dall'archiviazione BLOB.
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

### <a name="create-a-managed-disk-from-your-own-image"></a>Creare un disco gestito da un'immagine personalizzata
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

## <a name="virtual-machine-with-managed-disks"></a>Macchina virtuale con dischi gestiti

È possibile creare una macchina virtuale con un disco gestito implicito per un'immagine del disco specifica. La creazione viene semplificata tramite la creazione implicita dei dischi gestiti, senza la necessità di specificare tutti i dettagli del disco. Non è necessario preoccuparsi della creazione e della gestione degli account di archiviazione.

Un disco gestito viene creato implicitamente durante la creazione di una VM da un'immagine del sistema operativo in Azure. Nel parametro ``storage_profile`` il valore ``os_disk`` è ora facoltativo e non è necessario creare un account di archiviazione come precondizione obbligatoria per la creazione di una macchina virtuale.

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
Questo parametro ``storage_profile`` è ora valido. Per ottenere un esempio completo della procedura di creazione di una VM in Python (incluse le risorse di rete e così via), vedere l'[esercitazione completa per le VM in Python](https://github.com/Azure-Samples/virtual-machines-python-manage).

È possibile collegare con facilità un disco gestito sottoposto in precedenza a provisioning.
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

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>Set di scalabilità di macchine virtuali con Managed Disks

Senza Managed Disks, è necessario creare manualmente un account di archiviazione per tutte le VM da includere nel set di scalabilità e quindi usare il parametro di elenco ``vhd_containers`` per specificare i nomi di tutti gli account di archiviazione per l'API REST del set di scalabilità. La guida ufficiale per la transizione è disponibile qui `article <https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`.

Con Managed Disk non è necessario gestire alcun account di archiviazione. Se si ha familiarità con VMSS Python SDK, il valore di ``storage_profile`` può ora essere identico a quello usato durante la creazione delle VM:

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

Ecco l'esempio completo:

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

## <a name="other-operations-with-managed-disks"></a>Altre operazioni con Managed Disks

### <a name="resizing-a-managed-disk"></a>Ridimensionamento di un disco gestito.

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

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>Aggiornare il tipo di account di archiviazione dei dischi gestiti.
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

### <a name="create-an-image-from-blob-storage"></a>Creare un'immagine dall'archiviazione BLOB.
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

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>Creare uno snapshot di un disco gestito attualmente collegato a una macchina virtuale.
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

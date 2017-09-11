---
title: Librerie delle macchine virtuali di Azure per Python
description: 
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
ms.openlocfilehash: c25665e19adb44c7112bf1533097ce1e6c739cb8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-virtual-machine-libraries"></a>Librerie delle macchine virtuali di Azure

## <a name="overview"></a>Panoramica

Risorse di calcolo su richiesta e scalabili in esecuzione su Linux o Windows.

Per iniziare a usare le macchine virtuali di Azure, vedere [Creare una macchina virtuale Linux con il portale di Azure](/azure/virtual-machines/linux/quick-create-portal).

## <a name="management-api"></a>API di gestione

Creare, configurare, gestire e ridimensionare le macchine virtuali Windows e Linux in Azure dal codice con l'API di gestione.

Installare la libreria tramite pip.

```bash
pip install azure-mgmt-compute 
```   

### <a name="example"></a>Esempio

Creare una nuova macchina virtuale Linux in un gruppo di risorse di Azure esistente.

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
> [Esplorare le API di gestione](/python/api/overview/azure/virtualmachines/managementlibrary)

## <a name="samples"></a>Esempi

* [Gestire le macchine virtuali][1]
* [Gestire un servizio di bilanciamento del carico][2]
* [Creare e configurare dischi gestiti][3]
* [Elencare le immagini][4] 
* [Monitorare le macchine virtuali][5]

Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=virtual-machines) di esempi di macchine virtuali.

[1]: https://azure.microsoft.com/resources/samples/virtual-machines-python-manage/
[2]: https://azure.microsoft.com/resources/samples/network-python-manage-loadbalancer
[3]: ../docs-ref-conceptual/python-sdk-azure-samples-managed-disks.md
[4]: ../docs-ref-conceptual/python-sdk-azure-samples-list-images.md
[5]: ../docs-ref-conceptual/python-sdk-azure-samples-monitor-vms.md
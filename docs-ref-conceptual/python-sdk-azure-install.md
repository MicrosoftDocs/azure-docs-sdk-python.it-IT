---
title: Installazione
description: Come installare Azure Python SDK
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a>Installazione

## <a name="installation-with-pip"></a>Installazione con pip

È possibile installare singolarmente la libreria di ogni servizio di Azure:

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

I pacchetti di anteprima possono essere installati mediante il flag `--pre` :

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

È inoltre possibile installare un set di librerie di Azure in una singola riga utilizzando il meta pacchetto `azure` .

```bash
pip install azure
```

Viene pubblicata una versione di anteprima del pacchetto, a cui è possibile accedere tramite il flag --pre:

```bash
pip install --pre azure
```

## <a name="install-from-github"></a>Eseguire l'installazione da GitHub

Se si vuole installare `azure` dall'origine:

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install

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
ms.openlocfilehash: 5ce4ef27667d45697200eef67be92c62812b3809
ms.sourcegitcommit: c57305dad01cad925faf50a64953c408429d4ca9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/05/2017
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

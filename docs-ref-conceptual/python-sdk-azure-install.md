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
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
---
# <a name="installation"></a>Installazione

## <a name="which-python-and-which-version-to-use"></a>Tipo e versione di Python da utilizzare
Sono disponibili diversi interpreti Python, ad esempio:

* CPython: l'interprete Python standard e più comunemente usato
* PyPy - implementazione alternativa rapida e conforme a CPython
* IronPython: interprete Python eseguito in .NET/CLR
* Jython: interprete Python eseguito sulla macchina virtuale Java

**CPython** versione v2.7 o v3.4+ e PyPy 5.4.0 sono testati e supportati per Python Azure SDK.

## <a name="where-to-get-python"></a>Dove è possibile reperire Python
È possibile ottenere CPython in diversi modi:

* Direttamente da [Python](https://www.python.org/)
* Da un distributore attendibile, ad esempio [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) o [ActiveState](https://www.activestate.com/)
* Compilandolo da codice sorgente.

Salvo esigenze specifiche, si consiglia di scegliere le prime due opzioni.

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
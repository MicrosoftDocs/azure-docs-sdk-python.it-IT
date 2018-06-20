---
title: Librerie di Azure Active Directory per Python
description: Documentazione di riferimento per le librerie client Python per Azure Active Directory
keywords: Azure, Python, SDK, API, SQL, autenticazione, AAD, Active Directory, Graph, OAuth 2.0
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/07/2017
ms.topic: reference
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: active-directory
ms.openlocfilehash: 78df70001dd0d55ac2c9c9da04fac6a51c5919e6
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
ms.locfileid: "29478924"
---
# <a name="azure-active-directory-libraries-for-python"></a>Librerie di Azure Active Directory per Python

## <a name="overview"></a>Panoramica

Con [Azure Active Directory](/azure/active-directory/active-directory-whatis) è possibile consentire l'accesso degli utenti e controllare l'accesso ad applicazioni e API.

## <a name="client-library"></a>Libreria client

Configurare l'autenticazione OAuth2, OpenID Connect o Active Directory Graph e l'accesso Single Sign-On [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) con [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python).

```bash
pip install azure-graphrbac
```

### <a name="example"></a>Esempio
> [!NOTE]
> È necessario modificare il parametro della risorsa in https://graph.windows.net durante la creazione dell'istanza delle credenziali

```python
from azure.graphrbac import GraphRbacManagementClient
from azure.common.credentials import UserPassCredentials

credentials = UserPassCredentials(
            'user@domain.com',      # Your user
            'my_password',          # Your password
            resource="https://graph.windows.net"
    )

tenant_id = "myad.onmicrosoft.com"

graphrbac_client = GraphRbacManagementClient(
    credentials,
    tenant_id
)
```
Il codice seguente consente di creare un utente, di ottenerlo direttamente e tramite l'applicazione di filtri all'elenco e quindi di eliminarlo.
```python
from azure.graphrbac.models import UserCreateParameters, PasswordProfile

user = graphrbac_client.users.create(
    UserCreateParameters(
        user_principal_name="testbuddy@{}".format(MY_AD_DOMAIN),
        account_enabled=False,
        display_name='Test Buddy',
        mail_nickname='testbuddy',
        password_profile=PasswordProfile(
            password='MyStr0ngP4ssword',
            force_change_password_next_login=True
        )
    )
)
# user is a User instance
self.assertEqual(user.display_name, 'Test Buddy')

user = graphrbac_client.users.get(user.object_id)
self.assertEqual(user.display_name, 'Test Buddy')

for user in graphrbac_client.users.list(filter="displayName eq 'Test Buddy'"):
    self.assertEqual(user.display_name, 'Test Buddy')

graphrbac_client.users.delete(user.object_id)
```

> [!div class="nextstepaction"]
> [Esplorare le API client](/python/api/overview/azure/activedirectory/client)

Esplorare altri [esempi di codice Python per Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python) disponibili per l'uso nelle app.
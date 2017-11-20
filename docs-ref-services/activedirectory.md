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
ms.openlocfilehash: 41234fd44fa98c1ff57287193b0437b7caca46c8
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-libraries-for-python"></a><span data-ttu-id="4b2ff-104">Librerie di Azure Active Directory per Python</span><span class="sxs-lookup"><span data-stu-id="4b2ff-104">Azure Active Directory libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="4b2ff-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4b2ff-105">Overview</span></span>

<span data-ttu-id="4b2ff-106">Con [Azure Active Directory](/azure/active-directory/active-directory-whatis) è possibile consentire l'accesso degli utenti e controllare l'accesso ad applicazioni e API.</span><span class="sxs-lookup"><span data-stu-id="4b2ff-106">Sign-on users and control access to applications and APIs with [Azure Active Directory](/azure/active-directory/active-directory-whatis).</span></span>

## <a name="client-library"></a><span data-ttu-id="4b2ff-107">Libreria client</span><span class="sxs-lookup"><span data-stu-id="4b2ff-107">Client library</span></span>

<span data-ttu-id="4b2ff-108">Configurare l'autenticazione OAuth2, OpenID Connect o Active Directory Graph e l'accesso Single Sign-On [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) con [Azure Active Directory Authentication Library (ADAL) per Python](https://github.com/AzureAD/azure-activedirectory-library-for-python).</span><span class="sxs-lookup"><span data-stu-id="4b2ff-108">Configure OAuth2, OpenID Connect, or Active Directory Graph authentication and [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) single-sign on with the [Azure Active Directory authentication library (ADAL) for Python](https://github.com/AzureAD/azure-activedirectory-library-for-python).</span></span>

```bash
pip install azure-graphrbac
```

### <a name="example"></a><span data-ttu-id="4b2ff-109">Esempio</span><span class="sxs-lookup"><span data-stu-id="4b2ff-109">Example</span></span>
> [!NOTE]
> <span data-ttu-id="4b2ff-110">È necessario modificare il parametro della risorsa in https://graph.windows.net durante la creazione dell'istanza delle credenziali</span><span class="sxs-lookup"><span data-stu-id="4b2ff-110">You need to change the resource parameter to https://graph.windows.net while creating the credentials instance</span></span>

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
<span data-ttu-id="4b2ff-111">Il codice seguente consente di creare un utente, di ottenerlo direttamente e tramite l'applicazione di filtri all'elenco e quindi di eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="4b2ff-111">The following code creates a user, get it directly and by list filtering, and then delete it.</span></span>
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
> [<span data-ttu-id="4b2ff-112">Esplorare le API client</span><span class="sxs-lookup"><span data-stu-id="4b2ff-112">Explore the Client APIs</span></span>](/python/api/overview/azure/activedirectory/clientlibrary?)

<span data-ttu-id="4b2ff-113">Esplorare altri [esempi di codice Python per Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python) disponibili per l'uso nelle app.</span><span class="sxs-lookup"><span data-stu-id="4b2ff-113">Explore more [sample Python code for Azure AD](https://azure.microsoft.com/en-us/resources/samples/?term=active+directory&platform=python) you can use in your apps.</span></span>
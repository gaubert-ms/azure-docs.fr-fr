---
title: Joindre un nouvel appareil Windows 10 à Azure AD lors de la première utilisation | Microsoft Docs
description: Rubrique qui explique comment les utilisateurs peuvent configurer Azure AD Join lors de la première utilisation.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/25/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3693115d72d9928bea0ca8bbe4fae5858e0f3cb7
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54446423"
---
# <a name="tutorial-join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Didacticiel : Joindre un nouvel appareil Windows 10 à Azure AD lors de la première utilisation

La fonction de gestion des appareils intégrée à Azure Active Directory (Azure AD) vous permet de vous assurer que vos utilisateurs accèdent à vos ressources à partir d’appareils qui répondent à vos normes de conformité et de sécurité. Pour plus d’informations, consultez [Présentation de la gestion des appareils dans Azure Active Directory](overview.md).

Dans Windows 10, vous pouvez joindre un nouvel appareil à Azure AD dès la première utilisation.  
Ainsi, vous pouvez distribuer des appareils neufs à vos employés ou étudiants.

Si Windows 10 Professionnel ou Windows 10 Entreprise est installé sur un appareil, le processus exécuté par défaut est l’installation pour les appareils d’entreprise.

Dans *l’expérience OOBE (out-of-box experience)* de Windows, la jonction à un domaine Active Directory (AD) local n’est pas prise en charge. Si vous souhaitez joindre un ordinateur à un domaine AD, vous devez sélectionner le lien **Configurer Windows à l’aide d’un compte local** pendant la configuration. Vous pouvez ensuite joindre l’ordinateur au domaine par le biais des paramètres de votre ordinateur.
 
Dans le cadre de ce didacticiel, vous allez apprendre à joindre un appareil à Azure AD lors de la première utilisation :
 > [!div class="checklist"]
> * Prérequis
> * Joindre un appareil
> * Vérification

## <a name="prerequisites"></a>Prérequis

Pour joindre un appareil Windows 10, le service d’inscription des appareils doit être configuré pour vous permettre d’inscrire des appareils. En plus d’avoir l’autorisation de joindre des appareils dans votre locataire Azure AD, vous devez avoir moins d’appareils inscrits que le nombre maximal configuré. Pour plus d’informations, consultez [Configurer les paramètres de l’appareil](device-management-azure-portal.md#configure-device-settings).

De plus, si votre locataire est fédéré, votre fournisseur d’identité DOIT prendre en charge le point de terminaison nom d’utilisateur/mot de passe WS-Fed et WS-Trust. Il peut s’agir de la version 1.3 ou de la version 2005. La prise en charge de ce protocole est nécessaire tant pour joindre l’appareil à Azure AD que pour ouvrir une session sur l’appareil avec un mot de passe.

## <a name="joining-a-device"></a>Joindre un appareil

**Pour joindre un appareil Windows 10 à Azure AD lors de la première utilisation :**


1. Lorsque vous mettez sous tension votre nouveau périphérique et démarrez le processus d'installation, le message **Préparation** s’affiche. Suivez les invites pour configurer votre appareil.

2. Commencez par personnaliser votre région et votre langue. Ensuite, acceptez les termes du contrat de licence du logiciel Microsoft.
 
    ![Personnalisation pour votre région](./media/azuread-joined-devices-frx/01.png)

3. Sélectionnez le réseau que vous souhaitez utiliser pour la connexion à Internet.

4. Cliquez sur **Cet appareil appartient à mon organisation**. 

    ![Écran « À qui appartient cet appareil ? »](./media/azuread-joined-devices-frx/02.png)

5. Entrez les informations d’identification que vous a fournies votre organisation, puis cliquez sur **Se connecter**.

    ![Écran de connexion](./media/azuread-joined-devices-frx/03.png)

6. Votre appareil localise un locataire correspondant dans Azure AD. Si vous faites partie d’un domaine fédéré, vous êtes redirigé vers votre serveur STS (Secure Token Service) local, par exemple les services de fédération Azure Directory (AD FS).

7. Si vous êtes un utilisateur dans des domaines non fédérés, entrez vos informations d'identification directement sur la page hébergée sur Azure AD. 

8. Vous devrez ensuite fournir les informations nécessaires à l’authentification multifacteur. 
 
9. Azure AD vérifie si une inscription est exigée dans la gestion des appareils mobiles.

10. Windows inscrit l’appareil dans l’annuaire de l’organisation dans Azure AD, puis l’inscrit à la gestion des appareils mobiles, si nécessaire.

11. Si vous êtes :
    - Un utilisateur géré, Windows vous redirige vers le Bureau par le biais du processus de connexion automatique.

    - Un utilisateur fédéré, vous accédez à l’écran de connexion de Windows pour entrer vos informations d’identification.

## <a name="verification"></a>Vérification

Pour vérifier si un appareil est joint à Azure AD, consultez la boîte de dialogue **Accès Professionnel ou Scolaire** sur votre appareil Windows. La boîte de dialogue doit indiquer que vous êtes connecté à votre annuaire Azure AD.

![Accès Professionnel ou Scolaire](./media/azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations, consultez [Présentation de la gestion des appareils dans Azure Active Directory](overview.md).

- Pour plus d’informations sur la gestion des appareils dans le portail Azure AD, consultez [Gestion des appareils avec le portail Azure](device-management-azure-portal.md).

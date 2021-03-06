---
title: 'Tutoriel : Intégration d’Azure Active Directory avec PolicyStat | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et PolicyStat.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f8dad5e3b5bfd908e8a3b3c1f1bcc78dd689757a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823876"
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Tutoriel : Intégration d’Azure Active Directory avec PolicyStat

Dans ce didacticiel, vous allez apprendre à intégrer PolicyStat avec Azure Active Directory (Azure AD).

L’intégration de PolicyStat avec Azure AD vous offre les avantages suivants :

- Vous pouvez contrôler dans Azure AD qui a accès à PolicyStat.
- Vous pouvez permettre à vos utilisateurs d’être automatiquement connectés à PolicyStat (via l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes à partir d’un emplacement central : le portail Azure.

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec PolicyStat, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement PolicyStat pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de PolicyStat à partir de la galerie
1. Configuration et test de l’authentification unique Azure AD

## <a name="adding-policystat-from-the-gallery"></a>Ajout de PolicyStat à partir de la galerie
Pour configurer l’intégration de PolicyStat à Azure AD, vous devez ajouter PolicyStat à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter PolicyStat à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**. 

    ![Active Directory][1]

1. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![APPLICATIONS][2]
    
1. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![APPLICATIONS][3]

1. Dans la zone de recherche, tapez **PolicyStat**.

    ![Création d’un utilisateur de test Azure AD](./media/policystat-tutorial/tutorial_policystat_search.png)

1. Dans le volet de résultats, sélectionnez **PolicyStat**, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Création d’un utilisateur de test Azure AD](./media/policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuration et test de l’authentification unique Azure AD
Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec PolicyStat sur un utilisateur de test nommé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur PolicyStat équivalent à l’utilisateur dans Azure AD. En d’autres termes, une relation entre l’utilisateur Azure AD et l’utilisateur PolicyStat associé doit être établie.

Dans PolicyStat, affectez la valeur de **nom d’utilisateur** dans Azure AD comme valeur **Nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec PolicyStat, vous devez suivre les indications des sections suivantes :

1. **[Configuration de l’authentification unique Azure AD](#configuring-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
1. **[Création d’un utilisateur de test Azure AD](#creating-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
1. **[Création d’un utilisateur de test PolicyStat](#creating-a-policystat-test-user)** pour obtenir un équivalent de Britta Simon dans PolicyStat lié à la représentation Azure AD associée.
1. **[Affectation de l’utilisateur de test Azure AD](#assigning-the-azure-ad-test-user)** : permet à Britta Simon d’utiliser l’authentification unique Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** pour vérifier si la configuration fonctionne.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuration de l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application PolicyStat.

**Pour configurer l’authentification unique Azure AD avec PolicyStat, procédez comme suit :**

1. Dans le portail Azure, sur la page d’intégration de l’application **PolicyStat**, cliquez sur **Authentification unique**.

    ![Configurer l'authentification unique][4]

1. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.
 
    ![Configurer l'authentification unique](./media/policystat-tutorial/tutorial_policystat_samlbase.png)

1. Dans la section **Domaine et URL PolicyStat**, procédez comme suit :

    ![Configurer l'authentification unique](./media/policystat-tutorial/tutorial_policystat_url.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<companyname>.policystat.com`

    b. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez l’[équipe de support technique PolicyStat](http://www.policystat.com/support/). 
 
1. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Configurer l'authentification unique](./media/policystat-tutorial/tutorial_policystat_certificate.png) 

1. Cette section explique comment permettre aux utilisateurs de s’authentifier sur PolicyStat avec leur compte Azure AD en utilisant la fédération basée sur le protocole SAML.

    L’application PolicyStat s’attend à recevoir les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration **Attributs du jeton SAML**.  

     La capture d’écran suivante présente un exemple de cette opération.

     ![Attributs](./media/policystat-tutorial/tutorial_policystat_attribute.png "Attributs")

1. Pour ajouter les mappages d’attribut requis, procédez comme suit :

    | Nom de l'attribut    |   Valeur de l’attribut |
    |------------------- | -------------------- |
    | uid | ExtractMailPrefix([mail]) |
    
    a. Cliquez sur **Ajouter un attribut** pour ouvrir la boîte de dialogue **Ajouter un attribut**.

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_04.png)

    ![Configure Single Sign-On](./media/policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. Dans la zone de texte **Nom de l’attribut**, entrez **uid**.

    c. Dans la zone de texte **Valeur de l’attribut**, sélectionnez **ExtractMailPrefix()**.    
   
    d. Dans la liste **Mail**, sélectionnez **User.mail**.
    
    e. Cliquez sur **OK**.

1. Cliquez sur le bouton **Enregistrer** .

    ![Configurer l'authentification unique](./media/policystat-tutorial/tutorial_general_400.png)

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise PolicyStat en tant qu’administrateur.

1. Cliquez sur l’onglet **Admin**, puis sur **Single Sign-On Configuration** dans le volet de navigation de gauche.
   
    ![Menu administrateur](./media/policystat-tutorial/ic808633.png "Menu administrateur")

1. Dans la section **Setup**, sélectionnez **Enable Single Sign-on Integration**.
   
    ![Configuration de l’authentification unique](./media/policystat-tutorial/ic808634.png "Configuration de l’authentification unique")

1. Cliquez sur **Configure Attributes**, puis, dans la section **Configure Attributes**, procédez comme suit :
   
    ![Configuration de l’authentification unique](./media/policystat-tutorial/ic808635.png "Configuration de l’authentification unique")
   
    a. Dans la zone de texte **Username Attribute**, entrez **uid**.

    b. Dans la zone de texte **First Name Attribute**, entrez la valeur **firstname** de l’utilisateur **Britta**.

    c. Dans la zone de texte **Last Name Attribute**, entrez la valeur **lastname** de l’utilisateur **Simon**.

    d. Dans la zone de texte **Email Attribute**, entrez la valeur **emailaddress** de l’utilisateur **BrittaSimon@contoso.com**.

    e. Cliquez sur **Enregistrer les modifications**.

1. Cliquez sur **Your IDP Metadata**, puis, dans la section **Your IDP Metadata**, procédez comme suit :
   
    ![Configuration de l’authentification unique](./media/policystat-tutorial/ic808636.png "Configuration de l’authentification unique")
   
    a. Ouvrez votre fichier de métadonnées téléchargé, copiez son contenu, puis collez-le dans la zone de texte **Your Identity Provider Metadata**.

    b. Cliquez sur **Enregistrer les modifications**.

> [!TIP]
> Vous pouvez maintenant lire une version concise de ces instructions dans le [portail Azure](https://portal.azure.com), pendant que vous configurez l’application.  Après avoir ajouté cette application à partir de la section **Active Directory > Applications d’entreprise**, cliquez simplement sur l’onglet **Authentification unique** et accédez à la documentation incorporée par le biais de la section **Configuration** en bas. Pour en savoir plus sur la fonctionnalité de documentation incorporée, accédez à : [Documentation incorporée Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Création d’un utilisateur de test Azure AD
L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

![Créer un utilisateur Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **portail Azure**, cliquez sur l’icône **Azure Active Directory**.

    ![Création d’un utilisateur de test Azure AD](./media/policystat-tutorial/create_aaduser_01.png) 

1. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.
    
    ![Création d’un utilisateur de test Azure AD](./media/policystat-tutorial/create_aaduser_02.png) 

1. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue.
 
    ![Création d’un utilisateur de test Azure AD](./media/policystat-tutorial/create_aaduser_03.png) 

1. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :
 
    ![Création d’un utilisateur de test Azure AD](./media/policystat-tutorial/create_aaduser_04.png) 

    a. Dans la zone de texte **Nom**, entrez **BrittaSimon**.

    b. Dans la zone de texte **Nom d’utilisateur**, tapez **l’adresse e-mail** de Britta Simon.

    c. Sélectionnez **Afficher le mot de passe** et notez la valeur du **mot de passe**.

    d. Cliquez sur **Créer**.
 
### <a name="creating-a-policystat-test-user"></a>Création d’un utilisateur de test PolicyStat

Pour permettre aux utilisateurs Azure AD de se connecter à PolicyStat, vous devez les approvisionner dans PolicyStat.  

PolicyStat prend en charge l’approvisionnement juste-à-temps des utilisateurs. Il est donc inutile d’ajouter les utilisateurs manuellement à PolicyStat. Les utilisateurs seront ajoutés automatiquement à leur première connexion à l’aide de l’authentification unique.

>[!NOTE]
>Vous pouvez utiliser n’importe quel outil ou API de création de compte utilisateur, fourni par PolicyStat, pour approvisionner des comptes utilisateur Azure AD.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Affectation de l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à PolicyStat.

![Affecter des utilisateurs][200] 

**Pour affecter Britta Simon à PolicyStat, procédez comme suit :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201] 

1. Dans la liste des applications, sélectionnez **PolicyStat**.

    ![Configurer l'authentification unique](./media/policystat-tutorial/tutorial_policystat_app.png) 

1. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Affecter des utilisateurs][202] 

1. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Affecter des utilisateurs][203]

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

1. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

1. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.
    
### <a name="testing-single-sign-on"></a>Test de l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette PolicyStat dans le volet d’accès, vous êtes connecté automatiquement à votre application PolicyStat.
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/policystat-tutorial/tutorial_general_01.png
[2]: ./media/policystat-tutorial/tutorial_general_02.png
[3]: ./media/policystat-tutorial/tutorial_general_03.png
[4]: ./media/policystat-tutorial/tutorial_general_04.png

[100]: ./media/policystat-tutorial/tutorial_general_100.png

[200]: ./media/policystat-tutorial/tutorial_general_200.png
[201]: ./media/policystat-tutorial/tutorial_general_201.png
[202]: ./media/policystat-tutorial/tutorial_general_202.png
[203]: ./media/policystat-tutorial/tutorial_general_203.png


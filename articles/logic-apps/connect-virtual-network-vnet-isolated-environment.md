---
title: Connexion à des réseaux virtuels Azure à partir d’Azure Logic Apps via un environnement de service d’intégration (ISE)
description: Créer un environnement de service d’intégration (ISE, Integration Service Environment) afin que les applications logiques et les comptes d’intégration puissent accéder aux réseaux virtuels Azure, tout en restant privés et isolés de l’environnement Azure public ou « mondial »
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 12/06/2018
ms.openlocfilehash: 31f3cf9bd8f83c5da32569ed370de1ed35299749
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2019
ms.locfileid: "54062381"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-through-an-integration-service-environment-ise"></a>Connexion à des réseaux virtuels Azure à partir d’Azure Logic Apps via un environnement de service d’intégration (ISE)

> [!NOTE]
> Cette fonctionnalité est disponible en *préversion privée*. Pour demander l’accès, [créez votre demande de participation ici](https://aka.ms/iseprivatepreview).

Pour les scénarios où vos applications logiques et vos comptes d’intégration ont besoin d’accéder à un [réseau virtuel Azure](../virtual-network/virtual-networks-overview.md), créez un [*environnement de service d’intégration* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un environnement de service d’intégration est un environnement privé et isolé, qui utilise un stockage dédié et d’autres ressources conservées séparément du service Logic Apps public ou « mondial ». Cette séparation réduit également l’impact que d’autres locataires Azure peuvent avoir sur les performances de vos applications. Votre environnement ISE est *injecté* dans votre réseau virtuel Azure, qui déploie ensuite le service Logic Apps dans votre réseau virtuel. Quand vous créez une application logique ou un compte d’intégration, sélectionnez cet environnement ISE comme emplacement. Votre application logique ou votre compte d’intégration peut ensuite accéder directement à des ressources, comme des machines virtuelles, des serveurs, des systèmes et des services, dans votre réseau virtuel. 

![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Cet article vous explique comment effectuer ces tâches :

* Définissez les autorisations sur votre réseau virtuel Azure afin que l’instance privée de Logic Apps puisse accéder à votre réseau virtuel.

* Créez votre environnement de service d’intégration. 

* Créez une application logique qui peut s’exécuter dans votre environnement de service d’intégration. 

* Créez un compte d’intégration pour vos applications logiques dans votre environnement de service d’intégration.

Pour plus d’informations sur les environnements de service d’intégration, consultez [Accéder à des ressources Réseau virtuel Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure. Si vous n’avez pas d’abonnement Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscrivez-vous pour bénéficier d’un compte Azure gratuit</a>. 

  > [!IMPORTANT]
  > Les applications logiques, les actions intégrées et les connecteurs qui s’exécutent dans votre ISE utilisent un autre plan tarifaire que le tarif de facturation à l’utilisation. Pour plus d’informations, consultez [Tarifs Logic Apps](../logic-apps/logic-apps-pricing.md).

* Un [réseau virtuel Azure](../virtual-network/virtual-networks-overview.md). Si vous n’avez pas de réseau virtuel, découvrez comment [créer un réseau virtuel Azure](../virtual-network/quick-create-portal.md). 

* Pour donner à vos applications logiques un accès direct à votre réseau virtuel Azure, [configurez des autorisations de contrôle d’accès en fonction du rôle (RBAC)](#vnet-access), pour que le service Logic Apps dispose des autorisations permettant d’accéder à votre réseau virtuel. 

* Des connaissances de base en [création d’applications logiques](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-virtual-network-permissions"></a>Définir des autorisations de réseau virtuel

Quand vous créez un environnement de service d’intégration, vous pouvez sélectionner un réseau virtuel Azure dans lequel vous *injectez* votre environnement. Cependant, avant de pouvoir sélectionner votre réseau virtuel pour injecter votre environnement, vous devez configurer les autorisations de contrôle d’accès en fonction du rôle (RBAC) dans votre réseau virtuel. Pour configurer les autorisations, attribuez ces rôles spécifiques au service Azure Logic Apps :

1. Dans le [portail Azure](https://portal.azure.com), recherchez et sélectionnez votre réseau virtuel. 

1. Dans le menu de votre réseau virtuel, sélectionnez **Contrôle d’accès (IAM)**. 

1. Sous **Contrôle d’accès (IAM)**, choisissez **Ajouter une attribution de rôle**. 

   ![Ajouter des rôles](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Dans le volet **Ajouter une attribution de rôle**, ajoutez le rôle nécessaire au service Azure Logic Apps. 

   1. Sous **Rôle**, sélectionnez **Contributeur de réseaux**. 
   
   1. Sous **Attribuer l’accès à**, sélectionnez **Utilisateur, groupe ou principal du service Azure AD**.

   1. Sous **Sélectionner**, entrez **Azure Logic Apps**. 

   1. Une fois que la liste des membres apparaît, sélectionnez **Azure Logic Apps**. 

      > [!TIP]
      > Si vous ne trouvez pas ce service, entrez l’ID d’application du service Logic Apps : `7cd684f4-8a78-49b0-91ec-6a35d38739ba` 
   
   1. Une fois ces opérations effectuées, sélectionnez **Enregistrer**.

   Par exemple : 

   ![Ajouter une attribution de rôle](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

Pour plus d’informations, consultez [Autorisations d’accès au réseau virtuel](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Créer votre environnement de service d’intégration

Pour créer votre environnement de service d’intégration, effectuez les étapes suivantes :

1. Dans le [portail Azure](https://portal.azure.com), dans le menu principal d’Azure, sélectionnez **Créer une ressource**.

   ![Créer une ressource](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Dans la zone de recherche, entrez « environnement de service d’intégration » comme filtre.
Dans la liste des résultats, sélectionnez **Environnement de service d’intégration (préversion)**, puis choisissez **Créer**.

   ![Sélectionner « Environnement de service d’intégration »](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   ![Choisissez « Créer ».](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Spécifiez ces informations pour votre environnement, puis choisissez **Vérifier + créer**, par exemple :

   ![Spécifier les informations pour l’environnement](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propriété | Obligatoire | Valeur | Description |
   |----------|----------|-------|-------------|
   | **Abonnement** | Oui | <*Azure-subscription-name*> | Abonnement Azure à utiliser pour votre environnement | 
   | **Groupe de ressources** | Oui | <*nom-groupe-de-ressources-Azure*> | Groupe de ressources Azure où vous voulez créer votre environnement |
   | **Nom de l’environnement de service d’intégration** | Oui | <*nom-environnement*> | Nom à donner à votre environnement | 
   | **Lieu** | Oui | <*région-centre de données-Azure*> | Région du centre de données Azure où déployer votre environnement | 
   | **Capacité supplémentaire** | Oui | 0, 1, 2, 3 | Nombre d’unités de traitement à utiliser pour cette ressource ISE | 
   | **Réseau virtuel** | Oui | <*Azure-virtual-network-name*> | Réseau virtuel Azure où vous voulez injecter votre environnement, pour que les applications logiques de cet environnement puissent accéder à votre réseau virtuel. Si vous n’avez pas de réseau, vous devez en créer un ici. <p>**Important** : Vous pouvez effectuer cette injection *seulement*  quand vous créez votre ISE. Cependant, avant de pouvoir créer cette relation, vérifiez que vous avez déjà [configuré le contrôle d’accès en fonction du rôle dans votre réseau virtuel pour Azure Logic Apps](#vnet-access). | 
   | **Sous-réseaux** | Oui | <*subnet-resource-list*> | Un environnement ISE nécessite quatre sous-réseaux *vides* pour la création des ressources dans votre environnement. Par conséquent, assurez-vous que ces sous-réseaux *ne sont délégués à aucun service*. Vous *ne pouvez pas changer* ces adresses de sous-réseaux après avoir créé votre environnement. <p><p>Pour créer chaque sous-réseau, [suivez les étapes décrites dans ce tableau](#create-subnet). Chaque sous-réseau doit répondre aux critères suivants : <p>- Il doit être vide. <br>- Il utilise un nom qui ne commence pas par un chiffre ou par un trait d’union. <br>- Il utilise le format [CIDR (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) et un espace d’adressage de Classe B. <br>- Il comprend au moins un `/27` dans l’espace d’adressage, pour que le sous-réseau obtienne au moins 32 adresses. Pour en savoir plus sur le calcul du nombre d’adresses, consultez [Blocs CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks). Par exemple :  <p>- `10.0.0.0/24` a 256 adresses car 2<sup>(32-24)</sup> est égal à 2<sup>8</sup> soit 256. <br>- `10.0.0.0/27` a 32 adresses car 2<sup>(32-27)</sup> est égal à 2<sup>5</sup> soit 32. <br>- `10.0.0.0/28` a seulement 16 adresses car 2<sup>(32-28)</sup> est égal à 2<sup>4</sup> soit 16. |
   |||||

   <a name="create-subnet"></a>

   **Créer un sous-réseau**

   1. Sous la liste **Sous-réseaux**, choisissez **Gérer la configuration du sous-réseau**.

      ![Gérer la configuration du sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Dans le volet **Sous-réseaux**, choisissez **Sous-réseau**.

      ![Ajouter un sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Dans le volet **Ajouter un sous-réseau**, spécifiez ces informations.

      * **Nom** : Nom de votre sous-réseau
      * **Plage d’adresses (bloc CIDR)** : Plage de votre sous-réseau dans votre réseau virtuel, au format CIDR

      ![Détails de l’ajout d’un sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Une fois que vous avez terminé, sélectionnez **OK**.

   1. Répétez ces étapes pour trois autres sous-réseaux.

1. Une fois qu’Azure a validé les informations de votre ISE, choisissez **Créer**, par exemple :

   ![Après la validation, choisissez « Créer »](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure commence le déploiement de votre environnement, mais ce processus *peut prendre* jusqu’à deux heures. 
   Pour vérifier l’état du déploiement, dans votre barre d’outils Azure, choisissez l’icône Notifications, qui ouvre le volet Notifications.

   ![Vérifier l’état du déploiement](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Un fois le déploiement terminé, Azure montre cette notification :

   ![Déploiement réussi](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Si le déploiement échoue ou si vous supprimez votre ISE, Azure *peut* prendre jusqu’à une heure avant de libérer vos sous-réseaux. Par conséquent, vous devrez peut-être attendre avant de réutiliser ces sous-réseaux dans un autre ISE.

1. Pour voir votre environnement, choisissez **Accéder à la ressource** si Azure n’accède pas automatiquement à votre environnement une fois le déploiement terminé.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Créer une application logique - Environnement de service d’intégration

Pour créer des applications logiques qui utilisent votre environnement de service d’intégration, effectuez les étapes de [Guide pratique pour créer une application logique](../logic-apps/quickstart-create-first-logic-app-workflow.md), mais avec ces différences : 

* Quand vous créez votre application logique, sous la propriété **Emplacement**, sélectionnez votre ISE dans la section **Environnements de service d’intégration**, par exemple :

  ![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Vous pouvez utiliser les mêmes déclencheurs et les mêmes actions intégrés, comme HTTP, qui s’exécutent dans le même environnement de service d’intégration que votre application logique. Les connecteurs avec l’étiquette **ISE** peuvent également s’exécuter dans le même environnement de service d’intégration votre application logique. Les connecteurs ne présentant pas le libellé **ISE** s’exécutent dans le service Logic Apps global.

  ![Sélection de connecteurs ISE](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Après avoir injecté votre ISE dans un réseau virtuel Azure, les applications logiques de votre environnement de service d’intégration peuvent accéder directement aux ressources de ce réseau virtuel. Pour les systèmes locaux qui sont connectés à un réseau virtuel, injectez un environnement de service d’intégration dans ce réseau, pour que vos applications logiques puissent accéder directement à ces systèmes en utilisant un de ces éléments : 

  * Connecteur ISE pour ce système, par exemple, SQL Server
  
  * Action HTTP 
  
  * Connecteur personnalisé

  Pour les systèmes locaux qui ne se trouvent pas dans un réseau virtuel ou qui n’ont pas de connecteurs ISE, [configurez d’abord la passerelle de données locale](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Créer un compte d’intégration - Environnement de service d’intégration

Pour utiliser un compte d’intégration avec des applications logiques dans un environnement de service d’intégration, ce compte d’intégration doit utiliser le *même environnement* que les applications logiques. Les applications logiques dans un environnement de service d’intégration peuvent référencer seulement des comptes d’intégration de ce même environnement. 

Pour créer un compte d’intégration qui utilise un environnement de service d’intégration, effectuez les étapes de [Guide pratique pour créer des comptes d’intégration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), sauf pour la propriété **Emplacement**, où la section  **Environnements de service d’intégration** apparaît maintenant. À la place, sélectionnez votre environnement de service d’intégration au lieu d’une région, par exemple :

![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Obtenir de l’aide

* Si vous avez des questions, consultez le <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">forum Azure Logic Apps</a>.
* Pour voter pour des idées de fonctionnalités ou pour en soumettre, visitez le <a href="https://aka.ms/logicapps-wish" target="_blank">site de commentaires des utilisateurs Logic Apps</a>.

## <a name="next-steps"></a>Étapes suivantes

* En apprendre davantage sur le [Réseau virtuel Azure](../virtual-network/virtual-networks-overview.md)
* Découvrir l’[Intégration d’un réseau virtuel pour les services Azure](../virtual-network/virtual-network-for-azure-services.md)

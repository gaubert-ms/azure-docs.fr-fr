---
title: Gérer les comptes de stockage Azure Stack | Microsoft Docs
description: Découvrez comment rechercher, gérer, restaurer et récupérer des comptes de stockage Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 01/18/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 149da2c186ae1e0c3fa5af14a630a0edef7deb25
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470271"
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Gérer les comptes de stockage dans Azure Stack

Découvrez comment gérer les comptes de stockage dans Azure Stack pour rechercher, restaurer et récupérer de la capacité de stockage en fonction des besoins de l’entreprise.

## <a name="find-a-storage-account"></a>Rechercher un compte de stockage
La liste des comptes de stockage de la région peut être affichée dans Azure Stack comme suit :

1. Connectez-vous au [portail d’administration](https://adminportal.local.azurestack.external).

2. Sélectionnez **Tous les services** > **Comptes de stockage**.

   ![](media/azure-stack-manage-storage-accounts/image4.png)

Par défaut, les 10 premiers comptes sont affichés. Vous pouvez choisir d’en afficher plus en cliquant sur le lien **Charger plus** en bas de la liste.

Ou

Si vous êtes intéressé par un compte de stockage particulier, vous pouvez **filtrer et extraire les comptes appropriés** uniquement.


**Pour filtrer les comptes :**

1. Sélectionnez **Filtrer** en haut du volet.
2. Dans le volet Filtrer, vous pouvez spécifier un **nom de compte**, un **ID d’abonnement ou un **statut** pour affiner la liste des comptes de stockage à afficher. Utilisez-les pour filtrer selon vos besoins.
3. Sélectionnez **Update**. La liste est normalement actualisée en conséquence.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Pour réinitialiser le filtre, sélectionnez **Filtrer**, effacez les sélections, puis mettez à jour la liste.

La zone de texte de recherche (en haut du volet de la liste de comptes de stockage) vous permet de mettre en surbrillance le texte sélectionné dans la liste des comptes. Vous pouvez l’utiliser lorsque le nom complet ou l’ID n’est pas facilement disponible.

Vous pouvez utiliser ici du texte libre pour rechercher le compte qui vous intéresse.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Accéder aux détails du compte
Une fois que vous avez trouvé les comptes qui vous intéressent, vous pouvez sélectionner un compte particulier pour afficher certains détails. Un nouveau volet s’ouvre avec les informations détaillées du compte, comme le type du compte, l’heure de création, l’emplacement, etc.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Récupérer un compte supprimé
Il peut être parfois nécessaire de récupérer un compte supprimé.

Dans Azure Stack, il existe un moyen simple de le faire :

1. Accédez à la liste de comptes de stockage. Pour plus d’informations, consultez [Rechercher un compte de stockage](#find) dans cet article.
2. Localisez ce compte particulier dans la liste. Il peut être nécessaire de filtrer.
3. Vérifiez l’*état* du compte. Il doit être **Supprimé**.
4. Sélectionnez le compte pour ouvrir le volet des détails du compte.
5. En haut de ce volet, recherchez le bouton **Récupérer** et sélectionnez-le.
6. Sélectionnez **Oui** pour confirmer.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. La récupération est maintenant *en cours... Attendez* un message indiquant que la récupération est effective.
   Vous pouvez aussi sélectionner l’icône représentant une cloche en haut du portail, afin d’afficher les indications sur la progression.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Une fois que le compte récupéré est synchronisé, il peut à nouveau être utilisé.

### <a name="some-gotchas"></a>Quelques astuces
* Votre compte supprimé affiche un état **hors conservation**.
  
  « Hors rétention » signifie que le compte supprimé a dépassé la période de conservation et qu’il n’est peut-être pas récupérable.
* Votre compte supprimé n’apparaît pas dans la liste des comptes.
  
  Il se peut que votre compte ne s’affiche pas dans la liste des comptes lorsque les comptes supprimés ont déjà été effacés. Dans ce cas, il ne peut pas être récupéré. Consultez [Récupérer de la capacité](#reclaim) dans cet article.

## <a name="set-the-retention-period"></a>Définir la période de conservation
Le paramètre de période de conservation permet à un opérateur cloud de spécifier une période de temps en jours (entre 0 et 9 999 jours) pendant laquelle un compte supprimé peut être récupéré. La période de rétention par défaut est définie sur 0 jour. Pour ce paramètre, la valeur « 0 » signifie qu’un compte supprimé n’est plus conservé et qu’il est marqué comme devant faire l’objet d’un nettoyage périodique de la mémoire.

**Pour changer la période de conservation :**

1. Connectez-vous au [portail d’administration](https://adminportal.local.azurestack.external).
2. Sélectionnez **Tous les services** > **Gestion des régions** sous **Administration**.
3. Sélectionnez **Stockage** dans la liste **Fournisseurs de ressources**.
4. Sélectionnez **Paramètres** en haut pour ouvrir le volet des paramètres.
5. Sélectionnez **Configuration**, puis modifiez la valeur de la période de conservation.

   Définissez le nombre de jours et enregistrez-le.
   
   Cette valeur est prise en compte immédiatement et est appliquée à toute votre région.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Récupérer de la capacité
Un des effets secondaires de la période de conservation est qu’un compte supprimé continue à consommer de la capacité jusqu’à ce qu’il sorte de cette période de conservation. En tant qu’opérateur cloud, vous pouvez avoir besoin d’un moyen de récupérer l’espace du compte supprimé, même si la période de conservation n’a pas encore expiré.

Vous pouvez récupérer de la capacité en utilisant le portail ou PowerShell.

**Pour récupérer de la capacité en utilisant le portail :**
1. Accédez au volet des comptes de stockage. Consultez [Rechercher un compte de stockage](#find).
2. Sélectionnez **Récupérer de l’espace** en haut du volet.
3. Lisez le message, puis sélectionnez **OK**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Attendez la notification de la réussite de l’opération. Utilisez l’icône de cloche sur le portail.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Actualisez la page Comptes de stockage. Les comptes supprimés ne figurent plus dans la liste, car ils ont été supprimés définitivement.

Vous pouvez aussi utiliser PowerShell pour remplacer explicitement la période de rétention et récupérer immédiatement de la capacité.

**Pour récupérer de la capacité en utilisant PowerShell :**   

1. Vérifiez qu’Azure PowerShell est installé et configuré. Dans le cas contraire, suivez ces instructions : 
   * Pour installer la dernière version d’Azure PowerShell et l’associer à votre abonnement Azure, consultez [Installer et configurer Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Pour plus d’informations sur les applets de commande Azure Resource Manager, consultez [Utilisation d’Azure PowerShell avec Azure Resource Manager](https://go.microsoft.com/fwlink/?LinkId=394767).
2. Exécutez les applets de commande suivantes :

> [!NOTE]  
> Si vous exécutez ces applets de commande, vous supprimez définitivement le compte et son contenu. Il n’est pas récupérable. Utilisez cette option avec précaution.

```PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
````

Pour plus d’informations, consultez la [documentation Azure Stack PowerShell](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
 

## <a name="next-steps"></a>Étapes suivantes

 - Pour plus d’informations sur la gestion des autorisations, consultez [Gérer le contrôle d’accès en fonction du rôle](azure-stack-manage-permissions.md).
 - Pour plus d’informations sur la gestion de la capacité de stockage pour Azure Stack, consultez [Gérer la capacité de stockage pour Azure Stack](azure-stack-manage-storage-shares.md).
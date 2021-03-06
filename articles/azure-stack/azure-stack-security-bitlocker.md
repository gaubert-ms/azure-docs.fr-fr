---
title: Chiffrement des données au repos dans Azure Stack
description: Découvrez comment Azure Stack protège vos données avec le chiffrement au repos
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 12/06/2018
ms.author: patricka
ms.reviewer: fiseraci
keywords: ''
ms.openlocfilehash: f9e76b255647f62b273fef8336ed845e365261cf
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728503"
---
# <a name="data-at-rest-encryption-in-azure-stack"></a>Chiffrement des données au repos dans Azure Stack

Azure Stack protège les données des utilisateurs et de l’infrastructure au niveau du sous-système de stockage à l’aide du chiffrement au repos. Le sous-système de stockage Azure Stack est chiffré à l’aide de BitLocker avec le chiffrement AES 128 bits. Les clés BitLocker sont conservées dans un magasin des secrets interne.

Le chiffrement des données au repos sont une exigence courante pour la plupart des normes de conformité (par exemple, la norme PCI-DSS, FedRAMP, HIPAA). Azure Stack vous permet de respecter ces exigences sans travail ou configuration requise supplémentaire. Pour plus d’informations sur la méthode employée par Azure Stack pour vous aider à respecter les normes de conformité, consultez le [portail Microsoft Service Trust](https://aka.ms/AzureStackCompliance).

> [!NOTE]
> Le chiffrement des données au repos empêche une personne ayant volé un ou plusieurs de vos disques durs physiques d’accéder aux données qu’ils contiennent. Le chiffrement des données au repos ne protège pas vos données contre les interceptions sur le réseau (lorsqu’elles sont en transit), les interceptions en cours d’utilisation (lorsqu’elles sont utilisées en mémoire) ou, plus généralement, les exfiltrations lorsque que le système est opérationnel et en cours d’exécution.

## <a name="retrieving-bitlocker-recovery-keys"></a>Récupération des clés de récupération BitLocker

Les clés d’Azure Stack BitLocker pour les données au repos sont gérées en interne. Vous n’avez pas besoin de les fournir pour les opérations normales ou lors du démarrage du système. Toutefois, certains scénarios de prise en charge peuvent nécessiter des clés de récupération BitLocker pour restaurer le système en ligne.  

> [!WARNING]
> Récupérez vos clés de récupération BitLocker et stockez-les dans un emplacement sécurisé en dehors d’Azure Stack. Si vous n’avez pas de clés de récupération durant certains scénarios de prise en charge, vous risquez de perdre des données devoir restaurer votre système à partir d’une image de sauvegarde.

La récupération des clés de récupération BitLocker nécessite l’accès à votre [point de terminaison privilégié](azure-stack-privileged-endpoint.md) (PEP). À partir d’une session PEP, exécutez la cmdlet Get-AzsRecoveryKeys.

```powershell
##This cmdlet retrieves the recovery keys for all the volumes that are encrypted with BitLocker.
Get-AzsRecoveryKeys
```

Paramètres facultatifs pour la cmdlet *Get-AzsRecoveryKeys* :

| Paramètre | Description | type | Obligatoire |
|---------|---------|---------|---------|
|*raw* | retourne les données brutes du mappage entre la clé de récupération, le nom de l’ordinateur et l’ID de mot de passe de chaque volume chiffré  | commutateur | Aucun (conçu pour les scénarios de prise en charge)|


## <a name="troubleshoot-issues"></a>Problèmes de dépannage

Dans des circonstances extrêmes, une requête de déverrouillage BitLocker peut échouer et empêcher un volume spécifique de démarrer. Selon la disponibilité de certains composants de l’architecture, cet échec peut entraîner du temps d’arrêt et la perte de données si vous n’avez pas vos clés de récupération BitLocker.

> [!WARNING]
> Récupérez vos clés de récupération BitLocker et stockez-les dans un emplacement sécurisé en dehors d’Azure Stack. Si vous n’avez pas de clés de récupération durant certains scénarios de prise en charge, vous risquez de perdre des données devoir restaurer votre système à partir d’une image de sauvegarde.

Si vous pensez que votre système rencontre des problèmes avec BitLocker, par exemple, si Azure Stack ne parvient pas à démarrer, contactez le support technique. Les membres du support technique auront besoin de vos clés de récupération BitLocker. La plupart des problèmes liés à BitLocker peuvent être résolus avec une opération FRU pour la machine virtuelle/l’hôte/le volume concerné. Pour les autres cas, une procédure de déverrouillage manuelle à l’aide de clés de récupération BitLocker peut être effectuée. Si les clés de récupération BitLocker ne sont pas disponibles, la seule option est une restauration à partir d’une image de sauvegarde. Vous risquez de perdre des données si votre dernière sauvegarde date n’est pas récente.

## <a name="next-steps"></a>Étapes suivantes

- [Apprenez-en davantage sur la sécurité dans Azure Stack](azure-stack-security-foundations.md)
- Pour plus d’informations sur la manière dont BitLocker protège les CSV, consultez [protection des volumes partagés de cluster et des réseaux de zone de stockage avec BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/protecting-cluster-shared-volumes-and-storage-area-networks-with-bitlocker).
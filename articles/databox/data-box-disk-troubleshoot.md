---
title: Résolution des problèmes liés à Azure Data Box Disk | Microsoft Docs
description: Décrit la résolution des problèmes rencontrés dans Azure Data Box Disk.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: 13e42ae9b25c769a468a8c9839fd1aaf0abf2787
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54213142"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk"></a>Résoudre les problèmes rencontrés dans Azure Data Box Disk

Cet article s’applique à Microsoft Azure Data Box Disk et décrit les flux de travail utilisés pour résoudre les problèmes que vous rencontrez lors du déploiement de cette solution. 

Cet article inclut les sections suivantes :

- Télécharger les journaux de diagnostic
- Interroger les journaux d’activité
- Erreurs de l’outil de déverrouillage de Data Box Disk
- Erreurs de l’outil de copie Data Box Disk Split

## <a name="download-diagnostic-logs"></a>Télécharger les journaux de diagnostic

Si des erreurs se produisent pendant le processus de copie des données, le portail affiche un chemin d’accès au dossier dans lequel se trouvent les journaux de diagnostic. 

Les journaux de diagnostic peuvent être :
- Des journaux d’erreurs
- Des journaux détaillés  

Pour accéder au chemin d’accès du journal de copie, accédez au compte de stockage associé à votre commande Data Box. 

1.  Accédez à **Général > Détails de la commande** et prenez note du compte de stockage associé à votre commande.
 

2.  Accédez à **Toutes les ressources** et recherchez le compte de stockage identifié à l’étape précédente. Sélectionnez le compte de stockage et cliquez dessus.

    ![Journaux de copie 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  Accédez à **Service BLOB > Parcourir les objets blob** et recherchez l’objet blob correspondant au compte de stockage. Accédez à **diagnosticslogcontainer > waies**. 

    ![Journaux de copie 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    Vous devez voir les journaux d’erreurs et les journaux détaillés pour la copie des données. Sélectionnez chaque fichier et cliquez dessus, puis téléchargez une copie locale.

## <a name="query-activity-logs"></a>Interroger les journaux d’activité

Utilisez les journaux d’activité pour rechercher une erreur lors de la résolution de problèmes ou pour surveiller la manière dont un utilisateur de votre organisation a modifié une ressource. Les journaux d’activité vous permettent de déterminer :

- Les opérations qui ont été effectuées sur les ressources de votre abonnement.
- Ce qui a initié l’opération. 
- Le moment où a eu lieu l’opération.
- L’état de l’opération.
- Les valeurs d’autres propriétés qui peuvent vous aider à effectuer des recherches sur l’opération.

Le journal d’activité contient toutes les opérations d’écriture (par exemple PUT, POST, DELETE) effectuées sur vos ressources, mais pas les opérations de lecture (comme GET). 

Les journaux d’activité sont conservés pendant 90 jours. Vous pouvez interroger une plage quelconque de dates, pour autant que la date de début ne remonte pas à plus de 90 jours dans le passé. Vous pouvez également filtrer le contenu en fonction d’une des requêtes intégrées dans Insights. Par exemple, cliquez sur une erreur, puis sélectionnez des échecs spécifiques et cliquez dessus pour comprendre la cause racine.

## <a name="data-box-disk-unlock-tool-errors"></a>Erreurs de l’outil de déverrouillage de Data Box Disk


| Message d’erreur/Comportement de l’outil      | Recommandations                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Aucun<br><br>L’outil de déverrouillage de Data Box Disk se bloque.                                                                            | Le composant BitLocker n’est pas installé. Veillez à ce que BitLocker soit installé sur l’ordinateur hôte qui exécute l’outil de déverrouillage de Data Box Disk.                                                                            |
| Le composant .NET Framework actuel n’est pas pris en charge. Les versions prises en charge sont les versions 4.5 et ultérieures.<br><br>L’outil se ferme avec un message.  | .NET 4.5 n’est pas installé. Installez .NET 4.5 ou ultérieur sur l’ordinateur hôte qui exécute l’outil de déverrouillage de Data Box Disk.                                                                            |
| Impossible de déverrouiller ou de vérifier des volumes. Contactez le Support Microsoft.  <br><br>L’outil ne parvient pas à déverrouiller ni à vérifier les lecteurs verrouillés. | L’outil n’a pas pu déverrouiller l’un des lecteurs verrouillés avec la clé d’accès fournie. Contactez le Support Microsoft pour les étapes suivantes.                                                |
| Les volumes suivants sont déverrouillés et vérifiés. <br>Lettre de lecteur du volume : E:<br>Impossible de déverrouiller les volumes avec les clés d’accès suivantes : werwerqomnf, qwerwerqwdfda <br><br>L’outil déverrouille certains lecteurs et répertorie les lettres de lecteur ayant réussi ou échoué.| Réussite partielle. Impossible de déverrouiller certains lecteurs avec la clé d’accès fournie. Contactez le Support Microsoft pour les étapes suivantes. |
| Les volumes verrouillés sont introuvables. Vérifiez que le disque envoyé par Microsoft est connecté correctement et qu’il se trouve à l’état verrouillé.          | L’outil ne parvient pas à trouver de lecteurs verrouillés. Soit les lecteurs sont déjà déverrouillés, soit ils ne sont pas détectés. Assurez-vous que les lecteurs sont connectés et verrouillés.                                                           |
| Erreur irrécupérable : Paramètre non valide<br>Nom de paramètre : invalid_arg<br>UTILISATION :<br>DataBoxDiskUnlock /PassKeys:<liste_clés_accès_séparées_par_points_virgules><br><br>Exemple : DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>Exemple : DataBoxDiskUnlock /SystemCheck<br>Exemple : DataBoxDiskUnlock /Help<br><br>/PassKeys :       Permet d’obtenir cette clé d’accès à partir de la commande Azure Data Box Disk. La clé d’accès déverrouille vos disques.<br>/Help :           Cette option fournit une aide sur l’utilisation et les exemples de cmdlet.<br>/SystemCheck :    Cette option vérifie si votre système répond à la configuration requise pour l’exécution de l’outil.<br><br>Appuyez sur une touche pour quitter. | Paramètre non valide entré. Les seuls paramètres autorisés sont les suivants : /SystemCheck, /PassKey et /Help.                                                                            |

## <a name="data-box-disk-split-copy-tool-errors"></a>Erreurs de l’outil de copie Data Box Disk Split

|Messages d’erreur/avertissements  |Recommandations |
|---------|---------|
|[Info] Récupération du mot de passe bitlocker du volume : m <br>[Erreur] Exception détectée lors de la récupération de la clé bitlocker du volume m :<br> La séquence ne contient aucun élément.|Cette erreur se produit lorsque la destination Data Box Disk est hors ligne. <br> Utilisez l’outil `diskmgmt.msc` sur les disques en ligne.|
|[Erreur] Exception levée : Échec de l'opération WMI :<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=Le format du mot de passe de récupération fourni est invalide. <br>Les mots de passe de récupération BitLocker sont composés de 48 chiffres. <br>Vérifiez le format du mot de passe de récupération, puis essayez à nouveau.|Utilisez l’outil de déverrouillage de Data Box Disk Unlock pour déverrouiller les disques en premier lieu, puis essayez à nouveau la commande. Pour plus d’informations, consultez <li> [Déverrouillez Data Box Disk pour les clients Windows](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Déverrouillez Data Box Disk pour les clients Linux](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Erreur] Exception levée : Un fichier DriveManifest.xml se trouve sur le lecteur cible. <br> Cela signifie que le lecteur cible peut avoir été préparé avec un fichier journal différent. <br>Pour ajouter de nouvelles données sur le même lecteur, utilisez le fichier journal précédent. Pour supprimer les données existantes et réutiliser le lecteur cible pour un nouveau travail d’importation, supprimez le fichier DriveManifest.xml sur le lecteur. Exécutez à nouveau cette commande avec un nouveau fichier journal.| Cette erreur se produit lorsque vous essayez d’utiliser le même jeu de lecteurs pour plusieurs sessions d’importation. <br> Utilisez un jeu de lecteurs uniquement pour une seule session de fractionnement et de copie.|
|[Erreur] Exception levée : CopySessionId importdata-sept-test-1 fait référence à une session de copie précédente et ne peut pas être réutilisé pour une nouvelle session de copie.|Cette erreur est signalée lorsque vous essayez de nommer un nouveau travail avec le même nom que celui d’un travail précédent terminé avec succès.<br> Attribuez un nom unique à votre nouveau travail.|
|[Info] Le nom du fichier ou du répertoire de destination dépasse la limite de longueur NTFS. |Ce message est signalé lorsque le fichier de destination a été renommé en raison de la longueur du chemin d’accès au fichier.<br> Modifiez l’option de disposition dans le fichier `config.json` pour contrôler ce comportement.|
|[Erreur] Exception levée : Séquence d’échappement JSON incorrecte. |Ce message est signalé lorsque le format du fichier config.json n’est pas valide. <br> Validez votre `config.json` avec [JSONlint](https://jsonlint.com/) avant de sauvegarder le fichier.|

## <a name="deployment-issues-for-linux"></a>Problèmes de déploiement pour Linux

Cette section détaille certains des principaux problèmes rencontrés lors du déploiement de Data Box Disk en cas d’utilisation d’un client Linux pour la copie de données.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Problème : Lecteur monté en lecture seule
 
**Cause** 

Cela peut être dû à un système de fichiers qui n’est pas propre. 

Le remontage d'un lecteur en lecture-écriture ne fonctionne pas avec la solution Data Box Disk. Ce scénario n'est pas pris en charge avec les lecteurs déchiffrés par dislocker. Vous avez peut-être réussi à remonter le périphérique à l’aide de la commande suivante : 

    `# mount -o remount, rw / mnt / DataBoxDisk / mountVol1 ß`

Bien que le remontage ait réussi, les données ne seront pas conservées.

**Résolution :**

Si vous voyez l’erreur ci-dessus, vous pouvez essayer l’une des solutions suivantes :

- Installez [`ntfsfix`](https://linux.die.net/man/8/ntfsfix) (disponible dans le package `ntfsprogs`) et exécutez-le sur la partition correspondante.

- Si vous avez accès à un système Windows

    - Chargez le lecteur dans le système Windows.
    - Ouvrez une invite de commandes avec les privilèges d’administrateur. Exécutez `chkdsk` sur le volume.
    - Supprimez le volume et essayez à nouveau en toute sécurité.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Problème : Erreur avec des données non persistantes après copie
 
**Cause** 

Si vous constatez que votre lecteur est vide après démontage (bien que des données aient été copiées dessus), il est possible que vous ayez remonté un lecteur en lecture-écriture alors qu’il a été monté précédemment en lecture seule.

**Résolution :**
 
Si tel est le cas, reportez-vous à la résolution des [Lecteurs montés en lecture seule](#issue-drive-getting-mounted-as-read-only).

Si ce n'est pas le cas, copiez les journaux du dossier contenant l'outil de déverrouillage Data Box Disk et [contactez le support Microsoft](data-box-disk-contact-microsoft-support.md).

## <a name="deployment-issues-for-windows"></a>Problèmes de déploiement pour Windows

Cette section détaille certains des principaux problèmes rencontrés lors du déploiement de Data Box Disk en cas d'utilisation d'un client Windows pour la copie de données.

### <a name="issue-could-not-unlock-drive-from-bitlocker"></a>Problème : Impossible de déverrouiller le lecteur depuis BitLocker
 
**Cause** 

Vous avez utilisé le mot de passe dans la boîte de dialogue BitLocker et essayé de déverrouiller le disque via la boîte de dialogue BitLocker. Cette méthode ne fonctionne pas. 

**Résolution :**

Pour déverrouiller les Data Box Disks, vous devez utiliser l’outil de déverrouillage Data Box Disk et fournir le mot de passe du portail Azure. Pour plus d'informations, consultez [Tutoriel : Déballer, connecter et déverrouiller un disque Azure Data Box](data-box-disk-deploy-set-up.md#connect-to-disks-and-get-the-passkey).
 
### <a name="issue-could-not-unlock-or-verify-some-volumes-contact-microsoft-support"></a>Problème : Impossible de déverrouiller ou de vérifier certains volumes. Contactez le Support Microsoft.
 
**Cause** 

Il se peut que vous voyiez l’erreur suivante dans le journal des erreurs et que vous ne puissiez pas déverrouiller ou vérifier certains volumes.

`Exception System.IO.FileNotFoundException: Could not load file or assembly 'Microsoft.Management.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.`
 
Cela indique qu’il vous manque probablement la version appropriée de Windows PowerShell sur votre client Windows.

**Résolution :**

Vous pouvez installer [Windows PowerShell v 5.0](https://www.microsoft.com/download/details.aspx?id=54616) et réessayer l'opération.
 
Si vous ne parvenez toujours pas à déverrouiller les volumes, copiez les journaux du dossier contenant l'outil de déverrouillage Data Box Disk et [contactez le support Microsoft](data-box-disk-contact-microsoft-support.md).

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [gérer Data Box Disk via le portail Azure](data-box-portal-ui-admin.md).

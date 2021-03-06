---
title: Dépannage – Azure Disk Encryption pour les machines virtuelles IaaS | Microsoft Docs
description: Cet article contient des conseils de dépannage concernant Microsoft Azure Disk Encryption pour les machines virtuelles IaaS Windows et Linux.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 01/08/2018
ms.custom: seodec18
ms.openlocfilehash: 36ecfe8942d263ed84e430b01727743ed2cad00c
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54103163"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Guide de rési=olution des problèmes Azure Disk Encryption

Ce guide s’adresse aux informaticiens professionnels, aux analystes de la sécurité des informations et aux administrateurs de cloud dont les organisations utilisent Azure Disk Encryption. Cet article est destiné à vous aider à résoudre les problèmes liés au chiffrement des disques.

## <a name="troubleshooting-linux-os-disk-encryption"></a>Résolution des problèmes de chiffrement de disque de système d’exploitation Linux

Pour chiffrer le disque du système d’exploitation (SE) Linux, il faut démonter le lecteur du système d’exploitation avant l’exécution du processus de chiffrement de disque complet. Si le service ne peut pas démonter le lecteur, un message d’erreur du type « Échec du démontage après... » apparaît généralement.

Cette erreur peut se produire lors d’une tentative de chiffrement de disque dans un environnement de machines virtuelles cible, quand l’image de ce dernier a été modifiée par rapport à celle de la galerie d’images stock (officielles) prises en charge. Les écarts de l’image prise en charge peuvent interférer avec la capacité de l’extension à démonter le lecteur du système d’exploitation. Les exemples d’écarts peuvent inclure les éléments suivants :
- Images personnalisées qui ne correspondent plus au système de fichiers ou au schéma de partitionnement pris en charge.
- Les applications volumineuses, telles que SAP, MongoDB, Apache Cassandra et Docker, ne sont pas prises en charge quand elles sont installées et exécutées dans le système d’exploitation avant le chiffrement. Azure Disk Encryption ne peut pas arrêter en toute sécurité ces processus, comme cela est nécessaire pour la préparation du lecteur du système d’exploitation pour le chiffrement du disque. S’il existe toujours des processus actifs détenant des handles de fichier ouverts sur le lecteur du système d’exploitation, ce lecteur ne peut pas être démonté, ce qui entraîne l’échec du chiffrement du lecteur du système d’exploitation. 
- Les scripts personnalisés qui s’exécutent au moment de l’activation du chiffrement ou en cas de modification apportée sur la machine virtuelle pendant le processus de chiffrement. Ce conflit peut se produire lorsqu’un modèle Azure Resource Manager définit plusieurs extensions à exécuter simultanément ou lorsqu’une extension de script personnalisé ou une autre action est exécutée en même temps que le chiffrement de disque. L’exécution de ces étapes de manière sérialisée et isolée peut résoudre le problème.
- Comme Security Enhanced Linux (SELinux) n’a pas été désactivé avant l’activation du chiffrement, l’étape de démontage échoue. SELinux peut être réactivé à la fin du chiffrement.
- Le disque du système d’exploitation utilise un schéma de Gestionnaire de volume logique (LVM). Même si une prise en charge limitée des disques de données LVM existe, ce n’est pas le cas pour le disque du système d’exploitation LVM.
- Les configurations requises en matière de capacité de mémoire minimale ne sont pas remplies (7 Go sont recommandés pour le chiffrement de disque de système d’exploitation).
- Les lecteurs de données ont été montés de manière récursive sous le répertoire /mnt/ ou les uns sous les autres (par exemple /mnt/data1, /mnt/data2, /data3 + /data3/data4, etc.).
- D’autres [conditions préalables](azure-security-disk-encryption-prerequisites.md) pour Azure Disk Encryption pour Linux ne sont pas remplies.

## <a name="unable-to-encrypt"></a>Chiffrement impossible

Dans certains cas, le chiffrement de disque Linux semble être bloqué à l’étape « OS disk encryption started » (le chiffrement du disque du système d’exploitation a démarré) et SSH est désactivé. Ce processus de chiffrement peut prendre entre 3 et 16 heures pour se terminer sur une image de galerie de stock. Si des disques de données de plusieurs To sont ajoutés, le processus peut prendre des jours.

La séquence de chiffrement de disque du système d’exploitation Linux démonte le lecteur du système d’exploitation temporairement. Il effectue ensuite un chiffrement bloc par bloc de la totalité du disque du système d’exploitation avant de le remonter dans son état chiffré. Contrairement à Azure Disk Encryption sur Windows, le chiffrement de disque Linux n’autorise pas l’utilisation simultanée de la machine virtuelle pendant le chiffrement. Les caractéristiques de performances de la machine virtuelle peuvent faire la différence de façon significative dans le temps nécessaire pour exécuter le chiffrement. Ces caractéristiques incluent la taille du disque et si le compte de stockage est standard ou premium (SSD).

Pour vérifier l’état du chiffrement, vous pouvez interroger le champ **ProgressMessage** renvoyé de la commande [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus). Pendant le chiffrement du lecteur du système d’exploitation, la machine virtuelle passe en état de maintenance et SSH est également désactivé pour éviter toute interruption du processus en cours. Les rapports de message **EncryptionInProgress** pour la majorité du temps d’exécution du chiffrement. Plusieurs heures plus tard, un message **VMRestartPending** vous invite à redémarrer la machine virtuelle. Par exemple : 


```
PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : EncryptionInProgress
DataVolumesEncrypted       : EncryptionInProgress
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk encryption started

PS > Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName $resourceGroupName -VMName $vmName
OsVolumeEncrypted          : VMRestartPending
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OS disk successfully encrypted, please reboot the VM
```

Une fois que vous êtes invité à redémarrer la machine virtuelle et après le redémarrage de la machine virtuelle, vous devez attendre 2 à 3 minutes pour le redémarrage et l’exécution des étapes finales sur la cible. Le message d’état est modifié lorsque le chiffrement est enfin terminé. Lorsque ce message s’affiche, le lecteur de système d’exploitation chiffré est prêt à être utilisé et la machine virtuelle est de nouveau disponible.

Dans les cas suivants, nous vous recommandons de restaurer la machine virtuelle à la capture instantanée ou à la sauvegarde effectuée immédiatement avant le chiffrement :
   - Si la séquence de redémarrage décrite précédemment ne se produit pas.
   - Si les informations de démarrage, message de progression ou autres indicateurs d’erreur signalent que le chiffrement du système d’exploitation a échoué au milieu de ce processus. Le message peut-être par exemple l’erreur « Échec du démontage » qui est décrite dans ce guide.

Avant d’effectuer une autre tentative, il est recommandé de réévaluer les caractéristiques de la machine virtuelle et de vous assurer que toutes les conditions préalables sont remplies.

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Résolution des problèmes Azure Disk Encryption derrière un pare-feu
Lorsque la connectivité est limitée par un pare-feu, une exigence de proxy ou des paramètres de groupe de sécurité réseau (NSG), cela peut interrompre la capacité de l’extension à effectuer les tâches nécessaires. Cette interruption peut entraîner les messages d’état de type « État de l’extension non disponible sur la machine virtuelle ». Dans les scénarios prévus, le processus de chiffrement échoue. Les sections qui suivent décrivent certains problèmes courants au niveau du pare-feu qui valent la peine d’être examinés.

### <a name="network-security-groups"></a>Groupes de sécurité réseau
Les paramètres de groupe de sécurité réseau appliqués doivent permettre au point de terminaison de remplir les [conditions requises](azure-security-disk-encryption-prerequisites.md#bkmk_GPO) de configuration réseau indiquées pour le chiffrement de disque.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Key Vault derrière un pare-feu
La machine virtuelle doit pouvoir accéder au coffre de clés. Reportez-vous au guide d’accès au coffre de clés derrière un pare-feu géré par l’équipe [Azure Key Vault](../key-vault/key-vault-access-behind-firewall.md). 

### <a name="azure-instance-metadata-service"></a>Service de métadonnées d’instance Azure 
La machine virtuelle doit pouvoir accéder au point de terminaison [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md) qui utilise une adresse IP non routable bien connue (`169.254.169.254`), accessible uniquement à partir de la machine virtuelle.

### <a name="linux-package-management-behind-a-firewall"></a>Gestion des packages Linux derrière un pare-feu

Au moment de l’exécution, Azure Disk Encryption pour Linux s’appuie sur le système de gestion des packages de la distribution cible pour installer les composants prérequis nécessaires avant l’activation du chiffrement. Si les paramètres de pare-feu empêchent la machine virtuelle de télécharger et d’installer ces composants, des échecs sont à prévoir. Les étapes de configuration de ce système de gestion des packages peuvent varier selon la distribution. Sous Red Hat, lorsqu’un proxy est requis, vous devez absolument vous assurer que le gestionnaire d’abonnements et yum sont configurés correctement. Pour plus d’informations, consultez [Guide pratique pour résoudre les problèmes du gestionnaire d’abonnement et yum](https://access.redhat.com/solutions/189533).  


## <a name="troubleshooting-windows-server-2016-server-core"></a>Résolution des problèmes Windows Server 2016 Server Core

Sur Windows Server 2016 Server Core, le composant bdehdcfg n’est pas disponible par défaut. Ce composant est requis par Azure Disk Encryption. Il est utilisé pour séparer le volume système du volume du système d’exploitation, ce qui est effectué une seule fois pendant toute la durée de vie de la machine virtuelle. Ces fichiers binaires ne sont pas nécessaires lors des opérations de chiffrement ultérieures.

Pour contourner ce problème, copiez les quatre fichiers suivants à partir d’une machine virtuelle Windows Server 2016 Data Center vers le même emplacement sur Server Core :

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

   2. Entrez la commande suivante :

   ```
   bdehdcfg.exe -target default
   ```

   3. Cette commande crée une partition de système de 550 Mo. Redémarrez le système.

   4. Utilisez DiskPart pour vérifier les volumes, avant de continuer.  

Par exemple : 

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```
<!-- ## Troubleshooting encryption status

If the expected encryption state does not match what is being reported in the portal, see the following support article:
[Encryption status is displayed incorrectly on the Azure Management Portal](https://support.microsoft.com/en-us/help/4058377/encryption-status-is-displayed-incorrectly-on-the-azure-management-por) --> 

## <a name="next-steps"></a>Étapes suivantes

Ce document vous a fait découvrir certains problèmes couramment rencontrés dans Azure Disk Encryption et en a décrit la résolution. Pour plus d’informations sur ce service et ses fonctionnalités, consultez les articles suivants :

- [Appliquer le chiffrement de disque dans Azure Security Center](../security-center/security-center-apply-disk-encryption.md)
- [Chiffrement des données au repos Azure](azure-security-encryption-atrest.md)

---
title: Supprimer des données personnelles - Proxy d’application Azure Active Directory | Microsoft Docs
description: Supprimez des données personnelles des connecteurs installés sur des appareils pour des proxy d'application Azure Active Directory.
documentationcenter: ''
author: barbkess
manager: daveba
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: cbbe1fb4f145da0b5f5a360d33933655855fb042
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54468248"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Supprimer des données personnelles pour des proxy d’application Azure Active Directory  

Le proxy d'application Azure Active Directory nécessite l’installation de connecteurs sur vos appareils, ce qui signifie qu’il pourrait y avoir des données personnelles sur vos appareils. Cet article explique comment supprimer ces données personnelles afin d’améliorer la confidentialité. 


## <a name="where-is-the-personal-data"></a>Où se trouvent ces données personnelles ?
Le proxy d’application peut écrire des données personnelles dans les types de journaux suivants :

- Journaux d’événements des connecteurs
- Journaux des événements Windows

## <a name="remove-personal-data-from-windows-event-logs"></a>Supprimer des données personnelles de journaux des événements Windows

Pour obtenir des informations sur la configuration de la conservation des données dans les journaux des événements Windows, consultez [Paramètres de journaux des événements](https://technet.microsoft.com/library/cc952132.aspx). Pour en savoir plus sur les journaux des événements Windows, consultez [Utilisation des journaux des événements](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Supprimer des données personnelles de journaux des événements des connecteurs

Pour s’assurer que les journaux du proxy d’application ne contiennent aucune donnée personnelle, vous pouvez :

- Supprimer ou afficher les données si nécessaire, ou
- Désactiver la journalisation

Utiliser les sections suivantes pour supprimer les données personnelles de journaux des événements des connecteurs. Vous devez terminer le processus de suppression sur tous les appareils sur lesquels le connecteur est installé.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Afficher ou exporter des données spécifiques

Pour afficher ou exporter des données spécifiques, recherchez des entrées associées dans chaque journal des événements du connecteur. Les journaux se trouvent dans `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. 

Étant donné que les journaux sont des fichiers texte, vous pouvez utiliser [findstr](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) pour rechercher les entrées de texte associées à un utilisateur.  

Pour rechercher des données personnelles, recherchez UserID (ID utilisateur) dans les fichiers journaux. 

Pour rechercher des données personnelles consignées par une application qui utilise la délégation Kerberos contrainte, recherchez ces composants du type de nom d'utilisateur :

- Nom d’utilisateur principal local
- Partie correspondant au nom d’utilisateur dans le nom d’utilisateur principal
- Partie correspondant au nom d’utilisateur dans le nom d’utilisateur principal local
- Nom de compte du gestionnaire des comptes de sécurité (SAM) local 


### <a name="delete-specific-data"></a>Supprimer des données spécifiques

Pour supprimer des données spécifiques :

1. Redémarrez le service du connecteur de proxy d’application Microsoft Azure AD afin de générer un nouveau fichier journal. Le nouveau fichier journal vous permet de supprimer ou de modifier les anciens fichiers journaux. 
2. Suivez le processus [Afficher ou exporter des données spécifiques](#view-or-export-specific-data), détaillé précédemment, pour trouver les informations à supprimer. Recherchez dans tous les journaux du connecteur.
3. Supprimez les fichiers journaux adéquats ou sélectionnez les champs contenant des données personnelles et supprimez-les. De plus, vous pouvez supprimer tous les anciens fichiers journaux si vous n’en avez plus besoin.

### <a name="turn-off-connector-logs"></a>Désactiver les journaux du connecteur

Pour vous assurer que les journaux du connecteur ne contiennent aucune donnée personnelle, vous pouvez arrêter la génération de journaux. Pour arrêter la génération de journaux du connecteur, supprimez la ligne en surbrillance suivante de `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`. 

![Configuration](./media/application-proxy-remove-personal-data/01.png)


## <a name="next-steps"></a>Étapes suivantes

Pour obtenir une vue d’ensemble du proxy d’application, consultez [Offrir un accès à distance sécurisé aux applications locales](application-proxy.md).


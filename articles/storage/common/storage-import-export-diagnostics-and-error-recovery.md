---
title: Diagnostic et récupération d’erreur pour les travaux Azure Import/Export | Microsoft Docs
description: Découvrez comment activer la journalisation documentée pour les travaux du service Microsoft Azure Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 8a5c25cd5836c3bde630684eff24da7dc8b62fc6
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54855803"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnostic et récupération d’erreur pour les travaux Azure Import/Export
Pour chaque disque traité, le service Azure Import/Export crée un journal d’erreurs dans le compte de stockage associé. Vous pouvez également activer la journalisation documentée en définissant la propriété `LogLevel` sur `Verbose` lors de l’appel des opérations [Put Job](/rest/api/storageimportexport/jobs) ou [Update Job Properties](/rest/api/storageimportexport/jobs).

 Par défaut, les journaux sont écrits dans un conteneur nommé `waimportexport`. Vous pouvez spécifier un autre nom en définissant la propriété `DiagnosticsPath` lors de l’appel des opérations `Put Job` ou `Update Job Properties`. Les journaux sont stockés sous la forme d’objets blob de blocs avec la convention d’affectation de noms suivante : `waies/jobname_driveid_timestamp_logtype.xml`.

 Vous pouvez récupérer l’URI des journaux pour un travail en appelant l’opération [Get Job](/rest/api/storageimportexport/jobs). L’URI pour le journal documenté est retourné dans la propriété `VerboseLogUri` pour chaque disque, alors que l’URI pour le journal d’erreurs est retourné dans la propriété `ErrorLogUri`.

Vous pouvez utiliser les données de journalisation pour identifier les problèmes suivants.

## <a name="drive-errors"></a>Erreurs de disque

Les éléments suivants sont classés comme des erreurs de lecteur :

-   Erreurs d’accès ou de lecture du fichier de manifeste

-   Clés BitLocker incorrectes

-   Erreurs de lecture/écriture sur le disque

## <a name="blob-errors"></a>Erreurs d’objet blob

Les éléments suivants sont classés comme des erreurs d’objet blob :

-   Objet blob ou nom incorrect ou conflictuel

-   Fichiers manquants

-   Objet blob introuvable

-   Fichiers tronqués (les fichiers sur le disque sont plus petits que ceux spécifiés dans le manifeste)

-   Contenu de fichier endommagé (pour les travaux d’importation, détecté par une incompatibilité de la somme de contrôle MD5)

-   Fichiers de propriétés et de métadonnées blob endommagés (détectés par une incompatibilité de la somme de contrôle MD5)

-   Schéma incorrect des fichiers de propriétés et/ou de métadonnées blob

Il peut arriver que certaines parties d’un travail d’importation ou d’exportation n’aboutisse pas, alors que l’ensemble du travail est terminé. Dans ce cas, vous pouvez charger ou télécharger les parties manquantes des données sur le réseau, ou vous pouvez créer un nouveau travail pour transférer les données. Consultez la [référence sur l’outil Azure Import/Export](storage-import-export-tool-how-to-v1.md) pour découvrir comment corriger les données sur le réseau.

## <a name="next-steps"></a>Étapes suivantes

* [Utilisation de l’API REST du service Import/Export](storage-import-export-using-the-rest-api.md)

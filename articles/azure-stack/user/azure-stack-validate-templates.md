---
title: Utiliser l’outil de validation des modèles pour vérifier des modèles pour Azure Stack | Microsoft Docs
description: Vérifier des modèles pour un déploiement sur Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/27/2018
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 4ec617fe49a3b55ba0608b77a8d1226fae025e6c
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246447"
---
# <a name="check-your-templates-for-azure-stack-with-the-template-validation-tool"></a>Vérifier vos modèles pour Azure Stack avec l’outil de validation des modèles

*S’applique à : systèmes intégrés Azure Stack et Kit de développement Azure Stack*

Vous pouvez utiliser l’outil de validation des modèles pour vérifier si vos [modèles](azure-stack-arm-templates.md) Azure Resource Manager sont prêts à être déployés dans Azure Stack. L’outil de validation des modèles fait partie des outils disponibles avec Azure Stack. Téléchargez les outils Azure Stack en utilisant la procédure décrite dans l’article [Télécharger des outils à partir de GitHub](azure-stack-powershell-download.md).

## <a name="overview"></a>Vue d’ensemble

Pour valider un modèle, vous devez d’abord générer un fichier des fonctionnalités du cloud, puis exécuter l’outil de validation. Vous utilisez les modules PowerShell suivants à partir des outils Azure Stack :

- Dans le dossier **CloudCapabilities** :<br>         `AzureRM.CloudCapabilities.psm1` crée un fichier JSON des fonctionnalités du cloud représentant les services et versions présentes dans un cloud Azure Stack.
- Dans le dossier **TemplateValidator** :<br>
`AzureRM.TemplateValidator.psm1` utilise un fichier JSON des fonctionnalités du cloud pour tester des modèles en vue d’un déploiement dans Azure Stack.

## <a name="build-the-cloud-capabilities-file"></a>Générer le fichier des fonctionnalités du cloud

Avant d’utiliser le validateur de modèle, exécutez le module PowerShell **AzureRM.CloudCapabilities** pour générer un fichier JSON.

>[!NOTE]
>Si vous mettez à jour votre système intégré ou que vous ajoutez de nouveaux services ou de nouvelles extensions virtuelles, vous devez exécuter à nouveau ce module.

1. Vérifiez que vous disposez d’une connectivité à Azure Stack. Cette procédure peut être effectuée à partir de l’hôte du kit de développement Azure Stack, ou vous pouvez utiliser un [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) pour vous connecter à partir de votre station de travail.
2. Importez le module PowerShell **AzureRM.CloudCapabilities** :

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ```

3. Utilisez l’applet de commande `Get-CloudCapabilities` pour récupérer des versions de service et créer un fichier JSON de fonctionnalités cloud. Si vous ne spécifiez pas **-OutputPath**, le fichier AzureCloudCapabilities.Json est créé dans le répertoire actif. Utilisez votre emplacement réel :

    ```PowerShell
    Get-AzureRMCloudCapability -Location <your location> -Verbose
    ```

## <a name="validate-templates"></a>Valider des modèles

Utilisez cette procédure pour valider des modèles à l’aide du module PowerShell **AzureRM.TemplateValidator**. Vous pouvez utiliser vos propres modèles, ou valider les [modèles de démarrage rapide Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates).

1. Importez le module PowerShell **AzureRM.TemplateValidator.psm1** :

    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2. Exécutez le validateur de modèle :

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Les avertissements ou erreurs de validation de modèle sont journalisés sur la console PowerShell, ainsi que dans un fichier HTML situé dans le répertoire source. La capture d’écran suivante présente un exemple de rapport de validation :

![Rapport de validation de modèle](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>parameters

Le validateur de modèle prend en charge les paramètres suivants.

| Paramètre | Description | Obligatoire |
| ----- | -----| ----- |
| TemplatePath | Spécifie le chemin pour rechercher des modèles Azure Resource Manager de manière récursive | Oui | 
| TemplatePattern | Spécifie le nom des fichiers de modèle à faire correspondre | Non  |
| CapabilitiesPath | Spécifie le chemin du fichier JSON des fonctionnalités du cloud | Oui | 
| IncludeComputeCapabilities | Inclut l’évaluation de ressources IaaS telles que des tailles de machine virtuelle et des extensions de machine virtuelle | Non  |
| IncludeStorageCapabilities | Inclut l’évaluation de ressources de stockage comme des types de références (SKU) | Non  |
| Rapport | Spécifie le nom du rapport HTML généré | Non  |
| Détaillé | Journalise les erreurs et les avertissements dans la console | Non |

### <a name="examples"></a>Exemples

Cet exemple valide tous les [modèles de démarrage rapide Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates) téléchargés dans le stockage local. L’exemple valide également les tailles et extensions de machine virtuelle par rapport aux fonctionnalités du Kit de développement Azure Stack :

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json `
-TemplatePattern MyStandardTemplateName.json `
-IncludeComputeCapabilities `
-Report TemplateReport.html
```

## <a name="next-steps"></a>Étapes suivantes

- [Déployer des modèles sur Azure Stack](azure-stack-arm-templates.md)
- [Développer des modèles pour Azure Stack](azure-stack-develop-templates.md)

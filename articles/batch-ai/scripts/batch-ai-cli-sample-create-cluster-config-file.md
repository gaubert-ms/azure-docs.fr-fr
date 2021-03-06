---
title: Exemple de script Azure CLI – Créer un cluster Batch AI avec un fichier config | Microsoft Docs
description: Exemple de script Azure CLI – Créez un cluster Batch AI en spécifiant des paramètres de configuration dans un fichier JSON.
services: batch-ai
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch-ai
ms.devlang: azurecli
ms.topic: sample
ms.tgt-pltfrm: multiple
ms.workload: na
ms.date: 08/16/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 41a3a801214ff00c01397034e26fde6946ab97f0
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407811"
---
# <a name="cli-example-create-a-batch-ai-cluster-using-a-cluster-configuration-file"></a>Exemple CLI : Créer un cluster Batch AI à l’aide d’un fichier de configuration de cluster

[!INCLUDE [batch-ai-retiring](../../../includes/batch-ai-retiring.md)]

Ce script montre comment utiliser un fichier de configuration JSON pour spécifier les paramètres d’un cluster Batch AI. Utilisez ces paramètres au lieu des paramètres de ligne de commande `az batchai cluster create` correspondants. Un fichier de configuration est utile pour monter plusieurs systèmes de fichiers sur les nœuds de cluster ou utiliser une configuration identique dans plusieurs clusters.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si vous décidez d’installer et d’utiliser l’interface CLI localement, vous devez exécuter Azure CLI version 2.0.38 ou une version ultérieure pour poursuivre la procédure décrite dans cet article. Exécutez `az --version` pour déterminer la version. Si vous devez installer ou mettre à niveau, consultez [Installation d’Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../cli_scripts/batch-ai/create-cluster/create-cluster-config-file.sh "Create Batch AI cluster - configuration file")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement

Exécutez les commandes suivantes pour supprimer les groupes de ressources et toutes les ressources associées.

```azurecli-interactive
# Remove resource group for the cluster.
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Crée un compte de stockage. |
| [az storage share create](/cli/azure/storage/share#az-storage-share-create) | Crée un partage de fichiers dans un compte de stockage. |
| [az batchai workspace create](/cli/azure/batchai/workspace#az-batchai-workspace-create) | Crée un espace de travail Batch AI. |
| [az batchai cluster create](/cli/azure/batchai/cluster#az-batchai-cluster-create) | Crée un cluster Batch AI. |
| [az batchai cluster show](/cli/azure/batchai/cluster#az-batchai-cluster-show) | Affiche des informations sur un cluster Batch AI. |
| [az group delete](/cli/azure/group#az-group-delete) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](https://docs.microsoft.com/cli/azure).

---
title: Tutoriel - Déployer une application Service Fabric Mesh | Microsoft Docs
description: Découvrez comment utiliser Visual Studio pour publier une application Azure Service Fabric Mesh composée d’un site web ASP.NET Core communiquant avec un service web backend.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: e1f2991b2e006c97087c6288d3ed3c20d2927e8c
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413479"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-application"></a>Didacticiel : Déployer une application Service Fabric Mesh

Ce tutoriel est la troisième partie d’une série et vous montre comment publier une application web Azure Service Fabric Mesh directement depuis Visual Studio.

Ce didacticiel vous montre comment effectuer les opérations suivantes :
> [!div class="checklist"]
> * Publier l’application sur Azure à l’aide de Visual Studio
> * Vérifier l’état du déploiement de l’application
> * Voir toutes les applications actuellement déployées dans votre abonnement

Cette série de tutoriels vous montre comment effectuer les opérations suivantes :
> [!div class="checklist"]
> * [Créer une application Service Fabric Mesh dans Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Déboguer une application Service Fabric Mesh qui s’exécute dans votre cluster de développement local](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * Déployer une application Service Fabric Mesh
> * [Mettre à niveau une application Service Fabric Mesh](service-fabric-mesh-tutorial-upgrade.md)
> * [Nettoyer des ressources Service Fabric Mesh](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prérequis

Avant de commencer ce tutoriel :

* Si vous n’avez pas d’abonnement Azure, vous pouvez créer un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

* Assurez-vous que vous avez [configuré votre environnement de développement](service-fabric-mesh-howto-setup-developer-environment-sdk.md) qui inclut l’installation du runtime Service Fabric, du Kit de développement logiciel (SDK), de Docker et de Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>Télécharger l’exemple d’application de tâches

Si vous n’avez pas généré l’exemple d’application de tâches dans la [deuxième partie de cette série de tutoriels](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md), vous pouvez le télécharger. Dans une fenêtre Commande, exécutez la commande suivante pour cloner le référentiel de l’exemple d’application sur votre ordinateur local.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

L’application se trouve dans le répertoire `src\todolistapp`.

## <a name="publish-to-azure"></a>Publication dans Azure

Pour publier votre projet Service Fabric Mesh sur Azure, cliquez avec le bouton droit sur **todolistapp** dans Visual Studio, puis sélectionnez **Publier**.

Ensuite, une boîte de dialogue **Publier une application Service Fabric** s’affiche.

![Boîte de dialogue de publication d’une application Service Fabric MeshVisual studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Sélectionnez votre compte et votre abonnement Azure. Choisissez un **Emplacement**. Dans cet article, nous utilisons la région **USA Est**.

Sous **Groupe de ressources**, sélectionnez **\<Créer un nouveau groupe de ressources...>**. Une boîte de dialogue s’affiche, dans laquelle vous allez créer un nouveau groupe de ressources. Cet article utilise l’emplacement **USA Est** et nomme le groupe **sfmeshTutorial1RG** (si plusieurs personnes utilisent le même abonnement dans votre organisation, choisissez un nom de groupe unique).  Appuyez sur **Créer** pour créer le groupe de ressources et revenir à la boîte de dialogue Publier.

![Boîte de dialogue du nouveau groupe de ressources Service Fabric Mesh Visual studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

Revenez à la boîte de dialogue **Publier une application Service Fabric** sous **Azure Container Registry**, sélectionnez **\<Créer un registre de conteneurs...>**. Dans la boîte de dialogue **Créer un registre de conteneurs**, utilisez un nom unique pour le **nom du registre de conteneurs**. Spécifiez un **emplacement** (ce tutoriel utilise l’emplacement **USA Est**). Sélectionnez dans la liste déroulante le **groupe de ressources** que vous avez créé à l’étape précédente, par exemple, **sfmeshTutorial1RG**. Affectez à **Référence (SKU)** la valeur **De base**, puis appuyez sur **Créer** pour créer le registre de conteneurs Azure privé et retourner à la boîte de dialogue de publication.

![Boîte de dialogue Visual Studio de nouveau registre de conteneurs Service Fabric Mesh](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Si vous recevez une erreur indiquant que le fournisseur de ressources n’a pas été inscrit pour votre abonnement, vous pouvez l’inscrire. Tout d’abord, vérifiez si le fournisseur de ressources est disponible pour votre abonnement :

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Si le fournisseur de registres de conteneurs (`Microsoft.ContainerRegistry`) est disponible, enregistrez-le à partir de Powershell :

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

Dans la boîte de dialogue de publication, appuyez sur le bouton **Publier** pour déployer votre application Service Fabric dans Azure.

Lorsque vous publiez sur Azure pour la première fois, l’image docker est envoyée (push) à Azure Container Registry (ACR), ce qui prend un certain temps selon la taille de l’image. Les publications ultérieures du même projet seront plus rapides. Vous pouvez surveiller la progression du processus de déploiement en sélectionnant le volet **Service Fabric Tools** dans la fenêtre de **sortie** de Visual Studio. Une fois le déploiement terminé, la sortie **Service Fabric Tools** affiche l’adresse IP et le port de votre application sous la forme d’une URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Ouvrez un navigateur web et accédez à l’URL pour voir le site web en cours d’exécution dans Azure.

## <a name="set-up-service-fabric-mesh-cli"></a>Configurer l’interface de ligne de commande Service Fabric Mesh

Vous pouvez utiliser le service Azure Cloud Shell ou une installation locale de l’interface Azure CLI pour les étapes restantes. Installez le module d’extension CLI de Service Fabric mesh en suivant les [instructions](service-fabric-mesh-howto-setup-cli.md) ci-après.

## <a name="check-application-deployment-status"></a>Vérifier l’état du déploiement de l’application

À ce stade, votre application a été déployée. Vous pouvez vérifier son état à l’aide de la commande `app show`. 

Le nom de l’application pour l’application du tutoriel est `todolistapp`. Collecter les informations relatives à l’application à l’aide de la commande suivante :

```azurecli-interactive
az mesh app show --resource-group $rg --name todolistapp
```

## <a name="get-the-ip-address-of-your-deployment"></a>Obtenir l’adresse IP de votre déploiement

Si vous souhaitez obtenir l’adresse IP de votre application, utilisez la commande suivante :
  
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --name todolistappGateway
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Voir toutes les applications actuellement déployées dans votre abonnement

Vous pouvez utiliser la commande « app list » pour obtenir la liste des applications que vous avez déployées sur votre abonnement.

```azurecli-interactive
az mesh app list --output table
```

## <a name="next-steps"></a>Étapes suivantes

Dans cette partie du tutoriel, vous avez appris à :
> [!div class="checklist"]
> * Publiez l’application dans Azure.
> * Vérifier l’état du déploiement de l’application
> * Voir toutes les applications actuellement déployées dans votre abonnement

Passez au tutoriel suivant :
> [!div class="nextstepaction"]
> [Mettre à niveau une application Service Fabric Mesh](service-fabric-mesh-tutorial-upgrade.md)

[azure-cli-install]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest

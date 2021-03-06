---
titre : Reformer un nouveau service web Machine Learning Studio avec PowerShell - titleSuffix : Description d'Azure Machine Learning Studio : Apprenez à reformer un modèle par programme et à mettre à jour le service web pour utiliser le modèle reformé dans Azure Machine Learning à l’aide des applets de commande PowerShell de gestion Machine Learning.
services: machine-learning ms.service: machine-learning ms.component: studio ms.topic: article

author: ericlicoding ms.author: amlstudiodocs ms.custom: seodec18 ms.date: 28/03/2017
---
# <a name="retrain-a-new-resource-manager-based-studio-web-service-using-powershell"></a>Reformer un nouveau service web Studio basé sur Resource Manager avec Powershell
Quand vous reformez un nouveau service web, vous mettez à jour la définition de service web prédictif pour référencer le nouveau modèle formé.

## <a name="prerequisites"></a>Prérequis
Vous devez configurer une expérience de formation et une expérimentation prédictive comme indiqué dans [Reformer des modèles Machine Learning par programme](retrain-models-programmatically.md).

> [!IMPORTANT]
> L’expérience prédictive doit être déployée comme un service web Machine Learning basé sur Azure Resource Manager (nouveau).
> Pour déployer un nouveau service web, vous devez disposer d’autorisations suffisantes dans l’abonnement dans lequel déployer le service web. Pour en savoir plus, consultez [Gérer un service web à l’aide du portail des services web Azure Machine Learning](manage-new-webservice.md).

Pour plus d’informations sur le déploiement de services web, consultez [Déployer un service web Azure Machine Learning](publish-a-machine-learning-web-service.md).

Ce processus requiert l’installation des applets de commande Azure Machine Learning. Pour obtenir des informations sur l’installation des applets de commande Machine Learning, consultez la référence [Azure Machine Learning Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/) (Applets de commande Azure Machine Learning) sur MSDN.

Copiez les informations suivantes à partir de la sortie de reformation :

* BaseLocation
* RelativeLocation

Voici les étapes à suivre :

1. Connectez-vous à votre compte Azure Resource Manager.
2. Obtenir la définition du service web
3. Exportez la définition du service web au format JSON
4. Mettez à jour la référence sur l’objet blob ilearner dans le JSON.
5. Importez le JSON dans une définition du service web
6. Mettre à jour le service web avec la nouvelle définition du service web

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Connectez-vous à votre compte Azure Resource Manager
Vous devez tout d’abord vous connecter à votre compte Azure à partir de l’environnement PowerShell à l’aide de l’applet de commande [Add-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount).

## <a name="get-the-web-service-definition"></a>Obtenir la définition du service web
Ensuite, obtenez le service web en appelant l’applet de commande [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) . La définition du service web est une représentation interne du modèle formé du service web, qui n’est pas directement modifiable. Vérifiez que vous récupérez la définition du service web pour votre expérience prédictive et non pour votre expérience de formation.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Pour déterminer le nom du groupe de ressources d’un service web existant, exécutez l’applet de commande Get-AzureRmMlWebService sans paramètres pour afficher les services web dans votre abonnement. Recherchez le service web et examinez son ID de service web. Le nom du groupe de ressources est le quatrième élément de l’ID, juste après l’élément *resourceGroups* . Dans l’exemple suivant, le nom du groupe de ressources est Default-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Pour déterminer le nom du groupe de ressources d’un service web existant, vous pouvez également vous connecter au portail des services web Microsoft Azure Machine Learning. Sélectionnez le service web. Le nom de groupe de ressources est le cinquième élément de l’URL du service web, juste après l’élément *resourceGroups* . Dans l’exemple suivant, le nom du groupe de ressources est Default-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exportez la définition du service web au format JSON
Pour modifier la définition du modèle formé de manière à utiliser le modèle nouvellement formé, vous devez d’abord utiliser l’applet de commande [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) pour l’exporter vers un fichier au format JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Mettez à jour la référence sur l’objet blob ilearner dans le JSON.
Dans les ressources, recherchez le [modèle formé], mettez à jour la valeur *uri* dans le nœud *locationInfo* avec l’URI de l’objet blob ilearner. L’URI est générée en combinant les valeurs *BaseLocation* et *RelativeLocation* de la sortie de l’appel de reformation BES. Cela met à jour le chemin d’accès pour référencer le nouveau modèle formé.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Importez le JSON dans une définition du service web
Vous devez utiliser l’applet de commande [Import-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) pour convertir le fichier JSON modifié en une définition du service web que vous pouvez utiliser pour mettre à jour la définition de service web.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Mettre à jour le service web avec la nouvelle définition du service web
Enfin, utilisez l’applet de commande [Update-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) pour mettre à jour la définition de service web.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Résumé
À l’aide des applets de commande PowerShell Machine Learning, vous pouvez mettre à jour le modèle formé d’un service web prédictif en permettant des scénarios de type :

* Nouvel apprentissage périodique d’un modèle avec de nouvelles données.
* Distribution d’un modèle auprès des clients dans le but de leur permettre d’effectuer à nouveau l’apprentissage du modèle avec leurs propres données.


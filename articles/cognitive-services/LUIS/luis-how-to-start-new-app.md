---
title: Créer une application
titleSuffix: Language Understanding - Azure Cognitive Services
description: Créez et gérez vos applications sur la page web de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: ddfee80c67c22c7c6016ed87dc17925c91637d21
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53713996"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Créer une application LUIS dans le portail LUIS
Il existe deux façons de créer une application LUIS. Vous pouvez créer une application LUIS dans le portail [LUIS](https://www.luis.ai) ou par le biais des [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) de création LUIS.

## <a name="using-the-luis-portal"></a>Utilisation du portail LUIS
Vous pouvez créer une application dans le portail LUIS de plusieurs manières :

* Démarrez avec une application vide et créer des intentions, des énoncés et des entités.
* Démarrez avec une application vide et ajoutez un [domaine prédéfini](luis-how-to-use-prebuilt-domains.md).
* Importez une application LUIS à partir d’un fichier JSON qui contient déjà des intentions, des énoncés et des entités.

## <a name="using-the-authoring-apis"></a>Utilisation des API de création
Vous pouvez créer une application avec les API de création de deux manières :

* [Démarrez](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) avec une application vide et créer des intentions, des énoncés et des entités.
* [Démarrez](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) avec un domaine prédéfini.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Créer une application dans LUIS

1. Sur la page **Mes applications**, sélectionnez **Créer une application**.

    ![Liste des applications LUIS](./media/luis-create-new-app/apps-list.png)


2. Dans la boîte de dialogue, nommez votre application « TravelAgent ».

    ![Boîte de dialogue Créer une application](./media/luis-create-new-app/create-app.png)

3. Choisissez la culture de votre application (pour l’application TravelAgent, sélectionnez Anglais), puis sélectionnez **Terminé**. 

    > [!NOTE]
    > La culture ne peut pas être modifiée une fois que l’application est créée. 


## <a name="next-steps"></a>Étapes suivantes

Votre première tâche dans l’application consiste à [ajouter des intentions](luis-how-to-add-intents.md).
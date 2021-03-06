---
title: Guide pratique pour utiliser les entrées de remplacement avec Conversation Learner – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Découvrez comment utiliser les entrées de remplacement avec Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: b6ea1ee4eb8b55d2da4069ef19a268ec68f49cb4
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796490"
---
# <a name="how-to-use-alternative-inputs"></a>Guide pratique pour utiliser les entrées de remplacement

Ce didacticiel montre comment utiliser le champ Entrées alternatives pour des énoncés de l’utilisateur dans l’interface d’apprentissage.

## <a name="video"></a>Vidéo

[![Aperçu du didacticiel Entrées alternatives](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Configuration requise
Ce tutoriel nécessite que le bot tutoriel général soit en cours d’exécution.

    npm run tutorial-general

## <a name="details"></a>Détails
Les entrées alternatives sont des énoncés de remplacement sémantiquement équivalents que l’utilisateur peut avoir dits à un moment donné dans une boîte de dialogue d’apprentissage. Elles permettent de spécifier de manière plus compacte les variantes des énoncés, sans avoir à traiter chacune d’entre elles dans des boîtes de dialogue d’apprentissage distinctes.

## <a name="steps"></a>Étapes

### <a name="create-the-model"></a>Créer le modèle

1. Dans l'interface utilisateur web, cliquez sur « Nouveau modèle ».
2. Dans le champ « Nom », tapez « AlternativeInputs » et appuyez sur Entrée.
3. Cliquez sur le bouton « Créer ».

### <a name="entity-creation"></a>Création d'entités

1. Dans le panneau gauche, cliquez sur « Entités », puis sur le bouton « Nouvelle entité ».
2. Sous « Type d'entité », sélectionnez « Apprentissage personnalisé ».
3. Sous « Nom de l’entité », entrez « Ville ».
4. Cliquez sur le bouton « Créer ».

Créons à présent trois actions.

### <a name="create-the-first-action"></a>Créer la première action

1. Dans le panneau gauche, cliquez sur « Actions », puis sur le bouton « Nouvelle action ».
2. Dans le champ « Réponse du bot... », tapez « Quelle ville ? ».
3. Dans le champ « Entité attendue dans la réponse de l’utilisateur », tapez « Ville ».
4. Dans le champ « Entités disqualifiantes », tapez « ville ».
5. Cliquez sur le bouton « Créer ».

### <a name="create-the-second-action"></a>Créer la deuxième action

1. Dans le panneau gauche, cliquez sur « Actions », puis sur le bouton « Nouvelle action ».
2. Dans le champ « Réponse du bot... », tapez « Il fait probablement beau à $city ».
3. Cliquez sur le bouton « Créer ».

### <a name="create-the-third-action"></a>Créer la troisième action

1. Dans le panneau gauche, cliquez sur « Actions », puis sur le bouton « Nouvelle action ».
2. Dans le champ « Réponse du bot... », tapez « Essayez de demander la météo ».
3. Dans le champ « Entités disqualifiantes », tapez « ville ».
4. Cliquez sur le bouton « Créer ».

Vous avez maintenant trois actions.

### <a name="train-the-model"></a>Former le modèle

1. Dans le panneau gauche, cliquez sur « Boîtes de dialogue d'apprentissage », puis sur le bouton « Nouveau dialogue d'apprentissage ».
2. Dans le panneau de conversation, sous « Entrez votre message... », tapez « Quel temps fait-il ? ».
3. Cliquez sur le bouton « Noter les actions ».
4. Sélectionnez la réponse, « Quelle ville ? ».
5. Dans le panneau de conversation, sous « Entrez votre message... », tapez « Denver ».
6. Cliquez sur le bouton « Noter les actions ».
7. Sélectionnez la réponse, « La météo à Denver est probablement ensoleillée ».
8. Cliquez sur le bouton « Enregistrer ».

Poursuivons la formation du modèle en créant une autre boîte de dialogue d’apprentissage.

### <a name="second-model-train-dialog"></a>Deuxième boîte de dialogue d’apprentissage du modèle

1. Dans le panneau gauche, cliquez sur « Boîtes de dialogue d'apprentissage », puis sur le bouton « Nouveau dialogue d'apprentissage ».
2. Dans le panneau de conversation, sous « Entrez votre message... », tapez « Que pouvez-vous faire ? ».
3. Cliquez sur le bouton « Noter les actions ».
4. Sélectionnez la réponse, « Essayez de demander la météo ».
5. Dans le panneau de conversation, sous « Entrez votre message... », tapez « Quel temps fait-il à Seattle ? ».
6. Cliquez sur « Seattle », puis sur « ville » dans la liste d’entités.
7. Cliquez sur le bouton « Noter les actions ».
8. Sélectionnez la réponse, « La météo à Seattle est probablement ensoleillée ».
9. Cliquez sur le bouton « Enregistrer ».

### <a name="third-model-train-dialog-using-alternative-input"></a>Troisième boîte de dialogue d’apprentissage du modèle utilisant une entrée alternative

1. Dans le panneau gauche, cliquez sur « Boîtes de dialogue d'apprentissage », puis sur le bouton « Nouveau dialogue d'apprentissage ».
2. Dans le panneau de conversation, sous « Entrez votre message... », tapez « Aide ».
3. Cliquez sur le bouton « Noter les actions ».
    - Le modèle n’est pas certain de la meilleure option, donc il choisira le centile le plus élevé par défaut.
4. Cliquez sur le bouton de « Abandonner l’apprentissage », puis sur le bouton « Confirmer ».

![](../media/tutorial8_closescores.png)

Améliorons les paramètres du système en utilisant des entrées alternatives. Vous pouvez ajouter des entrées alternatives lors de l’apprentissage ou plus tard.

5. Dans le panneau gauche, cliquez sur « Boîtes de dialogue d’apprentissage », puis sélectionnez « Que pouvez-vous faire ? » dans la liste des boîtes de dialogue d’apprentissage.
6. Cliquez sur l’énoncé « Que pouvez-vous faire ? ». dans le panneau de conversation.
7. Dans le champ « Ajouter une entrée alternative... », tapez « aide », puis appuyez sur Entrée.
8. Cliquez sur le bouton « Enregistrer les modifications ».

![](../media/tutorial8_helpalternates.png)

Ajoutons une autre entrée alternative pour gérer Houston.

9. Cliquez sur « Quel temps fait-il à Seattle ? ». dans le panneau de conversation.
10. Dans le champ « Ajouter une entrée alternative... », tapez « prévisions pour Houston », puis appuyez sur Entrée.
    - Le message d’erreur souligne que les entrées alternatives de faits doivent être sémantiquement équivalentes et contenir les mêmes entités que l’énoncé d’origine, pas seulement les mêmes valeurs d’entités. La présence des mêmes entités est obligatoire.
11. Cliquez sur « Houston » et sélectionnez « ville » dans la liste des entités.
12. Dans le champ « Ajouter une entrée alternative... », tapez « prévisions pour Seattle », puis appuyez sur Entrée.
13. Cliquez sur « Seattle » et sélectionnez « ville » dans la liste des entités.
14. Cliquez sur le bouton « Enregistrer les modifications ».
15. Cliquez sur le bouton « Enregistrer la modification ».

### <a name="testing-the-model"></a>Tester le modèle

1. Dans le panneau gauche, cliquez sur « Boîtes de dialogue du journal », puis sur le bouton « Nouvelle boîte de dialogue du journal ».
2. Dans le panneau de conversation, sous « Entrez votre message... », tapez « aidez-moi ».
3. Dans le panneau de conversation, sous « Entrez votre message... », tapez « prévisions pour Denver ».

![](../media/tutorial8_altcities.png)

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Consigner les dialogues](./11-log-dialogs.md)

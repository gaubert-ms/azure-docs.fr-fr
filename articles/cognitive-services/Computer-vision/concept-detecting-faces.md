---
title: Détection des visages - Vision par ordinateur
titleSuffix: Azure Cognitive Services
description: Concepts liés à la fonctionnalité de détection des visages de l’API Vision par ordinateur.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0c6485bff4ad11aab37139cd2aa2d3660bddac0e
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580534"
---
# <a name="face-detection-with-computer-vision"></a>Détection des visages avec Vision par ordinateur

Vision par ordinateur détecte les visages au sein d’une image et génère l’âge, le sexe et le rectangle du visage de chaque visage détecté. Il fournit un sous-ensemble de fonctionnalités proposées par le service [Visage](/azure/cognitive-services/face/). Vous pouvez également utiliser le service Visage pour effectuer une analyse plus détaillée, comme l’identification faciale et la détection de la pose.  

## <a name="face-detection-examples"></a>Exemples de détection des visages

Le premier exemple montre la réponse JSON retournée par Vision par ordinateur pour une image contenant un seul visage.

![Analyse Vision femme toit visage](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

Le deuxième exemple montre la réponse JSON retournée pour une image contenant plusieurs visages.

![Analyse Vision photo de famille visage](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Découvrez les concepts concernant la [détection du contenu spécifique à un domaine](concept-detecting-domain-content.md).
---
title: Description d’images - Vision par ordinateur
titleSuffix: Azure Cognitive Services
description: Concepts liés à la description d’images de l’API Vision par ordinateur.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5b920e4ce8df131b81a9ef6ce2d66c7082d8f5e4
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53583406"
---
# <a name="describe-images-with-human-readable-language"></a>Décrire les images dans le langage humain

Les algorithmes de la Vision par ordinateur analysent le contenu d’une image. L’analyse constitue la description essentielle qui est affichée sous forme de texte lisible par l’homme (phrases complètes). La description résume les éléments identifiés dans l’image. Les algorithmes de la Vision par ordinateur génèrent différentes descriptions selon les éléments visuels identifiés dans l’image. Chacune des descriptions est évaluée, et un indice de confiance est généré. Une liste est ensuite renvoyée, classée du score de confiance plus élevé au plus bas.

## <a name="image-description-example"></a>Exemple de description d’image

La réponse JSON suivante montre la description de l’image qui est retournée par la Vision par ordinateur, sur la base des éléments visuels qu’elle contient.

![Image noir et blanc représentant des immeubles de Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Découvrez les concepts de [balisage d’images](concept-tagging-images.md) et de [catégorisation des images](concept-categorizing-images.md).
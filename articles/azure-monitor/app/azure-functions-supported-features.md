---
title: Azure Application Insights - Fonctionnalités prises en charge par Azure Functions | Microsoft Docs
description: Fonctionnalités Application Insights prises en charge pour Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 9ad0579ff9c25753b1e4816b80948b4d8d1232f7
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54081877"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Fonctionnalités Application Insights prises en charge pour Azure Functions

Azure Functions offre une [intégration prédéfinie](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) à Application Insights, elle est disponible via l’interface ILogger. La liste des fonctionnalités actuellement prises en charge est indiquée ci-dessous. Servez-vous du guide d’Azure Functions pour [Bien démarrer](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Fonctionnalités prises en charge

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Kit SDK .NET d’Application Insights**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Collecte automatique de**        |                 |                   |               
| &bull; Requêtes                     | Oui             | Oui               | 
| &bull; Exceptions                   | Oui             | Oui               | 
| &bull; Dépendances                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Oui               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Oui               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Oui               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Oui               | 
| | | | 
| **Fonctionnalités prises en charge**                |                   |                   |               
| &bull; Pulsation rapide/Métriques temps réel       | Oui             | Oui               | 
| &bull; Échantillonnage                     | Oui             | Oui               | 
| &bull; Pulsations                   |                 | Oui               | 
| | | | 
| **Corrélation**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Oui               | 
| &bull; EventHub                       |                   | Oui               | 
| | | | 
| **Configurable**                      |                   |                   |           
| &bull; Entièrement configurable.<br/>Consultez [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) pour obtenir des instructions.<br/>Consultez [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) pour connaître toutes les options.               |                   | Oui                   | 


## <a name="sampling"></a>échantillonnage

Azure Functions permet l’échantillonnage par défaut dans sa configuration. Pour plus d’informations, consultez [Configurer l’échantillonnage](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

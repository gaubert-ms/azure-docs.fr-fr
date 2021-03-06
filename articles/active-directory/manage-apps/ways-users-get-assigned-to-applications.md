---
title: Guide pratique pour affecter des utilisateurs à des applications | Microsoft Docs
description: Comprendre comment les utilisateurs sont affectés à une application dans votre client
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: cb477ccba648a15c88f2e216b39d4ba1676d43c7
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464610"
---
# <a name="how-to-assign-users-to-applications"></a>Guide pratique pour affecter des utilisateurs à des applications

Cet article vous explique comment les utilisateurs sont affectés à une application dans votre client.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Comment les utilisateurs sont-ils affectés à une application dans Azure AD ?

Pour qu’un utilisateur puisse accéder à une application, il doit tout d’abord être affecté à cette application d’une certaine manière. L’affectation peut être effectuée par un administrateur, un délégué de l’entreprise ou, parfois, par l’utilisateur lui-même. Voici une description des méthodes permettant d’affecter des utilisateurs à des applications :

1.  Un administrateur [affecte un utilisateur](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) directement à l’application

2.  Un administrateur [affecte un groupe](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) dont l’utilisateur est membre à l’application, y compris :

    * Un groupe synchronisé localement

    * Un groupe de sécurité statique créé dans le cloud

    * Un [groupe de sécurité dynamique](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) créé dans le cloud

    * Un groupe Office 365 créé dans le cloud

    * Le groupe [Tous les utilisateurs](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups)

3.  Un administrateur active l[’accès aux applications en libre-service](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) pour autoriser un utilisateur à ajouter une application à l’aide de la fonctionnalité **Ajouter une application** du [volet d’accès aux applications](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction), **sans approbation de l’entreprise**

4.  Un administrateur active l[’accès aux applications en libre-service](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) pour autoriser un utilisateur à ajouter une application à l’aide de la fonctionnalité **Ajouter une application** du [volet d’accès aux applications](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction), mais uniquement **après approbation d’un ensemble sélectionné d’approbateurs d’entreprise**

5.  Un administrateur active la [gestion des groupes en libre-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) pour permettre à un utilisateur de joindre un groupe auquel une application est affectée, **sans approbation d’entreprise**

6.  Un administrateur active la [gestion des groupes en libre-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) pour permettre à un utilisateur de joindre un groupe auquel une application est affectée, mais uniquement **après approbation d’un ensemble sélectionné d’approbateurs d’entreprise**

7.  Un administrateur affecte une licence à un utilisateur pour une première application, par exemple [Microsoft Office 365](https://products.office.com/)

8.  Un administrateur affecte une licence à un groupe dont l’utilisateur est membre pour une première application, par exemple [Microsoft Office 365](https://products.office.com/)

9.  Un [administrateur donne son consentement à une application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) pour être utilisée par tous les utilisateurs, puis un utilisateur se connecte à l’application

10. Un utilisateur [donne lui-même son consentement à une application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) en se connectant à l’application

## <a name="next-steps"></a>Étapes suivantes
[Gestion des applications avec Azure Active Directory](what-is-application-management.md)

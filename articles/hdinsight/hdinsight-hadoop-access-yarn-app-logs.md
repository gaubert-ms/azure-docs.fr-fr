---
title: Accéder aux journaux des applications Hadoop YARN par programmation - Azure
description: Accéder aux journaux des applications par programmation sur un cluster Hadoop dans HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 7326cf6a1153d5dc1f7e5f910a376a21b05db606
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725438"
---
# <a name="access-apache-hadoop-yarn-application-logs-on-windows-based-hdinsight"></a>Accéder aux journaux des applications Apache Hadoop YARN dans HDInsight Windows
Ce document explique comment accéder aux journaux des applications [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) qui se sont terminées sur un cluster Apache Hadoop Windows dans Azure HDInsight.

> [!IMPORTANT]  
> Les informations présentes dans ce document sont spécifiques aux clusters HDInsight sous Windows. Linux est le seul système d’exploitation utilisé sur HDInsight version 3.4 ou supérieure. Pour plus d’informations, consultez [Suppression de HDInsight sous Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Pour plus d’informations sur l’accès aux journaux YARN sur des clusters HDInsight Linux, consultez [Accéder aux journaux d’applications YARN Apache Hadoop sous Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md).


### <a name="prerequisites"></a>Prérequis
* Un cluster HDInsight Windows  Consultez [Création de clusters Apache Hadoop basés sur Windows dans HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>YARN Timeline Server
Le serveur <a href="https://hadoop.apache.org/docs/r2.4.1/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Apache Hadoop YARN Timeline Server</a> fournit des informations génériques sur les applications terminées ainsi que des informations sur les applications spécifiques au framework, via deux interfaces différentes. Plus précisément :

* Le stockage et la récupération d’informations génériques sur les applications sur les clusters HDInsight sont activés dans les versions 3.1.1.374 ou ultérieures.
* Le composant fournissant des informations sur les applications spécifiques à l'infrastructure n'est pas actuellement disponible sur les clusters HDInsight.

Les informations génériques sur les applications comprennent les données suivantes :

* ID d’application, identificateur unique d’une application
* Utilisateur ayant démarré l’application
* Informations sur les tentatives effectuées afin de terminer l’application
* Conteneurs utilisés par toute tentative d’application donnée

Sur vos clusters HDInsight, ces informations sont stockées par Azure Resource Manager. Les informations sont enregistrées dans le magasin d’historique au sein du stockage par défaut pour le cluster. Vous pouvez récupérer ces données génériques sur les applications terminées via une API REST :

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Applications et journaux YARN
YARN (Yet Another Resource Negotiator) prend en charge plusieurs modèles de programmation en séparant la gestion des ressources de la planification et de l’analyse des applications. YARN utilise les éléments suivant : *ResourceManager* (RM) global, *NodeManagers* (NM) par nœud worker et *ApplicationMasters* (AM) par application. Le maître d'application, propre à chaque application, négocie les ressources nécessaires (processeur, mémoire, disque, réseau) pour exécuter l'application avec le gestionnaire de ressources. Le gestionnaire de ressources fonctionne avec les gestionnaires de nœuds pour octroyer ces ressources sous forme de *conteneurs*. Le maître d'application est chargé de suivre la progression des conteneurs qui lui sont assignés par le gestionnaire de ressources. Selon la nature de l'application, celle-ci peut nécessiter de nombreux conteneurs.

* Chaque application peut comporter plusieurs *tentatives d’application*. 
* Les conteneurs sont accordés à une tentative spécifique d’une application. 
* Un conteneur fournit le contexte d’une unité de travail de base. 
* Le travail réalisé dans le contexte d’un conteneur est effectué sur l’unique nœud de travail auquel le conteneur a été alloué. 

Pour plus d’informations, consultez [Concepts d’Apache Hadoop YARN][YARN-concepts].

Les journaux des applications (et les journaux des conteneurs associés) sont essentiels pour déboguer des applications Hadoop problématiques. La fonctionnalité [Agrégation de journaux][log-aggregation] de YARN fournit une infrastructure adaptée à la collecte, à l’agrégation et au stockage des journaux des applications. La fonctionnalité d’agrégation de journaux rend l’accès aux journaux des applications plus déterministe, car elle regroupe les journaux de tous les conteneurs sur un nœud de travail et les stocke dans un même fichier journal agrégé par nœud de travail sur le système de fichiers lorsqu’une application se termine. Votre application peut utiliser des centaines voire des milliers de conteneurs, mais les journaux de tous les conteneurs exécutés sur un nœud de travail unique sont regroupés dans un fichier unique, ce qui se traduit par l’existence d’un fichier par nœud de travail utilisé par votre application. L’agrégation de journaux est activée par défaut sur les clusters HDInsight (versions 3.0 et ultérieures) et les journaux agrégés figurent dans le conteneur par défaut de votre cluster à l’emplacement suivant :

    wasb:///app-logs/<user>/logs/<applicationId>

où *user* est le nom de l’utilisateur ayant démarré l’application et *applicationId*, l’identificateur unique d’une application telle qu’elle a été assignée par le gestionnaire de ressources YARN.

Les journaux agrégés ne sont pas lisibles directement, car ils sont écrits dans un [TFile][T-file], un [format binaire][binary-format] indexé par conteneur. YARN fournit des outils CLI pour vider ces journaux au format texte brut pour les applications ou les conteneurs qui présentent un intérêt. Vous pouvez afficher ces journaux au format texte brut en exécutant l'une des commandes YARN suivantes directement sur les nœuds de cluster (après avoir établi une connexion via RDP) :

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>Interface utilisateur de ResourceManager YARN
L’IU ResourceManager de YARN s’exécute sur le nœud principal du cluster et est accessible via le tableau de bord du portail Azure :

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Dans le menu de gauche, cliquez sur **Parcourir**, puis sur **Clusters HDInsight**. Cliquez ensuite sur le cluster Windows sur lequel vous souhaitez accéder aux journaux d’application YARN.
3. Cliquez sur **Tableau de bord**dans le menu du haut. Une page appelée **Console de requête HDInsight**s’ouvre dans un nouvel onglet.
4. À partir de la **Console de requête HDInsight**, cliquez sur l’**interface utilisateur YARN**.

[YARN-timeline-server]:https://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/

---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8de46fee4f93fffa669dc96ec5bb84e36842a055
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50164458"
---
#### <a name="to-delete-a-cloud-appliance"></a>Pour supprimer une appliance cloud

1. Connectez-vous au portail Azure.
2. Vous pouvez seulement supprimer un appareil désactivé qui ne contient aucune donnée. Supprimez d’abord les données sur l’appareil ou choisissez de [basculer les données](../articles/storsimple/storsimple-8000-device-failover-cloud-appliance.md) dans des conteneurs de volumes sur un autre appareil. Une fois les données supprimées, vous êtes prêt à désactiver l’appareil.
3. Sur la page de votre service StorSimple Device Manager, cliquez sur **Appareils**, puis sélectionnez l’appareil. Faites un clic droit dessus et sélectionnez **Désactiver**.
4. Une fois le périphérique désactivé, faites un clic droit sur l’appareil et sélectionnez **Supprimer**.

    ![Sélectionnez l’appareil désactivé et cliquez sur Supprimer](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance1.png)

5. Tapez le nom de l’appareil pour confirmer sa suppression. Une fois l’appareil supprimé, la liste des appareils se met à jour.

    ![Confirmer la suppression](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance2.png)

6. Un message vous informe que l’appareil a bien été supprimé.

    ![Notification pour la suppression réussie de l’appareil](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance4.png)

7. La liste des appareils se met à jour pour indiquer l’appareil supprimé.

    ![Liste des appareils mise à jour](./media/storsimple-8000-delete-cloud-appliance/delete-cloud-appliance5.png)

---
ms.assetid: 95CF7F3D-9E3A-40AC-A083-D8A375272181
title: Meilleures pratiques pour l’utilisation du pool de threads
description: Découvrez les meilleures pratiques en matière d’utilisation du pool de threads pour effectuer des tâches de façon asynchrone dans des threads parallèles.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, thread, pool de threads
ms.localizationpriority: medium
ms.openlocfilehash: 692dcae11c80e817d1d400e66e5f90b65df8b249
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161713"
---
# <a name="best-practices-for-using-the-thread-pool"></a>Meilleures pratiques pour l’utilisation du pool de threads

Cette rubrique décrit les meilleures pratiques relatives à l’utilisation du pool de threads.

## <a name="dos"></a>Pratiques conseillées


-   Utilisez le pool de threads pour effectuer des tâches parallèles dans votre application.

-   Utilisez des éléments de travail pour accomplir des tâches étendues sans bloquer le thread d’interface utilisateur.

-   Créez des éléments de travail à courte durée de vie et indépendants. Les éléments de travail s’exécutent de manière asynchrone et peuvent être envoyés au pool dans n’importe quel ordre à partir de la file d’attente.

-   Distribuez les mises à jour au thread d’interface utilisateur à l’aide de l’objet [**Windows.UI.Core.CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher).

-   Utilisez [**ThreadPoolTimer.CreateTimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) plutôt que la fonction **Sleep**.

-   Utilisez le pool de threads au lieu de créer votre propre système de gestion des threads. Le pool de threads s’exécute au niveau du système d’exploitation avec des fonctionnalités avancées. Il est optimisé pour une mise à l’échelle dynamique en fonction des ressources de l’appareil et de l’activité au sein du processus et dans le système.

-   En C++, assurez-vous que les délégués des éléments de travail utilisent le modèle de thread Agile (les délégués C++ sont Agile par défaut).

-   Utilisez des éléments de travail préalloués si vous ne pouvez pas tolérer d’échec d’allocation de ressources au moment de l’utilisation.

## <a name="donts"></a>Choses à faire


-   Ne créez pas de minuteurs périodiques avec une valeur *period* inférieure à &lt;1 milliseconde (y compris 0). Cela amène l’élément de travail à se comporter comme un minuteur à déclenchement unique.

-   N’envoyez pas d’éléments de travail périodiques dont l’exécution est plus longue que la durée spécifiée dans le paramètre *period*.

-   N’essayez pas d’envoyer des mises à jour de l’interface utilisateur (autres que des toasts et des notifications) à partir d’un élément de travail distribué à partir d’une tâche en arrière-plan. Au lieu de cela, utilisez des gestionnaires d’achèvement ou de progression de tâches en arrière-plan, par exemple, [**IBackgroundTaskInstance.Progress**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress).

-   Lorsque vous utilisez des gestionnaires d’éléments de travail qui utilisent le mot clé **async**, sachez que l’élément de travail du pool de threads peut être défini sur l’état Terminé avant que tout le code du gestionnaire ne soit exécuté. Le code qui suit un mot clé **await** dans le gestionnaire peut s’exécuter après que l’élément de travail a été défini sur l’état Terminé.

-   N’essayez pas d’exécuter plusieurs fois un élément de travail préalloué sans le réinitialiser. [Créer un élément de travail périodique](create-a-periodic-work-item.md)

## <a name="related-topics"></a>Rubriques connexes


* [Créer un élément de travail périodique](create-a-periodic-work-item.md)
* [Envoyer un élément de travail au pool de threads](submit-a-work-item-to-the-thread-pool.md)
* [Utiliser un minuteur pour envoyer un élément de travail](use-a-timer-to-submit-a-work-item.md)
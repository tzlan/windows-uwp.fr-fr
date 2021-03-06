---
title: Mode Fenêtre ou Plein écran
description: Les applications Direct3D peuvent s’exécuter dans deux modes, fenêtré ou plein écran.
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords:
- Mode Fenêtre ou Plein écran
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d84bbebfaf19b756e6abc6c592187b6b0ee92200
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618464"
---
# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>Fenêtrées et le mode plein écran


Les applications Direct3D peuvent s’exécuter dans deux modes : fenêtré ou plein écran. En *mode fenêtré*, l’application partage l’espace disponible sur l’écran du bureau avec toutes les applications en cours d’exécution. En *mode plein écran*, la fenêtre dans laquelle s'exécute l'application couvre la totalité du bureau et masque toutes les applications en cours d'exécution (y compris votre environnement de développement). Les jeux s'exécutent généralement par défaut en mode plein écran pour plonger entièrement l’utilisateur dans le jeu en masquant toutes les applications en cours d’exécution.

Les différences de code entre le mode plein écran et le mode fenêtré sont très faibles.

Comme une application qui s'exécute en mode plein écran prend le contrôle sur l’écran, le débogage de l’application requiert un autre moniteur ou l’utilisation d’un débogueur à distance. L’avantage d’une application en mode fenêtré est que vous pouvez parcourir le code dans un débogueur sans avoir besoin de plusieurs moniteurs ou d'un débogueur distant.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Appareils](devices.md)

 

 





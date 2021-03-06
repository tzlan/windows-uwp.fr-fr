---
description: Les animations de transition de contenu vous permettent de modifier le contenu d’une zone de l’écran tout en maintenant le conteneur ou l’arrière-plan constant. Le nouveau contenu apparaît. Si du contenu déjà à l’écran doit être remplacé, il disparaît.
title: Recommandations en matière d’animations de transition de contenu
ms.assetid: 0188FDB4-E183-466f-8A03-EE3FF5C474B1
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ed70a100b4aec7a2c1490cfc48e4d2f6ceac950e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029778"
---
# <a name="content-transition-animations"></a>Animations de transition de contenu



Les animations de transition de contenu vous permettent de modifier le contenu d’une zone de l’écran tout en maintenant le conteneur ou l’arrière-plan constant. Le nouveau contenu apparaît. Si du contenu déjà à l’écran doit être remplacé, il disparaît.

> **API importantes** : [ **classe ContentThemeTransition (XAML)**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées


-   Utilisez une animation d’ouverture lorsque vous devez insérer un ensemble de nouveaux éléments dans un conteneur vide. Par exemple, après le chargement initial d’une application, une partie de son contenu est susceptible de ne pas pouvoir s’afficher immédiatement. Une fois le contenu prêt à l’affichage, utilisez une animation de transition de contenu pour activer son affichage.
-   Utilisez les transitions de contenu pour remplacer un ensemble de contenu par un autre résidant déjà dans le même conteneur sur la zone d’affichage.
-   Lorsque vous intégrez un nouveau contenu, faites glisser ce dernier de bas en haut pour l’insérer dans l’affichage dans le sens contraire du flux de pages général ou du sens de lecture.
-   Introduisez le nouveau contenu de manière logique, par exemple le contenu le plus important en dernier.
-   Si vous avez plusieurs conteneurs dont le contenu doit être actualisé, déclenchez l’intégralité des animations de transition simultanément sans aucune étape ou délai.
-   N’utilisez pas d’animations de transition de contenu quand la page entière est en cours de modification. Dans ce cas, utilisez plutôt les animations de transition entre les pages.
-   N’utilisez pas d’animation de transition de contenu si le contenu est uniquement en cours d’actualisation. Les animations de transition de contenu sont destinées à représenter du mouvement. Pour les actualisations, utilisez les animations en fondu.



## <a name="related-articles"></a>Articles connexes

**Pour les développeurs (XAML)**
* [Vue d’ensemble des animations](./xaml-animation.md)
* [Animation de transitions de contenu](/previous-versions/windows/apps/jj649426(v=win.10))
* [Démarrage rapide : Animation de votre interface utilisateur avec des animations de la bibliothèque](/previous-versions/windows/apps/hh452703(v=win.10))
* [**Classe ContentThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.contentthemetransition)

 

 

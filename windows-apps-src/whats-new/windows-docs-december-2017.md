---
title: Nouveautés apportées dans la documentation Windows en décembre 2017 - Développer des applications UWP
description: De nouvelles fonctionnalités, des vidéos et des conseils aux développeurs ont été ajoutés à la documentation du développeur Windows 10 en décembre 2017.
keywords: nouveautés, mise à jour, fonctionnalités, conseils aux développeurs, Windows 10, décembre
ms.date: 12/14/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d58fe1662c5ba13c2952fbd96414ab201f5ba27
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174383"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>Nouveautés apportées dans la documentation du développeur Windows en décembre 2017

La documentation du développeur Windows est constamment mise à jour avec des informations sur les nouvelles fonctionnalités mises à la disposition des développeurs sur la plateforme Windows. Les présentations de fonctionnalités, les conseils aux développeurs et les exemples qui suivent ont été récemment mis à disposition, après la parution de la mise à jour Fall Creators Update. Ils contiennent des informations nouvelles et mises à jour destinées aux développeurs Windows.

[Installez les outils et le SDK](https://developer.microsoft.com/windows/downloads#_blank) sur Windows 10, et vous pourrez ainsi [créer une application Windows universelle](../get-started/create-uwp-apps.md) ou découvrir comment vous pouvez utiliser votre [code d’application existant sur Windows](../porting/index.md).

## <a name="features"></a>Fonctionnalités

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality : Guide pour les fans

Particulièrement destiné aux passionnés de technologie immergés dans l'univers de la réalité mixte, le [Guide des fans](/windows/mixed-reality/enthusiast-guide/) répond aux principales questions posées sur Windows Mixed Reality. 

Vous trouverez dans ce guide : 
- un forum aux questions préalable à l'achat ; 
- des instructions de vérification de la compatibilité de votre PC ; 
- des instructions de configuration ; 
- des instructions d'utilisation du casque et des contrôleurs ; 
- des instructions de téléchargement et d'utilisation de jeux immersifs, de vidéos à 360°, d'applications 2D, de WebVR et de SteamVR ; 
- des instructions pour résoudre les problèmes éventuels, etc.

![Utilisatrice du casque Windows Mixed Reality et des contrôleurs de mouvement](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>Interactions avec le clavier

Concevez et optimisez vos applications UWP pour offrir des fonctionnalités et une expérience accessible aux utilisateurs avancés, avec la mise à jour des [interactions avec le clavier](../design/input/keyboard-interactions.md). Nous avons mis à jour nos recommandations et nos conseils pour prendre en compte les nouvelles améliorations de ces interactions, apportées dans la mise à jour Fall Creators Update.

Pour étendre les fonctionnalités au clavier de vos applications, consultez [Raccourcis clavier](../design/input/keyboard-accelerators.md) et [Personnaliser les interactions clavier](../design/input/focus-navigation.md).

Sur les appareils qui prennent en charge les interactions tactiles, ajoutez des fonctionnalités au clavier en vous inspirant des articles [Répondre à la présence du clavier tactile](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) et [Utiliser l'étendue des entrées pour modifier le clavier tactile](../design/input/use-input-scope-to-change-the-touch-keyboard.md).

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

Le portail Microsoft Collaborate fournit des outils et des services qui permettent de simplifier la collaboration en ingénierie au sein de l'écosystème Microsoft. Il permet de partager des éléments de travail (bogues, demandes de fonctionnalités, etc.) et de diffuser du contenu (builds, documents, spécifications). [En savoir plus](/collaborate/)

![Microsoft Collaborate dans l'Espace partenaires](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>Créer des packages d'application de bureau avec les projets UWP

Visual Studio 2017 version 15.5 a mis à jour le modèle **Projet de création de packages d'application Windows** afin qu'il soit beaucoup plus facile d'y inclure un projet UWP. Il n'est plus nécessaire d'utiliser un projet de création de package JavaScript, puis d'ajuster manuellement le manifeste du package.  

Pour obtenir des conseils sur l'utilisation de ce nouveau modèle pour empaqueter votre application de bureau, consultez [Créer un package d'application à l'aide de Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

Pour obtenir des instructions sur l'ajout d'un projet UWP à votre package, consultez [Étendre votre application de bureau avec des composants UWP modernes](/windows/apps/desktop/modernize/desktop-to-uwp-extend).

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Les extensions d'abonnement sont désormais disponibles pour les développeurs dans le programme Insider du Centre de développement Windows

Tous les développeurs qui ont rejoint le programme Insider du Centre de développement peuvent désormais utiliser les extensions d'abonnement pour vendre des produits numériques dans leurs applications (comme des fonctionnalités d'application ou du contenu numérique) avec une facturation périodique automatique. Pour plus d'informations, consultez [Activer des extensions d'abonnement pour votre application](../monetize/enable-subscription-add-ons-for-your-app.md).

## <a name="developer-guidance"></a>Conseils aux développeurs

### <a name="color"></a>Couleur

Pour une expérience utilisateur optimale, nous avons ajouté de nouvelles recommandations sur l'utilisation des couleurs dans vos applications. Celles-ci comprennent des scénarios d'utilisation d'API ainsi que des instructions générales sur la conception d'une interface utilisateur et son accessibilité. Nous avons également mis à jour la liste des couleurs d'accentuation disponibles sur Xbox. [Consultez ici la mise à jour de l'article consacré aux couleurs.](../design/style/color.md)

![palette de couleurs windows universelle](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>Guides d'accès aux données

Nous avons ajouté un [Guide SQL Server](../data-access/sql-server-databases.md) pour montrer de quelle manière votre application peut accéder directement à une base de données SQL Server. Aucune couche de service n’est requise.

En outre, nous avons complètement remodelé notre [guide SQLite](../data-access/sqlite-databases.md) pour le rendre plus clair, et nous y avons intégré les dernières bonnes pratiques de stockage et d'extraction de données dans une base de données légère sur l'appareil des utilisateurs.

### <a name="forms"></a>Formulaires

Nous avons ajouté un article consacré à la [construction de formulaires dans vos applications](../design/controls-and-patterns/forms.md), les formulaires étant destinés à recueillir et soumettre des données des utilisateurs. Cet article comprend des informations spécifiques sur l'implémentation des formulaires et des recommandations d'ordre général sur le lieu et le moment de leur utilisation.

### <a name="intro-to-app-design"></a>Présentation de la conception d’application

Le guide de conception pour la plateforme Windows universelle (UWP) est une ressource destinée à vous aider à concevoir et générer de belles applications abouties. [Notre nouvelle introduction](../design/basics/design-and-ui-intro.md) offre une vue d'ensemble des fonctionnalités de conception universelle incluses dans chaque application UWP. Elle explique en outre comment utiliser la documentation pour créer des interfaces utilisateur (UI) qui s'adaptent parfaitement à toute une gamme d'appareils.


### <a name="request-ratings-and-reviews"></a>Demander des évaluations et des avis

Nous avons ajouté un article qui explique comment [demander des évaluations et des avis pour votre application](../monetize/request-ratings-and-reviews.md). Vous pouvez afficher une boîte de dialogue d'évaluation et d'avis dans le contexte de votre application, ou vous pouvez ouvrir la page d'évaluation et d'avis de votre application dans le Store.

## <a name="samples"></a>exemples

### <a name="customer-orders"></a>Commandes client

L'exemple de [base de données de commandes client](https://github.com/Microsoft/Windows-appsample-customers-orders-database) a été mis à jour de façon à afficher les bonnes pratiques en matière d'accès aux données, comme l'utilisation du modèle de référentiel et la procédure de connexion à plusieurs sources de données (notamment Sqlite, SQL Azure et un service REST).

## <a name="videos"></a>Vidéos

### <a name="package-a-net-app-in-visual-studio"></a>Créer un package d'application .NET dans Visual Studio

Porter votre application pour poste de travail vers la plateforme Windows universelle (UWP) est plus simple que jamais. [Regardez la vidéo](https://www.youtube.com/watch?v=fJkbYPyd08w) pour savoir comment empaqueter votre application .NET pour la distribution, puis [consultez cette page](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) pour plus d'informations.
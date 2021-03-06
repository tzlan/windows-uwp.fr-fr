---
title: Modèles de projet de jeu DirectX
description: Découvrez les modèles pour la création d’un jeu pour la plateforme Windows universelle (UWP) et DirectX.
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, modèles
ms.localizationpriority: medium
ms.openlocfilehash: 99d68be96858249fdd314857e1fe6d3020ee56e6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159033"
---
# <a name="directx-game-project-templates"></a>Modèles de projet de jeu DirectX



Les modèles DirectX et de plateforme Windows universelle (UWP) vous permettent de créer rapidement un projet en tant que point de départ pour votre jeu.

## <a name="prerequisites"></a>Prérequis


Pour créer le projet, procédez comme suit :

-   [Téléchargez Microsoft Visual Studio 2015](https://visualstudio.microsoft.com/vs/). Visual Studio 2015 comprend des outils de programmation graphique, tels que des outils de débogage. Pour obtenir une vue d’ensemble des fonctionnalités et outils graphiques et de jeux DirectX, voir [Outils Visual Studio pour le développement de jeux DirectX](set-up-visual-studio-for-game-development.md).

## <a name="choosing-a-template"></a>Choix d’un modèle


Visual Studio 2015 comprend trois modèles DirectX et UWP :

-   Application DirectX 11 (Windows universel) : le modèle Application DirectX 11 (Windows universel) crée un projet UWP dont le rendu s’effectue directement dans une fenêtre d’application à l’aide de DirectX 11.
-   Application DirectX 12 (Windows universel) : le modèle Application DirectX 12 (Windows universel) crée un projet UWP dont le rendu s’effectue directement dans une fenêtre d’application à l’aide de DirectX 12.
-   Application DirectX 11 et XAML (Windows universel) : le modèle Application DirectX 11 et XAML (Windows universel) crée un projet UWP dont le rendu s’effectue à l’intérieur d’un contrôle XAML à l’aide de DirectX 11. Ce modèle utilise un [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel), ce qui vous permet d’utiliser des contrôles d’interface utilisateur XAML. L’utilisation du modèle XAML peut donc faciliter l’ajout d’éléments d’interface utilisateur, mais risque également de réduire les performances.

Le modèle que vous choisissez dépend des performances et des technologies que vous souhaitez utiliser.

## <a name="template-structure"></a>Structure du modèle


Les modèles Windows universels DirectX contiennent les fichiers suivants :

-   pch.h et pch.cpp : prise en charge de l’en-tête précompilé.
-   Package.appxmanifest : propriétés du package de déploiement de l’application.
-   \*. pfx : certificats pour l’application.
-   Dépendances externes : liens vers des fichiers externes que le projet utilise.
-   \*Main. h et \* main. cpp-méthodes pour gérer les ressources de l’application, mettre à jour l’état de l’application et restituer le frame.
-   App.h et App.cpp : point d’entrée principal de l’application. Connecte l’application avec l’interpréteur de commandes Windows et gère les événements de cycle de vie de l’application. Ces fichiers s’affichent uniquement dans les modèles d’application DirectX 11 (Windows universelle) et les modèles d’application DirectX 12 (Windows universelle).
-   App.xaml, App.xaml.cpp et App.xaml.h : point d’entrée principal pour l’application. Connecte l’application avec l’interpréteur de commandes Windows et gère les événements de cycle de vie de l’application. Ces fichiers s’affichent uniquement dans le modèle d’application Direct 11 et XAML (Windows universelle).
-   DirectXPage.xaml, DirectXPage.xaml.cpp et DirectXPage.xaml.h : page qui héberge un SwapChainPanel DirectX. Ces fichiers s’affichent uniquement dans le modèle d’application Direct 11 et XAML (Windows universelle).
-   Contenu
    -   Sample3DSceneRenderer.h et Sample3DSceneRenderer.cpp : convertisseur d’échantillon qui instancie un pipeline de rendu de base.
    -   SampleFpsTextRenderer.h et SampleFpsTextRenderer.cpp : rend la valeur actuelle de fréquence d’images dans l’angle inférieur droit de l’écran utilisant Direct2D et DirectWrite. Ces fichiers s’affichent uniquement dans les modèles d’application DirectX 11 (Windows universelle) et d’application DirectX 11 et WAML (Windows universelle).
    -   SamplePixelShader.hlsl : exemple simple de nuanceur de pixels.
    -   SampleVertexShader.hlsl : exemple simple de nuanceur de vertex.
    -   ShaderStructures.h : structures utilisées pour envoyer une date à l’exemple de nuanceur de vertex.
-   Courant
    -   StepTimer.h : classe d’assistance pour le minutage d’animation et de simulation.
    -   DirectXHelper.h : fonctions d’assistance diverses.
    -   DeviceResources.h et Device Resources.cpp : fournit une interface pour une application qui détient DeviceResources pour être avertie de la perte ou de la création de l’appareil.
    -   d3dx12.h : contient la bibliothèque d’utilitaires D3DX12. Ce fichier s’affiche uniquement dans l’application DirectX 12 (Windows universelle).
-   Ressources : images de logo et d’élément SplashScreen utilisées par l’application.

## <a name="next-steps"></a>Étapes suivantes


Maintenant que vous avez un point de départ, ajoutez-le pour créer vos connaissances en développement de jeux et Microsoft Store compétences en développement de jeux.

Si vous portez un jeu existant, voir les rubriques suivantes.

-   [Effectuer un portage d’OpenGL ES 2.0 vers Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Porter de DirectX 9 vers la plateforme Windows universelle (UWP)](porting-your-directx-9-game-to-windows-store.md)

Si vous créez un jeu DirectX, voir les rubriques suivantes.

-   [Créez un jeu UWP simple avec DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Développement de Marble Maze, jeu pour la plateforme Windows universelle en C++ et DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)
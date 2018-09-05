---
author: mtoepke
title: Outils Graphics Diagnostics
description: Découvrez comment obtenir et utiliser les fonctionnalités de diagnostic de graphiques, notamment le débogage graphique, l’analyse des frames graphiques et l’utilisation du processeur graphique (GPU) dans Visual Studio.
ms.assetid: 629ea462-18ed-a333-07e9-cc87ea2dcd93
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, jeux, graphiques, diagnostics, outils, directx
ms.openlocfilehash: 076020d88889a9cc8b417befa2dd54b41d688e5e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.locfileid: "228828"
---
# <a name="graphics-diagnostics-tools"></a>Outils Graphics Diagnostics


\[ Mise à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir la [documentation archivée](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Dans Windows10, les outils de diagnostic de graphiques sont désormais disponibles au sein même de Windows sous la forme d’une fonctionnalité facultative. Pour utiliser les fonctionnalités de diagnostic de graphiques fournies dans le runtime et dans Visual Studio afin de développer des applications ou des jeux DirectX, installez la fonctionnalité facultative Outils graphiques:

1.  Accédez à **Paramètres**, sélectionnez **Système**, sélectionnez **Applications et fonctionnalités**, puis cliquez sur **Gérer les fonctionnalités facultatives**.
2.  Cliquez sur **Ajouter une fonctionnalité**.   
3.  Dans la liste **Fonctionnalités facultatives**, sélectionnez **Outils graphiques**, puis cliquez sur **Installer**.

Les fonctionnalités de diagnostic de graphiques incluent la possibilité de créer des périphériques de débogage Direct3D (par le biais des couches SDK Direct3D) dans le runtime DirectX, ainsi que les fonctions de débogage graphique, d’analyse des frames graphiques et d’utilisation du GPU.

-   Le débogage graphique vous permet de tracer les appels Direct3D effectués par votre application. Vous pouvez ensuite réécouter ces appels, inspecter les paramètres, procéder à des opérations de débogage et à des expérimentations à l’aide de nuanceurs, et visualiser les ressources graphiques pour diagnostiquer les problèmes de rendu. Les journaux peuvent être relevés sur des PC, simulateurs ou appareils Windows, puis relus sur d’autres matériels.
-   L’analyse des frames graphiques dans Visual Studio s’exécute sur un journal de débogage graphique et collecte le minutage de base pour les appels de dessin Direct3D. Puis elle effectue un ensemble d’expérimentations en modifiant divers paramètres de graphiques et génère un tableau présentant les résultats de minutage. Ces données peuvent vous aider à comprendre les problèmes de performances de rendu graphique dans votre application. Vous pouvez examiner les résultats de ces diverses expériences afin de trouver des moyens d’améliorer les performances.
-   La fonctionnalité d’utilisation du GPU dans Visual Studio vous permet de surveiller l’utilisation du GPU en temps réel. Elle recueille et analyse les données de minutage des charges de travail gérées par l’UC et par le GPU afin de déterminer l’emplacement des goulots d’étranglement.

## <a name="related-topics"></a>Rubriques connexes


[Vue d’ensemble de Graphics Diagnostics dans VisualStudio](http://go.microsoft.com/fwlink/p/?LinkID=526382)

 

 




---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Liaison de données et le modèle MVVM
description: Liaison de données est au cœur du modèle de conception de l’architecture de l’interface utilisateur Model-View-ViewModel (MVVM) et permet de couplage faible entre le code de l’interface utilisateur et sans interface utilisateur.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4431218"
---
# <a name="data-binding-and-mvvm"></a>Liaison de données et le modèle MVVM

Model-View-ViewModel (MVVM) est un modèle de conception de l’architecture de l’interface utilisateur pour dissocier le code de l’interface utilisateur et sans interface utilisateur. Grâce au modèle MVVM, vous définissez votre interface utilisateur de manière déclarative dans le code XAML et balisage de liaison de données permet de lier à d’autres couches contenant les données et les commandes. L’infrastructure de liaison de données fournit un couplage faible qui maintient l’interface utilisateur et les données liées synchronisés et dirige l’entrée d’utilisateur pour les commandes appropriées. 

Dans la mesure où elle offre couplage faible, l’utilisation de la liaison de données réduit durs dépendances entre les différents types de code. Cela rend plus facile de modifier les unités de code individuels (méthodes, classes, contrôles, etc.) sans pour autant causer des effets secondaires involontaires dans d’autres unités. Cette dissocier est un exemple de la *séparation des responsabilités*, qui est un concept important dans nombreux modèles de conception. 

## <a name="benefits-of-mvvm"></a>Avantages du modèle MVVM

Dissocier votre code présente de nombreux avantages, notamment:

* Activation d’un style de codage itératif, exploratoire. Modification isolé est moins risqués et plus facile d’expérimenter.
* Simplification des tests unitaires. Unités de code qui sont isolées les unes des autres peuvent être testées individuellement et en dehors des environnements de production.
* Prise en charge de collaboration en équipe. Code découplé qui est conforme aux interfaces bien conçues peut être développée par différentes personnes ou équipes et intégré ultérieurement.
* Amélioration de la facilité de gestion. Résolution de bogues dans le code découplé est moins susceptible de provoquer régressions dans un autre code.

Contrairement au modèle MVVM, une application avec une structure de «code-behind» plus conventionnelle généralement utilise pour afficher uniquement les données de liaison de données et répond à l’entrée utilisateur en gérant directement les événements déclenchés par les contrôles. Les gestionnaires d’événements sont implémentés dans les fichiers code-behind (par exemple, MainPage.xaml.cs) et sont souvent étroitement couplés aux contrôles, en règle générale, contenant le code qui manipule directement de l’interface utilisateur. Cela rend difficile, voire impossible de remplacer un contrôle sans avoir à mettre à jour le code de gestion des événements. Avec cette architecture, les fichiers code-behind cumuler souvent code qui n’est pas directement liée à l’interface utilisateur, par exemple, le code d’accès de base de données, qui finit par en cours dupliqué et modifiés pour être utilisés avec d’autres pages.

## <a name="app-layers"></a>Couches de l’application

Lorsque vous utilisez le modèle MVVM, une application est divisée en les couches suivantes:

* La couche de **modèle** définit les types qui représentent les données de votre entreprise. Ceci inclut tous les éléments requis pour le domaine d’application principaux du modèle et souvent logique d’application. Cette couche est complètement indépendante de l’affichage et les couches de modèle d’affichage et souvent réside partiellement dans le cloud. Étant donné une couche de modèle entièrement implémentée, vous pouvez créer plusieurs clients différentes applications si vous le souhaitez, telles que des applications UWP et web qui fonctionnent avec les mêmes données sous-jacentes.
* La couche de **vue** définit l’interface utilisateur à l’aide du balisage XAML. Le balisage comprend des expressions de liaison de données (par exemple, [x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) qui définissent la connexion entre les composants d’interface utilisateur spécifiques et des membres de modèle d’affichage et modèles différents. Fichiers code-behind sont parfois utilisés dans le cadre de la couche de vue pour contenir du code supplémentaires nécessaire pour personnaliser ou manipuler l’interface utilisateur, ou pour extraire des données à partir des arguments d’événement gestionnaire avant d’appeler une méthode de modèle d’affichage qui effectue le travail. 
* La couche de **modèle d’affichage** fournit des cibles de liaison de données pour l’affichage. Dans de nombreux cas, le modèle d’affichage expose le modèle directement, ou fournit des membres qui encapsulent les membres du modèle spécifique. Le modèle d’affichage peut également définir des membres pour assurer le suivi des données qui s’appliquent à l’interface utilisateur, mais pas le modèle, par exemple, l’ordre d’affichage d’une liste d’éléments. Le modèle d’affichage sert également un point d’intégration avec d’autres services, comme du code d’accès de base de données. Pour les projets simples, vous aurez ne peut-être pas besoin une couche de modèle distinct, mais uniquement un modèle d’affichage qui encapsule toutes les données que vous avez besoin. 

## <a name="basic-and-advanced-mvvm"></a>MVVM de base et avancée

Comme avec n’importe quel modèle de conception, il existe plusieurs façons d’implémenter le modèle MVVM, et bien d’autres techniques est considérés comme faisant partie du modèle MVVM. Pour cette raison, il existe plusieurs infrastructures MVVM tierces différentes prenant en charge les différentes plateformes basées sur XAML, y compris UWP. Toutefois, ces infrastructures incluent généralement plusieurs services pour l’implémentation découplé architecture de la définition exacte de MVVM quelque peu ambigu. 

Bien que les infrastructures MVVM sophistiquées peuvent être très utiles, en particulier pour les projets d’entreprise à l’échelle, il est généralement un coût associé à adopter n’importe quel modèle particulier ou technique, et les avantages ne sont pas toujours claires, en fonction de la mise à l’échelle et la taille de votre projet. Heureusement, vous pouvez adopter uniquement ces techniques qui fournissent un avantage clair et concret et ignorer d’autres jusqu'à ce que vous en avez besoin. 

En particulier, vous pouvez obtenir un grand nombre d’avantages simplement en comprendre et d’appliquer toute la puissance de la liaison de données et la séparation de votre logique de l’application en les couches décrites précédemment. Cela peut être obtenue avec uniquement les fonctionnalités fournies par le Kit de développement Windows et sans l’aide de n’importe quel Framework externe. En particulier, l' [extension de balisage {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) simplifie la liaison de données et requis très performantes que dans les plateformes XAML précédentes, ce qui élimine la nécessité d’une grande partie du code réutilisable précédemment.

Pour obtenir des conseils supplémentaires sur l’utilisation de base, out-of-the-box MVVM, consultez l' [exemple de base de données de commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database) sur GitHub. De nombreux autres [exemples d’applications UWP](https://github.com/Microsoft?q=windows-appsample
) utilisent également une architecture MVVM base, et l' [exemple d’application de trafic](https://github.com/Microsoft/Windows-appsample-trafficapp) inclut à la fois code-behind et les versions MVVM, avec des notes décrivant la [conversion MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Articles associés

### <a name="topics"></a>Rubriques

[Présentation détaillée de la liaison de données](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Extension de balisage {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Exemples

[Exemple de base de données de commandes clients](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Exemple de stock VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemple d’application de trafic](https://github.com/Microsoft/Windows-appsample-trafficapp)  
---
description: Apprenez à développer des applications Windows accessibles qui incluent la navigation au clavier, les paramètres de couleur et de contraste, ainsi que la prise en charge des technologies d’assistance.
ms.assetid: 9311D23A-B340-42F0-BEFE-9261442AF108
title: Développement d’applications Windows 10 inclusives
label: Developing inclusive Windows 10 apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff42cc2ac8ffb965b5f58db081cd86106f4145ef
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029872"
---
# <a name="developing-inclusive-windows-apps"></a>Développement d’applications Windows inclusives  

Cet article explique comment développer des applications Windows accessibles. Plus précisément, il part du principe que vous comprenez comment concevoir la hiérarchie logique de votre application. Apprenez à développer des applications Windows accessibles qui incluent la navigation au clavier, les paramètres de couleur et de contraste, ainsi que la prise en charge des technologies d’assistance.

Si vous ne l’avez pas encore fait, commencez par lire [Conception de logiciels inclusifs](designing-inclusive-software.md).

Voici trois opérations à effectuer pour vérifier que votre application est accessible :

1. Exposer vos éléments d’interface utilisateur pour [un accès par programme](#programmatic-access).
2. Assurez-vous que votre application prend en charge la [navigation au clavier](#keyboard-navigation) pour les personnes qui ne parviennent pas à utiliser de souris ou d’écran tactile.
3. Assurez-vous que votre application prend en charge les paramètres de [couleur et de contraste](#color-and-contrast) accessibles.

## <a name="programmatic-access"></a>Accès par programme  
Un accès par programme est essentiel pour la création d’une accessibilité au sein des applications. Pour ce faire, définissez le nom accessible (obligatoire) et la description accessible (facultative) du contenu et des éléments d’interface utilisateur interactifs de votre application. Cela permet de s’assurer que les contrôles d’interface utilisateur sont exposés aux technologies d’assistance (AT) tels que les lecteurs d’écran (par exemple, le Narrateur) ou les périphériques de sortie alternatifs (tels que les affichages en Braille). Sans accès par programme, les API relatives aux technologies d’assistance ne peuvent pas interpréter correctement les informations. Par conséquent, l’utilisateur ne peut pas utiliser suffisamment les produits ou la technologie d’assistance se trouve contrainte d’utiliser des interfaces de programmation non documentées ou des techniques qui n’ont jamais été conçues pour être utilisées comme interface d’accessibilité. Lorsque les contrôles d’interface utilisateur sont exposés à la technologie d’assistance, cette dernière est en mesure de déterminer quelles actions et options sont disponibles pour l’utilisateur.  

Pour plus d’informations sur la manière de rendre l’interface utilisateur de votre application disponible pour les technologies d’assistance, voir [Exposer les informations d’accessibilité de base](basic-accessibility-information.md).

## <a name="keyboard-navigation"></a>Navigation au clavier  
Pour les utilisateurs aveugles ou ayant des problèmes de mobilité, il est primordial de pouvoir naviguer dans l’interface utilisateur à l’aide d’un clavier. Toutefois, seuls les contrôles d’interface utilisateur exigeant une interaction avec l’utilisateur pour fonctionner doivent recevoir le focus du clavier. Des composants qui ne nécessitent aucune action, telles que les images statiques, n’exigent pas le focus du clavier.  

Il est important de se souvenir que, contrairement à la navigation avec une souris ou par le biais de la fonctionnalité tactile, la navigation au clavier est linéaire. Au moment d’envisager la navigation au clavier, pensez à la manière dont vos utilisateurs interagiront avec votre produit et comment se déroulera la navigation logique. Dans les cultures occidentales, les personnes lisent de gauche à droite et de haut en bas. Par conséquent, une pratique courante consiste à suivre ce modèle pour la navigation au clavier.  

Lors de la conception de la navigation au clavier, examinez votre interface utilisateur et prenez en considération les questions suivantes :
* Comment les contrôles sont-ils disposés ou regroupés dans l’interface utilisateur ?
* Existe-t-il plusieurs groupes de contrôles importants ?
    * Si oui, ces groupes contiennent-ils un autre niveau de groupes ?
*   La navigation dans les contrôles de l’homologue s’effectue-t-elle en utilisant la touche TAB ou par le biais d’une navigation spéciale (via les touches de direction), ou les deux ?

L’objectif est d’aider l’utilisateur à comprendre la façon dont l’interface utilisateur est disposée et à identifier les contrôles exploitables. Si vous trouvez que l’utilisateur se trouve confronté à un trop grand nombre de taquets de tabulation lors de la boucle de navigation, envisagez de regrouper les contrôles. Certains contrôles associés, tel qu’un contrôle hybride, doivent être traités à ce stade précoce. Il est difficile de reconcevoir la navigation au clavier une fois que vous avez commencé à développer votre produit, c’est pourquoi vous devez veiller à la planifier de manière rigoureuse, et ce dès le début !  

Pour en savoir plus sur la navigation au clavier parmi les éléments de l’interface utilisateur, voir [Accessibilité du clavier](keyboard-accessibility.md).  

En outre, le livre électronique [Engineering Software for Accessibility](https://www.microsoft.com/download/details.aspx?id=19262) (Conception de logiciels accessibles) comporte un chapitre sur ce sujet intitulé _Conception de la hiérarchie logique_ .

## <a name="color-and-contrast"></a>Couleur et contraste  
L’une des fonctionnalités d’accessibilité intégrées dans Windows est le mode Contraste élevé, qui améliore le contraste de couleur du texte et des images sur l’écran de l’ordinateur. Pour certaines personnes, l’augmentation du contraste des couleurs permet de réduire la fatigue visuelle et de faciliter la lecture. Lorsque vous vérifiez votre interface utilisateur en mode de contraste élevé, vous souhaitez vous assurer que les contrôles ont été codés de manière cohérente et avec les couleurs système (pas à l’aide de couleurs codées en dur) pour vous assurer qu’ils seront en mesure de voir tous les contrôles de l’écran qu’un utilisateur n’utilisant pas le contraste élevé pourrait voir.  

XAML
```xaml
<Button Background="{ThemeResource ButtonBackgroundThemeBrush}">OK</Button>
```
Pour plus d’informations sur l’utilisation des couleurs système et des ressources, voir [Ressources de thème XAML](../controls-and-patterns/xaml-theme-resources.md).

Tant que vous n’avez pas remplacé les couleurs système, une application UWP prend en charge les thèmes à contraste élevé par défaut. Si un utilisateur a décidé que le système doit utiliser un thème à contraste élevé parmi les outils d’accessibilité ou les paramètres système, l’infrastructure utilise automatiquement des couleurs et des paramètres de style produisant une disposition à contraste élevé et un rendu pour les contrôles et les composants de l’interface utilisateur.   

Pour plus d’informations, voir [Thèmes à contraste élevé](high-contrast-themes.md).  

Si vous avez décidé d’utiliser votre propre thème de couleur au lieu des couleurs système, tenez compte des recommandations suivantes :  

**Coefficient de contraste couleur**  : la section 508 mise à jour du Americans with Disability Act, ainsi que d’autres législations, exigent que le contraste de couleur par défaut entre le texte et son arrière-plan doit être de 5:1. Pour le texte de grande taille (tailles de police de 18 points, ou 14 points et en gras), le contraste requis par défaut est de 3:1.  

**Combinaisons de couleurs**  : environ 7 % des hommes (et moins de 1 % des femmes) souffrent de problèmes de perception des couleurs. Les utilisateurs daltoniens ont des difficultés à distinguer certaines couleurs, il est donc important de ne pas se servir uniquement de la couleur pour communiquer un état ou une idée. Comme pour les images décoratives (par exemple, les icônes ou les arrière-plans), les combinaisons de couleurs doivent être choisies de manière à optimiser la perception de l’image par les utilisateurs daltoniens.  

## <a name="accessibility-checklist"></a>Liste de vérification de l’accessibilité  
Voici une version abrégée de la liste de vérification de l’accessibilité :

1. Définissez le nom accessible (obligatoire) et la description accessible (facultative) du contenu et des éléments d’interface utilisateur interactifs de votre application.
2. Mettez en œuvre l’accessibilité du clavier :
3. Vérifiez visuellement votre interface utilisateur pour vous assurer que le contraste du texte est suffisant, que le rendu des éléments est correct dans les thèmes à contraste élevé et que les couleurs sont utilisées correctement.
4. Exécutez les outils d’accessibilité, traitez les problèmes signalés et vérifiez l’expérience de lecture d’écran. (Voir la rubrique concernant le test de l’accessibilité)
5. Assurez-vous que vos paramètres de manifeste d’application respectent les recommandations en matière d’accessibilité.
6. Déclarez votre application comme accessible dans le Microsoft Store. (Voir la rubrique [Accessibilité dans le Windows Store](accessibility-in-the-store.md))

Pour en savoir plus, consultez la rubrique [Liste de vérification de l’accessibilité](accessibility-checklist.md).

## <a name="related-topics"></a>Rubriques connexes  
* [Conception de logiciels inclusifs](designing-inclusive-software.md)  
* [Conception inclusive](https://www.microsoft.com/design/inclusive/)
* [Pratiques d’accessibilité à éviter](practices-to-avoid.md)
* [Logiciel d’ingénierie pour l’accessibilité](https://www.microsoft.com/download/details.aspx?id=19262)
* [Hub Microsoft Accessibility Developer](https://developer.microsoft.com/windows/accessible-apps)
* [Accessibilité](accessibility.md)

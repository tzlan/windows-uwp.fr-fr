---
description: Révéler focus est un effet visuel qui anime la bordure des éléments susceptibles d’être activés quand l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers.
title: Révéler focus
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 15ddbd46f2e4177b53701259feecd03d8306064b
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218112"
---
# <a name="reveal-focus"></a>Révéler focus

![Image Hero](images/header-reveal-focus.svg)

Révéler focus est un effet visuel pour les [expériences à 3 mètres](../devices/designing-for-tv.md), comme Xbox One et les écrans de télévision. Cet effet anime la bordure des éléments susceptibles d’être activés, comme les boutons, quand l’utilisateur déplace le focus du clavier ou du boîtier de commande sur ces derniers. Il est désactivé par défaut, mais il est facile de l’activer. 

(Pour plus d’informations sur l’effet Révéler, un effet visuel qui met en évidence les éléments interactifs, consulter l’[article sur l’effet Révéler](./reveal.md).)


> **API importantes** : [propriété Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind), [énumération FocusVisualKind](/uwp/api/windows.ui.xaml.focusvisualkind), [propriété Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Fonctionnement
L’effet Révéler focus attire l’attention sur les éléments où se trouve le focus en ajoutant une lumière animée autour de la bordure de l’élément :

![Visuel de l’effet Révéler](images/traveling-focus-fullscreen-light-rf.gif)

Ceci est particulièrement utile dans les scénarios à 3 mètres, où l’utilisateur peut ne pas être totalement concentré sur la totalité de l’écran de télévision. 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/RevealFocus">ouvrir l’application et voir l’effet Révéler focus en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Comment l’utiliser

Par défaut, Révéler focus est désactivé. Pour l’activer :
1. Dans le constructeur de votre application, appelez la propriété [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) et vérifiez si la famille d’appareils en cours est `Windows.Xbox`.
2. Si la famille d’appareils est `Windows.Xbox`, définissez la propriété [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) sur `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Une fois la propriété **FocusVisualKind** définie, le système applique automatiquement l’effet Révéler focus à tous les contrôles dont la propriété [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) est définie sur **True** (la valeur par défaut pour la plupart des contrôles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>Pourquoi Révéler focus n’est pas activé par défaut ? 
Comme vous pouvez le constater, il est relativement facile d’activer Révéler focus quand l’application détecte qu’elle est exécutée sur une Xbox. Pourquoi le système ne l’active pas simplement pour vous ? La raison en est que Révéler focus augmente la taille du visuel du focus, ce qui peut entraîner des problèmes avec la disposition de votre interface utilisateur. Dans certains cas, vous voudrez personnaliser l’effet Révéler focus afin de l’optimiser pour votre application.

## <a name="customizing-reveal-focus"></a>Personnaliser l’effet Révéler focus

Vous pouvez personnaliser l’effet Révéler focus en modifiant les propriétés visuelles du focus pour chaque contrôle : [FocusVisualPrimaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Ces propriétés vous permettent de personnaliser la couleur et l’épaisseur du rectangle de focus. (Ce sont les mêmes propriétés que vous utilisez pour la création de [visuels de focus à haute visibilité](../input/guidelines-for-visualfeedback.md#high-visibility-focus-visuals).) 

Mais avant de commencer la personnalisation, il est utile de connaître un peu plus les composants de l’effet Révéler focus.

Les visuels de Révéler focus par défaut sont composés de trois éléments : la bordure principale, la bordure secondaire et la lumière. La bordure principale a une épaisseur de **2 px** et apparaît autour de la bordure secondaire *extérieure*. La bordure secondaire a une épaisseur de **1 px** et apparaît autour de la bordure principale *intérieure*. La lumière de Révéler focus a une épaisseur proportionnelle à l’épaisseur de la bordure principale et apparaît autour de la bordure principale *extérieure*.

En plus des éléments statiques, les visuels de Révéler focus présentent une lumière animée qui clignote au repos et se déplace dans la direction du focus lors du déplacement de ce dernier.

![Couches de l’effet Révéler focus](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personnaliser l’épaisseur des bordures

Pour modifier l’épaisseur des types de bordures d’un contrôle, utilisez ces propriétés :

| Type de bordure | Propriété |
| --- | --- |
| Principale, Lumière   | [FocusVisualPrimaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Le fait de modifier la bordure principale modifie de façon proportionnelle l’épaisseur de la lumière.)   |
| Secondaire   | [FocusVisualSecondaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


Cet exemple modifie l’épaisseur de la bordure du visuel du focus d’un bouton :

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personnaliser la marge

La marge est l’espace entre les limites de l’élément visuel du contrôle et le début de la bordure secondaire des visuels du focus. La marge par défaut est à 1 px des limites du contrôle. Vous pouvez modifier cette marge pour chaque contrôle, en modifiant la propriété [FocusVisualMargin](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin) :

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Une marge négative éloigne la bordure du centre du contrôle, tandis qu’une marge positive rapproche la bordure du centre du contrôle.

## <a name="customize-the-color"></a>Personnaliser la couleur

Pour modifier la couleur du visuel Révéler focus, utilisez les propriétés [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) et [FocusVisualSecondaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush).

| Propriété | Ressource par défaut | Valeur de la ressource par défaut |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(La propriété FocusPrimaryBrush a seulement pour valeur par défaut les ressources **SystemControlRevealFocusVisualBrush** quand **FocusVisualKind** est défini sur **Reveal**. Dans le cas contraire, elle utilise **SystemControlFocusVisualPrimaryBrush**.)


Pour changer la couleur du visuel du focus d’un contrôle individuel, définissez les propriétés directement sur le contrôle. Cet exemple remplace les couleurs du visuel du focus d’un bouton.

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

Pour changer la couleur de tous les visuels du focus dans l’ensemble de votre application, vous devez remplacer les ressources **SystemControlRevealFocusVisualBrush** et **SystemControlFocusVisualSecondaryBrush** par votre propre définition :

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Pour plus d’informations sur la modification de la couleur du visuel du focus, consultez [Personnalisation des couleurs et des visuels du focus](../input/guidelines-for-visualfeedback.md#color-branding--customizing).


## <a name="show-just-the-glow"></a>Afficher uniquement la lumière

Si vous voulez utiliser seulement la lumière sans le visuel du focus principal ou secondaire, définissez simplement la propriété [FocusVisualPrimaryBrush](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) du contrôle sur `Transparent` et [FocusVisualSecondaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) sur `0`. Dans ce cas, la lumière adopte la couleur de l’arrière-plan du contrôle pour offrir un aspect sans bordure. Vous pouvez modifier l’épaisseur de la lumière avec [FocusVisualPrimaryThickness](/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Utiliser vos propres visuels de focus

Une autre façon de personnaliser Révéler focus consiste à choisir de ne pas utiliser les visuels de focus fournis par le système en dessinant vos propres états visuels. Pour plus d’informations, consultez [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Révéler focus et le système Fluent Design

Révéler focus est un composant du système Fluent Design qui ajoute des effets lumineux à votre application. Pour plus d’informations sur le système Fluent Design et ses autres composants, consultez [Présentation de Fluent Design](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Articles connexes

- [Effet Révéler](./reveal.md)
- [Conception pour Xbox et TV](../devices/designing-for-tv.md)
- [Interactions entre le boîtier de commande et la télécommande](../input/gamepad-and-remote-interactions.md)
- [Exemples de visuels de focus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
- [Effets de composition](../../composition/composition-effects.md)
- [Science in the System: Fluent Design and Depth](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Science in the System: Fluent Design and Light](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)

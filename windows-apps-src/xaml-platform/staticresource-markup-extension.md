---
description: Fournit une valeur pour un attribut XAML en évaluant une référence à une ressource déjà définie. Les ressources sont définies dans un ResourceDictionary, et une utilisation StaticResource référence la clé de cette ressource dans le ResourceDictionary.
title: Extension de balisage StaticResource
ms.assetid: D50349B5-4588-4EBD-9458-75F629CCC395
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 32d09d4366d5ee05fe9581c4c8076811cfa0436f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155003"
---
# <a name="staticresource-markup-extension"></a>Extension de balisage {StaticResource}


Fournit une valeur pour un attribut XAML en évaluant une référence à une ressource déjà définie. Les ressources sont définies dans un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary), et une utilisation **StaticResource** fait référence à la clé de cette ressource dans le **ResourceDictionary**.

## <a name="xaml-attribute-usage"></a>Utilisation des attributs XAML

``` syntax
<object property="{StaticResource key}" .../>
```

## <a name="xaml-values"></a>Valeurs XAML

| Terme | Description |
|------|-------------|
| key | Clé pour la ressource demandée. Cette clé est initialement affectée par l’élément [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary). Une clé de ressource peut correspondre à toute chaîne définie dans la grammaire XamlName. |

## <a name="remarks"></a>Remarques

**StaticResource** est une technique permettant d’obtenir des valeurs d’un attribut XAML définies ailleurs dans un dictionnaire de ressources XAML. Ces valeurs peuvent être placées dans un dictionnaire de ressources car elles sont destinées à être partagées par plusieurs valeurs de propriété, ou car un dictionnaire de ressources XAML est utilisé comme technique de packaging ou de factorisation XAML. Le dictionnaire de thèmes pour un contrôle est un exemple de technique de packaging XAML. Des dictionnaires de ressources fusionnés utilisés pour les ressources de secours en sont un autre exemple.

**StaticResource** prend un argument, lequel spécifie la clé de la ressource demandée. Une clé de ressource est toujours une chaîne dans le code XAML Windows Runtime. Pour plus d’informations sur la façon dont la clé de ressource est initialement spécifiée, voir [Attribut x:Key](x-key-attribute.md).

Les règles selon lesquelles un **StaticResource** est résolu en un élément d’un dictionnaire de ressources ne sont pas décrites dans cette rubrique. Cela varie selon que la référence et la ressource existent toutes les deux dans un modèle, que des dictionnaires de ressources fusionnés sont utilisés, etc. Pour plus d’informations sur la façon de définir des ressources et d’utiliser correctement un élément [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary), et pour obtenir un exemple de code, voir l’article [Références aux ressources ResourceDictionary et XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).

**Important**    **StaticResource** ne doit pas essayer de créer une référence avant à une ressource qui est définie de manière lexicale dans le fichier XAML. Une telle tentative n’est pas prise en charge. Même si la référence anticipée n’échoue pas, toute tentative en ce sens pénalise les performances. Pour obtenir de meilleurs résultats, ajustez la composition de vos dictionnaires de ressources afin d’éviter les références anticipées.

Une tentative de spécification d’un **StaticResource** sur une clé impossible à résoudre lève une exception d’analyse XAML au moment de l’exécution. Les outils de conception peuvent également générer des avertissements ou des erreurs.

Dans l’implémentation du processeur XAML Windows Runtime, il n’existe aucune représentation de classe de stockage pour la fonctionnalité **StaticResource**. **StaticResource** est à utiliser exclusivement en XAML. L’équivalent le plus proche dans le code consiste à utiliser l’API de collection d’un élément [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary), par exemple l’appel à [**Contains**](/uwp/api/windows.ui.xaml.resourcedictionary.contains) ou à [**TryGetValue**](/uwp/api/windows.ui.xaml.resourcedictionary.trygetvalue).

L’[extension de balisage {ThemeResource}](themeresource-markup-extension.md) est une extension de balisage similaire qui référence des ressources nommées à un autre emplacement. La différence est la suivante : l’extension de balisage {ThemeResource} peut retourner d’autres ressources en fonction du thème système actif. Pour plus d’informations, voir [Extension de balisage {ThemeResource}](themeresource-markup-extension.md).

**StaticResource** est une extension de balisage. Les extensions de balisage sont généralement implémentées pour éviter que les valeurs d’attribut ne soient autre chose que des valeurs littérales ou des noms de gestionnaire et lorsque l’exigence dépasse le cadre de la définition de convertisseurs de type sur certains types ou propriétés. Toutes les extensions de balisage en XAML utilisent les \{ caractères « » et « » \} dans leur syntaxe d’attribut, qui est la Convention selon laquelle un processeur XAML reconnaît qu’une extension de balisage doit traiter l’attribut.

### <a name="an-example-staticresource-usage"></a>Exemple d’utilisation de {StaticResource}

Cet exemple de code XAML provient de l’[exemple de liaison de données XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

```xml
<StackPanel Margin="5">
    <!-- Add converter as a resource to reference it from a Binding. --> 
    <StackPanel.Resources>
        <local:S2Formatter x:Key="GradeConverter"/>
    </StackPanel.Resources>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Percent grade:" Margin="5" />
    <Slider x:Name="sliderValueConverter" Minimum="1" Maximum="100" Value="70" Margin="5"/>
    <TextBlock Style="{StaticResource BasicTextStyle}" Text="Letter grade:" Margin="5"/>
    <TextBox x:Name="tbValueConverterDataBound"
      Text="{Binding ElementName=sliderValueConverter, Path=Value, Mode=OneWay,  
        Converter={StaticResource GradeConverter}}" Margin="5" Width="150"/> 
</StackPanel> 
```

Cet exemple particulier crée en tant que ressource d’un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) un objet qui est stocké par une classe personnalisée. Pour être une ressource valide, cet élément `local:S2Formatter` doit également avoir une valeur d’attribut **x:Key**. La valeur définie pour l’attribut est « GradeConverter ».

La ressource est ensuite demandée juste un peu plus loin dans le code XAML, au niveau de `{StaticResource GradeConverter}`.

Notez la façon dont l’utilisation de l’extension de balisage {StaticResource} définit une propriété d’une autre extension de balisage, l’[extension de balisage {Binding}](binding-markup-extension.md). Il y a donc deux utilisations d’extensions de balisage imbriquées ici. L’utilisation la plus imbriquée est évaluée en premier, afin que la ressource soit obtenue en premier et puisse être utilisée comme valeur. Ce même exemple est présenté dans Extension de balisage {Binding}.

## <a name="design-time-tools-support-for-the-staticresource-markup-extension"></a>Prise en charge d’outils au moment de la conception pour l’extension de balisage **{StaticResource}**

Microsoft Visual Studio 2013 peut inclure les valeurs de clés possibles dans les listes déroulantes Microsoft IntelliSense lorsque vous utilisez l’extension de balisage **{StaticResource}** dans une page XAML. Par exemple, dès que vous tapez « {StaticResource », toute clé de ressource provenant de l’étendue de recherche actuelle s’affiche dans les listes déroulantes IntelliSense. En plus des ressources standard présentes au niveau de la page ([**FrameworkElement.Resources**](/uwp/api/windows.ui.xaml.frameworkelement.resources)) et au niveau de l’application ([**Application.Resources**](/uwp/api/windows.ui.xaml.application.resources)), vous pouvez aussi apercevoir les [ressources de thème XAML](../design/controls-and-patterns/xaml-theme-resources.md), ainsi que les ressources de toute extension utilisée par votre projet.

Lorsqu’une clé de ressource fait partie d’une utilisation de **{StaticResource}** quelconque, la fonctionnalité **Atteindre la définition** (F12) peut résoudre cette ressource et vous montrer le dictionnaire dans lequel elle est définie. Pour les ressources de thème, il s’agit du fichier generic.xaml à utiliser au moment de la conception.

## <a name="related-topics"></a>Rubriques connexes

* [Références aux ressources ResourceDictionary et XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary)
* [Attribut x:Key](x-key-attribute.md)
* [Extension de balisage {ThemeResource}](themeresource-markup-extension.md)
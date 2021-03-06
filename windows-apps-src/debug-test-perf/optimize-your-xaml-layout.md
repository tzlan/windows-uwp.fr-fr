---
ms.assetid: 79CF3927-25DE-43DD-B41A-87E6768D5C35
title: Optimiser votre disposition XAML
description: La disposition peut s’avérer coûteuse pour une application XAML,\# tant au niveau de l’utilisation du processeur que de la surcharge de la mémoire. Voici quelques mesures simples que vous pouvez entreprendre pour améliorer les performances de la disposition de votre application XAML.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29bf53560759b8e691ac016987efb0226ace9309
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154173"
---
# <a name="optimize-your-xaml-layout"></a>Optimiser votre disposition XAML


**API importantes**

-   [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)

La disposition est le processus de définition de la structure visuelle de l’interface utilisateur. Les panneaux sont le mécanisme principal utilisé pour décrire la disposition en XAML. Ce sont des objets conteneurs permettant de positionner et d’organiser les éléments d’interface qu’ils contiennent. La disposition peut s’avérer coûteuse pour une application XAML, tant au niveau de l’utilisation du processeur que de la surcharge de la mémoire. Voici quelques mesures simples que vous pouvez entreprendre pour améliorer les performances de la disposition de votre application XAML.

## <a name="reduce-layout-structure"></a>Réduire la structure de la disposition

La meilleure solution pour améliorer les performances de la disposition consiste à simplifier la structure hiérarchique de l’arborescence des éléments d’interface utilisateur. Les panneaux existent dans l’arborescence visuelle, mais ce sont des éléments structurels, pas des *éléments produisant des pixels* comme une classe [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) ou [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Le fait de simplifier l’arborescence en réduisant le nombre d’éléments qui ne produisent pas de pixels permet généralement d’améliorer significativement les performances.

De nombreuses interfaces utilisateur sont implémentées en imbriquant des panneaux, ce qui crée des arborescences complexes et étendues de panneaux et d’éléments. L’imbrication de panneaux est pratique, mais le plus souvent il est possible d’obtenir la même interface utilisateur avec un seul panneau plus complexe. Les performances sont meilleures en utilisant un seul panneau.

### <a name="when-to-reduce-layout-structure"></a>À quel moment réduire la structure de la disposition

Le fait de réduire la structure de la disposition de manière simple (par exemple, en réduisant un panneau imbriqué à partir de la page de niveau supérieur) n’a pas d’effet visible.

Pour optimiser les performances, il faut réduire la structure de la disposition qui se répète dans l’interface utilisateur, comme dans une classe [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) ou [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView). Ces éléments [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) utilisent une classe [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate), qui définit une sous-arborescence d’éléments d’interface utilisateur qui est instanciée plusieurs fois. Lorsque la même sous-arborescence est dupliquée de nombreuses fois dans votre application, les améliorations des performances de cette sous-arborescence démultiplient les performances globales de votre application.

### <a name="examples"></a>Exemples

Examinez l’interface utilisateur suivante :

![Exemple de disposition de formulaire](images/layout-perf-ex1.png)

Ces exemples montrent 3 méthodes d’implémentation de la même interface utilisateur. Chaque implémentation choisie produit un nombre quasiment identique de pixels à l’écran. Les différences se jouent dans les détails.

Option 1 : Éléments [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) imbriqués

Bien que ce modèle soit le plus simple, il utilise 5 éléments panneau et entraîne une surcharge importante.

```xml
  <StackPanel>
  <TextBlock Text="Options:" />
  <StackPanel Orientation="Horizontal">
      <CheckBox Content="Power User" />
      <CheckBox Content="Admin" Margin="20,0,0,0" />
  </StackPanel>
  <TextBlock Text="Basic information:" />
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Name:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Email:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <StackPanel Orientation="Horizontal">
      <TextBlock Text="Password:" Width="75" />
      <TextBox Width="200" />
  </StackPanel>
  <Button Content="Save" />
</StackPanel>
```

Option 2 : Un seul élément [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid)

L’élément [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) ajoute une certaine complexité, mais utilise un seul élément panneau.

```xml
<Grid>
  <Grid.RowDefinitions>
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
      <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
      <ColumnDefinition Width="Auto" />
      <ColumnDefinition Width="Auto" />
  </Grid.ColumnDefinitions>
  <TextBlock Text="Options:" Grid.ColumnSpan="2" />
  <CheckBox Content="Power User" Grid.Row="1" Grid.ColumnSpan="2" />
  <CheckBox Content="Admin" Margin="150,0,0,0" Grid.Row="1" Grid.ColumnSpan="2" />
  <TextBlock Text="Basic information:" Grid.Row="2" Grid.ColumnSpan="2" />
  <TextBlock Text="Name:" Width="75" Grid.Row="3" />
  <TextBox Width="200" Grid.Row="3" Grid.Column="1" />
  <TextBlock Text="Email:" Width="75" Grid.Row="4" />
  <TextBox Width="200" Grid.Row="4" Grid.Column="1" />
  <TextBlock Text="Password:" Width="75" Grid.Row="5" />
  <TextBox Width="200" Grid.Row="5" Grid.Column="1" />
  <Button Content="Save" Grid.Row="6" />
</Grid>
```

Option 3 : Un seul élément [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) :

Ce panneau unique est également un peu plus complexe que l’utilisation de panneaux imbriqués, mais peut être plus facile à comprendre et à gérer qu’un élément [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid).

```xml
<RelativePanel>
  <TextBlock Text="Options:" x:Name="Options" />
  <CheckBox Content="Power User" x:Name="PowerUser" RelativePanel.Below="Options" />
  <CheckBox Content="Admin" Margin="20,0,0,0" 
            RelativePanel.RightOf="PowerUser" RelativePanel.Below="Options" />
  <TextBlock Text="Basic information:" x:Name="BasicInformation"
           RelativePanel.Below="PowerUser" />
  <TextBlock Text="Name:" RelativePanel.AlignVerticalCenterWith="NameBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="NameBox"               
           RelativePanel.Below="BasicInformation" />
  <TextBlock Text="Email:"  RelativePanel.AlignVerticalCenterWith="EmailBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="EmailBox"
           RelativePanel.Below="NameBox" />
  <TextBlock Text="Password:" RelativePanel.AlignVerticalCenterWith="PasswordBox" />
  <TextBox Width="200" Margin="75,0,0,0" x:Name="PasswordBox"
           RelativePanel.Below="EmailBox" />
  <Button Content="Save" RelativePanel.Below="PasswordBox" />
</RelativePanel>
```

Comme ces exemples le montrent, il existe de nombreuses façons d’obtenir la même interface utilisateur. Vous devez faire votre choix en réfléchissant à tous les compromis, notamment les performances, la lisibilité et la maintenance.

## <a name="use-single-cell-grids-for-overlapping-ui"></a>Utiliser des grilles à cellule unique pour le chevauchement de l’interface utilisateur

Il est courant qu’une interface utilisateur ait une disposition dans laquelle les éléments se chevauchent. Généralement, pour positionner les éléments de cette manière, on utilise le remplissage, les marges, les alignements et les transformations. Le contrôle [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) XAML est optimisé pour améliorer les performances de la disposition des éléments qui se chevauchent.

**Important**  Pour voir l’amélioration, utilisez un élément [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) à une cellule. Ne définissez pas [**RowDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.rowdefinitions) ou [**ColumnDefinitions**](/uwp/api/windows.ui.xaml.controls.grid.columndefinitions).

### <a name="examples"></a>Exemples

```xml
<Grid>
    <Ellipse Fill="Red" Width="200" Height="200" />
    <TextBlock Text="Test" 
               HorizontalAlignment="Center" 
               VerticalAlignment="Center" />
</Grid>
```

![Texte superposé dans un cercle](images/layout-perf-ex2.png)

```xml
<Grid Width="200" BorderBrush="Black" BorderThickness="1">
    <TextBlock Text="Test1" HorizontalAlignment="Left" />
    <TextBlock Text="Test2" HorizontalAlignment="Right" />
</Grid>
```

![Deux blocs de texte dans une grille](images/layout-perf-ex3.png)

## <a name="use-a-panels-built-in-border-properties"></a>Utiliser les propriétés intégrées de bordure du panneau

Les contrôles [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel), [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) et [**ContentPresenter**](/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter) ont des propriétés de bordure intégrée qui vous permettent de dessiner une bordure autour d’eux sans avoir à ajouter un élément [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) supplémentaire à votre XAML. Les nouvelles propriétés qui prennent en charge la bordure intégrée sont les suivantes : **BorderBrush**, **BorderThickness**, **CornerRadius** et **Padding**. Chacune d’elles est une [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty). Vous pouvez donc les utiliser avec les liaisons et les animations. Elles sont conçues pour remplacer intégralement un élément **Border** séparé.

Si votre interface utilisateur comporte des éléments [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) autour de ces panneaux, utilisez la bordure intégrée à la place, qui enregistre un élément supplémentaire dans la structure de la disposition de votre application. Comme mentionné précédemment, cela peut représenter une économie importante, notamment dans le cas d’une interface utilisateur répétée.

### <a name="examples"></a>Exemples

```xml
<RelativePanel BorderBrush="Red" BorderThickness="2" CornerRadius="10" Padding="12">
    <TextBox x:Name="textBox1" RelativePanel.AlignLeftWithPanel="True"/>
    <Button Content="Submit" RelativePanel.Below="textBox1"/>
</RelativePanel>
```

## <a name="use-sizechanged-events-to-respond-to-layout-changes"></a>Utilisez des événements **SizeChanged** pour réagir à des modifications de disposition.

La classe [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) expose deux événements similaires pour répondre aux changements de disposition : [**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) et [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Vous utilisez peut-être l’un de ces événements pour recevoir une notification lorsqu’un élément est redimensionné pendant la disposition. La sémantique des deux événements est différente, et le choix de l’un ou l’autre influe considérablement sur les performances.

Pour des performances optimales, [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) est presque toujours le bon choix. **SizeChanged** a une sémantique intuitive. Il est déclenché pendant la disposition lorsque la taille de [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) a été mise à jour.

[**LayoutUpdated**](/uwp/api/windows.ui.xaml.frameworkelement.layoutupdated) est également déclenché pendant la disposition, mais il a une sémantique globale : il est déclenché au niveau de chaque élément chaque fois qu’un élément est mis à jour. Il est courant de faire uniquement un traitement local dans le gestionnaire d’événement, auquel cas le code est exécuté plus souvent que nécessaire. Utilisez **LayoutUpdated** seulement si vous avez besoin de savoir quand un élément est repositionné sans modification de taille (ce qui est rare).

## <a name="choosing-between-panels"></a>Choix entre des panneaux

Les performances ne sont généralement pas prises en compte lors du choix entre des panneaux individuels. Ce choix repose habituellement sur la prise en compte du panneau fournissant le comportement de disposition le plus proche de l’interface utilisateur que vous implémentez. Par exemple, si vous choisissez entre [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid), [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel)et [**RelativePanel**](/uwp/api/Windows.UI.Xaml.Controls.RelativePanel), vous devez choisir le panneau de configuration qui fournit le mappage le plus proche de votre modèle mental de l’implémentation.

Chaque panneau XAML est optimisé pour des performances optimales, et tous les panneaux fournissent des performances similaires pour une interface utilisateur similaire.
---
description: Obtenez des modèles d’éléments que vous pouvez utiliser avec un contrôle ListView pour afficher des éléments de liste unique, double, triple et tabulaire.
title: Modèles d’éléments d’affichage Liste
template: detail.hbs
ms.date: 11/03/2017
ms.topic: article
keywords: windows 10, uwp, fluent
ms.openlocfilehash: fb4e45721c1da399e8b51974bef9f55b0c70e16c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172673"
---
# <a name="item-templates-for-list-view"></a>Modèles d’éléments d’affichage Liste

Cette section contient des modèles d’éléments que vous pouvez utiliser avec un contrôle [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView). Utilisez ces modèles pour obtenir l’apparence des types d’applications courants. 

Pour illustrer la liaison de données, ces modèles lient des **ListViewItems** à l’exemple de classe Recording de la [vue d’ensemble de la liaison de données](../../data-binding/data-binding-quickstart.md).

> [!NOTE] 
> Quand un **DataTemplate** contient plusieurs contrôles (par exemple, plusieurs **TextBlock**), le nom accessible par défaut pour les lecteurs d’écran est issu de .ToString() sur l’élément. Par commodité, vous pouvez plutôt définir l’[**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties) sur l’élément racine du **DataTemplate**. Pour en savoir plus sur l’accessibilité, consultez [Vue d’ensemble de l’accessibilité](../accessibility/accessibility-overview.md).

## <a name="single-line-list-item"></a>Élément de liste monoligne
Utilisez ce modèle pour afficher une liste d’éléments avec une image et une seule ligne de texte.

![exemple d’élément de liste monoligne](images/listitems/singlelineexample.png)
![élément de liste monoligne](images/listitems/singlelineicon.png)
```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="SingleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="44" Padding="12" AutomationProperties.Name="{x:Bind CompositionName}">
                <Image Source="Placeholder.png" Height="16" Width="16" VerticalAlignment="Center"/>
                <TextBlock Text="{x:Bind CompositionName}" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" Margin="12,0,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="double-line-list-item"></a>Élément de liste à deux lignes 
Utilisez ce modèle pour afficher une liste d’éléments avec une image et deux lignes de texte.

![exemple d’élément de liste à deux lignes avec une icône](images/listitems/doublelineexample.png) 
![élément de liste à deux lignes avec une icône](images/listitems/doublelineicon.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="triple-line-list-item"></a>Élément de liste à trois lignes
Utilisez ce modèle pour afficher une liste d’éléments avec trois lignes de texte.

![exemple d’élément de liste à trois lignes](images/listitems/triplelineexample.png)
![élément de liste à trois lignes](images/listitems/tripleline.png)

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TripleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Height="84" Padding="20" AutomationProperties.Name="{x:Bind CompositionName}">
                <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".8" Margin="0,4,0,0"/>
                <TextBlock Text="{x:Bind ReleaseDateTime}" Style="{ThemeResource CaptionTextBlockStyle}" Opacity=".6" Margin="0,4,0,0"/>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="table-list-item"></a>Élément de liste en tableau
Utilisez ce modèle pour afficher une liste d’éléments avec du texte dans des colonnes définies.

![Exemple d’élément de liste en tableau](images/listitems/tablelist.png)
```xaml
<ListView  ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.HeaderTemplate>
        <DataTemplate>
            <Grid Padding="12" Background="{ThemeResource SystemBaseLowColor}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="408"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Composition" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="1" Text="Artist" Style="{ThemeResource CaptionTextBlockStyle}"/>
                <TextBlock Grid.Column="2" Text="Release Date" Style="{ThemeResource CaptionTextBlockStyle}"/>
            </Grid>
        </DataTemplate>
    </ListView.HeaderTemplate>
    <ListView.ItemTemplate>
        <DataTemplate x:Name="TableDataTemplate" x:DataType="local:Recording">
            <Grid Height="48" AutomationProperties.Name="{x:Bind CompositionName}">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="48"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                    <ColumnDefinition Width="360"/>
                </Grid.ColumnDefinitions>
                <Ellipse Height="32" Width="32" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <TextBlock Grid.Column="1" VerticalAlignment="Center" Style="{ThemeResource BaseTextBlockStyle}" Text="{x:Bind CompositionName}" />
                <TextBlock Grid.Column="2" VerticalAlignment="Center" Text="{x:Bind ArtistName}"/>
                <TextBlock Grid.Column="3" VerticalAlignment="Center" Text="{x:Bind ReleaseDateTime}"/>
            </Grid>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

## <a name="related-articles"></a>Articles connexes
- [Classe ListView](/uwp/api/windows.ui.xaml.controls.listview)
- [Vue d’ensemble de la liaison de données](../../data-binding/data-binding-quickstart.md)
- [Vue d’ensemble de l’accessibilité](../accessibility/accessibility-overview.md)
- [Exemple de ListView et GridView (Windows 10)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Images miniatures](../../files/thumbnails.md)
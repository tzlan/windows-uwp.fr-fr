---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: Cette étude de cas, qui repose sur les informations fournies dans Bookstore, commence par une application Silverlight pour Windows Phone qui affiche des données groupées dans un élément LongListSelector.
title: Windows Phone l’étude de cas Silverlight pour UWP, Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 48603651c8e12ed452d8d3b136bfd3e9b8788316
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172933"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore2"></a>Étude de cas de portage d’une application Silverlight pour Windows Phone vers UWP : Bookstore2


Cette étude de cas, qui repose sur les informations fournies dans [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), commence par une application Silverlight pour Windows Phone qui affiche des données groupées dans un élément **LongListSelector**. Dans le modèle d’affichage, chaque instance de la classe **Author** représente l’ensemble des livres écrits par l’auteur en question ; dans l’élément **LongListSelector**, nous pouvons visualiser la liste des livres regroupés par auteur ou nous pouvons effectuer un zoom arrière pour afficher une liste de raccourcis relatifs aux auteurs. Grâce à cette liste, vous pouvez vous déplacer beaucoup plus rapidement que si vous faisiez défiler la liste des ouvrages. Nous suivons la procédure de portage de l’application vers une application de plateforme Windows universelle (UWP) Windows 10.

**Remarque**    Lors de l’ouverture \_ de Bookstore2Universal 10 dans Visual Studio, si vous voyez le message « mise à jour requise de Visual Studio », suivez les étapes de définition de la version de la plateforme cible dans [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Téléchargements

[Téléchargez l’application Silverlight pour Windows Phone Bookstore2WPSL8](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2WPSL8).

[Téléchargez l' \_ application Bookstore2Universal 10 Windows 10](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10).

##  <a name="the-windowsphone-silverlight-app"></a>Application Silverlight pour Windows Phone

Voici à quoi ressemble Bookstore2WPSL8, l’application que nous allons porter. Il s’agit d’un élément **LongListSelector** permettant le défilement vertical des livres regroupés par auteur. Vous pouvez effectuer un zoom arrière vers la liste de raccourcis et, à partir de cette dernière, revenir à n’importe quel groupe. Cette application comporte deux parties principales : le modèle d’affichage, qui fournit la source de données groupées, et l’interface utilisateur, qui est liée à ce modèle d’affichage. Comme nous allons le voir, ces deux parties sont facilement portées depuis la technologie Silverlight pour Windows Phone vers la plateforme Windows universelle (UWP).

![Apparence de l’application Bookstore2WPSL8](images/wpsl-to-uwp-case-studies/c02-01-wpsl-how-the-app-looks.png)

##  <a name="porting-to-a-windows10-project"></a>Portage d’une application vers un projet Windows 10

La procédure consistant à créer un projet dans Visual Studio, puis à copier des fichiers depuis Bookstore2WPSL8 et à insérer les fichiers copiés dans le nouveau projet, est très rapide. Commencez par créer un projet Application vide (universelle Windows). Nommez-le Bookstore2Universal \_ 10. Il s’agit des fichiers à copier à partir de Bookstore2WPSL8 vers Bookstore2Universal \_ 10.

-   Copiez le dossier contenant les fichiers PNG de l’image de couverture du livre (le dossier est \\ ressources \\ CoverImages). Une fois le dossier copié, dans l’**Explorateur de solutions**, assurez-vous que l’option **Afficher tous les fichiers** est activée. Cliquez avec le bouton droit de la souris sur le dossier que vous avez copié et sélectionnez **Inclure dans le projet**. Cette commande correspond à ce que nous avons voulu dire par « insertion » de fichiers ou de dossiers dans un projet. Chaque fois que vous copiez un fichier ou un dossier, sélectionnez **Actualiser** dans l’**Explorateur de solutions**, puis incluez le fichier ou dossier dans le projet. Cette opération n’est pas nécessaire pour les fichiers dont vous modifiez la destination.
-   Copiez le dossier contenant le fichier source du modèle d’affichage (le dossier est \\ ViewModel).
-   Copiez le fichier MainPage.xaml et remplacez le fichier dans la destination.

Nous pouvons conserver les fichiers App.xaml et App.xaml.cs générés par Visual Studio dans le projet Windows 10.

Modifiez le code source et les fichiers de balisage que vous venez de copier, puis remplacez toutes les références à l’espace de noms Bookstore2WPSL8 par Bookstore2Universal \_ 10. Une méthode rapide consiste à utiliser la fonctionnalité **Remplacer dans les fichiers**. Dans le code impératif du fichier source du modèle d’affichage, apportez les modifications de portage ci-après.

-   Remplacez `System.ComponentModel.DesignerProperties` par `DesignMode`, puis appliquez-lui la commande **Résoudre**. Supprimez la propriété `IsInDesignTool` et utilisez IntelliSense pour ajouter le nom de propriété correct : `DesignModeEnabled`.
-   Utilisez la commande **Résoudre** sur `ImageSource`.
-   Utilisez la commande **Résoudre** sur `BitmapImage`.
-   Supprimez les éléments `using System.Windows.Media;` et `using System.Windows.Media.Imaging;`.
-   Remplacez la valeur retournée par la propriété **Bookstore2Universal \_ 10. BookstoreViewModel. AppName** « BOOKSTORE2WPSL8 » par « Bookstore2Universal ».
-   Comme nous l’avons fait dans le cas de [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), mettez à jour l’implémentation de la propriété **BookSku.CoverImage** (voir [Liaison d’une propriété Image à un modèle d’affichage](wpsl-to-uwp-case-study-bookstore1.md)).

Dans le fichier MainPage.xaml, apportez les modifications de portage initiales ci-après.

-   Remplacez l’élément `phone:PhoneApplicationPage` par `Page` (y compris les occurrences figurant dans la syntaxe des éléments de propriété).
-   Supprimez les déclarations de préfixe d’espace de noms `phone` et `shell`.
-   Remplacez l’élément « clr-namespace » par « using » dans la déclaration de préfixe d’espace de noms restante.
-   Supprimez les éléments `SupportedOrientations="Portrait"` et `Orientation="Portrait"`, puis configurez l’option **Portrait** dans le manifeste du package d’application du nouveau projet.
-   Supprimez `shell:SystemTray.IsVisible="True"`.
-   Les types des convertisseurs d’élément de liste de raccourcis (qui sont présents dans le balisage en tant que ressources) ont été déplacés vers l’espace de noms [**Windows. UI. Xaml. Controls. Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) . Par conséquent, ajoutez la déclaration de préfixe de l’espace de noms fenêtres \_ de contrôles XAML de l’interface utilisateur \_ \_ \_ et mappez-la à **Windows. UI. Xaml. Controls. Primitives**. Dans les ressources des convertisseurs d’éléments de la liste de raccourcis, remplacez le préfixe `phone:` par `Windows_UI_Xaml_Controls_Primitives:`.
-   Tout comme nous l’avons fait pour [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), remplacez toutes les références au `PhoneTextExtraLargeStyle` style  **TextBlock** par une référence à `SubtitleTextBlockStyle` , remplacez `PhoneTextSubtleStyle` par `SubtitleTextBlockStyle` , remplacez `PhoneTextNormalStyle` par `CaptionTextBlockStyle` et remplacez par `PhoneTextTitle1Style` `HeaderTextBlockStyle` .
-   Il existe une seule exception dans `BookTemplate`. Le style du second élément **TextBlock** doit référencer `CaptionTextBlockStyle`.
-   Supprimez l’attribut FontFamily de l’élément **TextBlock** dans `AuthorGroupHeaderTemplate` et définissez l’arrière-plan de l’élément **Border** pour qu’il référence `SystemControlBackgroundAccentBrush` au lieu de `PhoneAccentBrush`.
-   Suite aux [modifications associées aux pixels d’affichage](wpsl-to-uwp-porting-xaml-and-ui.md), parcourez le balisage et multipliez toutes les valeurs de taille fixes par 0,8 (marges, largeur, hauteur, etc.).

## <a name="replacing-the-longlistselector"></a>Remplacement de l’élément LongListSelector


La procédure de remplacement de l’élément **LongListSelector** par un contrôle [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) comprend plusieurs étapes. Nous allons nous concentrer sur ces dernières. Un élément **LongListSelector** est directement lié à la source de données groupées, mais un élément **SemanticZoom** contient des contrôles [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) ou [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) qui sont liés de manière indirecte aux données, par l’intermédiaire d’un adaptateur [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource). L’élément **CollectionViewSource** doit être présent dans le balisage en tant que ressource ; nous pouvons donc commencer par l’ajouter au balisage dans le fichier MainPage.xaml, dans `<Page.Resources>`.

```xml
    <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{Binding Authors}"
        IsSourceGrouped="true"/>
```

Notez que la liaison sur **LongListSelector.ItemsSource** devient la valeur de **CollectionViewSource.Source**, et l’élément **LongListSelector.IsGroupingEnabled** devient **CollectionViewSource.IsSourceGrouped**. L’élément **CollectionViewSource** présente un nom (et non une clé, comme on pourrait s’y attendre), donc nous pouvons effectuer la liaison vers ce dernier.

Ensuite, remplacez l’élément `phone:LongListSelector` par ce balisage. Cela nous permet de proposer une version préliminaire de **SemanticZoom** que nous pouvons utiliser :

```xml
    <SemanticZoom>
        <SemanticZoom.ZoomedInView>
            <ListView
                ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource BookTemplate}">
                <ListView.GroupStyle>
                    <GroupStyle
                        HeaderTemplate="{StaticResource AuthorGroupHeaderTemplate}"
                        HidesIfEmpty="True"/>
                </ListView.GroupStyle>
            </ListView>
        </SemanticZoom.ZoomedInView>
        <SemanticZoom.ZoomedOutView>
            <ListView
                ItemsSource="{Binding CollectionGroups, Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource ZoomedOutAuthorTemplate}"/>
        </SemanticZoom.ZoomedOutView>
    </SemanticZoom>
```

La notion d’élément **LongListSelector** des modes de liste plate et de liste de raccourcis est associée à une réponse dans la notion d’élément **SemanticZoom** relative aux vues avec zoom avant et arrière, respectivement. La vue avec zoom avant est une propriété, que vous définissez sur une instance d’élément **ListView**. Dans ce cas, la vue avec zoom arrière est également définie sur un élément **ListView**, et les deux contrôles **ListView** sont liés à notre élément **CollectionViewSource**. La vue avec zoom avant utilise le même modèle d’élément, modèle d’en-tête de groupe et paramètre **HideEmptyGroups** (désormais appelé **HidesIfEmpty**) que la liste plate de l’élément **LongListSelector**. Par ailleurs, la vue avec zoom arrière utilise un modèle d’élément très semblable à celui qui figure dans le style de liste de raccourcis de l’élément **LongListSelector** (`AuthorNameJumpListStyle`). Notez également que la vue avec zoom arrière est liée à une propriété spéciale de l’élément **CollectionViewSource**, appelée **CollectionGroups**, qui correspond à une collection contenant les groupes plutôt que les éléments.

Nous n’avons plus besoin de l’élément `AuthorNameJumpListStyle`, pas dans son intégralité, en tout cas. Nous avons uniquement besoin du modèle de données associé aux groupes (qui sont les auteurs dans cette application) dans la vue avec zoom arrière. Pour cette raison, nous allons supprimer le style `AuthorNameJumpListStyle` et le remplacer par le modèle de données ci-après.

```xml
   <DataTemplate x:Key="ZoomedOutAuthorTemplate">
        <Border Margin="9.6,0.8" Background="{Binding Converter={StaticResource JumpListItemBackgroundConverter}}">
            <TextBlock Margin="9.6,0,9.6,4.8" Text="{Binding Group.Name}" Style="{StaticResource SubtitleTextBlockStyle}"
            Foreground="{Binding Converter={StaticResource JumpListItemForegroundConverter}}" VerticalAlignment="Bottom"/>
        </Border>
    </DataTemplate>
```

Notez que, dans la mesure où le contexte de données de ce modèle de données est un groupe plutôt qu’un élément, nous effectuons la liaison avec une propriété spéciale appelée **Group**.

Vous pouvez à présent générer et exécuter l’application. Voici comment cette dernière apparaît sur l’émulateur d’appareil mobile.

![Application UWP sur un appareil mobile avec les modifications du code source initial](images/wpsl-to-uwp-case-studies/c02-02-mob10-initial-source-code-changes.png)

Le modèle d’affichage et les vues avec zoom avant et arrière fonctionnent ensemble correctement. Toutefois, cette approche nécessite un peu plus de travail de stylisation et de création de modèles. Par exemple, les styles et les pinceaux corrects ne sont pas encore utilisés, donc le texte est invisible dans les en-têtes de groupe sur lesquels vous pouvez cliquer pour effectuer un zoom arrière. Si vous exécutez l’application sur un appareil de bureau, vous verrez un deuxième problème, ce qui signifie que l’application n’adapte pas encore son interface utilisateur pour offrir la meilleure expérience et utiliser de l’espace sur des appareils plus grands où Windows peut être potentiellement plus grand que l’écran d’un appareil mobile. Nous allons donc résoudre ces problèmes dans les sections ci-après ([Stylisation et création de modèles initiaux](#initial-styling-and-templating), [Interface utilisateur adaptative](#adaptive-ui) et [Stylisation finale](#final-styling)).

## <a name="initial-styling-and-templating"></a>Stylisation et création de modèles initiaux

Pour espacer harmonieusement les en-têtes de groupe, modifiez `AuthorGroupHeaderTemplate` et définissez un élément **Margin** de `"0,0,0,9.6"` sur l’élément **Border**.

Pour espacer harmonieusement les différents livres, modifiez `BookTemplate` et définissez l’élément **Margin** sur `"9.6,0"` sur les deux éléments **TextBlock**.

Pour améliorer la disposition du nom de l’application et du titre de la page, dans `TitlePanel`, supprimez l’élément **Margin** supérieur sur le second élément **TextBlock** en définissant la valeur `"7.2,0,0,0"`. Et sur l’élément `TitlePanel` proprement dit, définissez la marge sur `0` (ou sur toute valeur qui vous semble adaptée).

Remplacez l’arrière-plan de l’élément `LayoutRoot` par `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.

## <a name="adaptive-ui"></a>Interface utilisateur adaptative

Étant donné que nous avons commencé avec une application Windows Phone, il n’est pas surprenant que la disposition de l’interface utilisateur de notre application portée soit uniquement adaptée aux petits appareils et aux fenêtres étroites à ce stade du processus. Or, nous souhaiterions que la disposition de l’interface utilisateur s’adapte automatiquement et utilise l’espace de manière plus rationnelle lorsque l’application s’exécute dans une fenêtre large (ce qui n’est possible que sur un appareil doté d’un grand écran). Nous voudrions également qu’elle utilise uniquement l’interface utilisateur dont nous disposons actuellement lorsque la fenêtre de l’application est étroite (ce qui se produit sur un appareil de petite taille, et qui peut également survenir sur un appareil plus grand).

Nous pouvons utiliser la fonction Gestionnaire d’état visuel adaptative pour y parvenir. Nous allons définir des propriétés sur les éléments visuels afin que, par défaut, l’interface utilisateur présente une disposition étroite à l’aide des modèles que nous utilisons pour l’instant. Ensuite, nous détecterons les cas où la fenêtre de l’application est d’une largeur égale ou supérieure à une taille spécifique (mesurée en [pixels effectifs](wpsl-to-uwp-porting-xaml-and-ui.md)), et nous modifierons alors les propriétés des éléments visuels comme il convient afin d’obtenir une disposition plus grande et plus large. Nous affecterons à ces changements de propriété un état visuel ; nous utiliserons un déclencheur adaptatif pour effectuer un suivi en continu et déterminer si l’état visuel doit ou non être appliqué, selon la largeur de la fenêtre en pixels effectifs. Dans ce cas précis, nous baserons le déclenchement sur la largeur de la fenêtre, mais il est également possible de choisir la hauteur de la fenêtre comme déclencheur.

Une largeur minimale de 548 epx est appropriée pour ce cas d’utilisation, car cette valeur correspond à la taille de l’appareil le plus petit auquel nous voulons appliquer la disposition d’écran large. En général, les téléphones présentent une largeur inférieure à 548 epx. Ainsi, sur ce type d’appareil de petite taille, nous conserverons la disposition étroite par défaut. Sur un PC, la fenêtre s’ouvrira par défaut sur une largeur suffisante pour déclencher le passage à l’état large, qui affichera les éléments d’une taille de 250 x 250 epx. À partir de là, vous serez en mesure de faire glisser la fenêtre afin qu’elle soit suffisamment étroite pour afficher un minimum de deux colonnes d’éléments d’une taille de 250 x 250 epx. Si vous utilisez une largeur inférieure, le déclencheur sera désactivé, l’état visuel large sera supprimé, et la disposition étroite par défaut continuera d’être appliquée.

Avant d’aborder la partie consacrée au Gestionnaire d’état visuel adaptatif, nous devons commencer par concevoir l’état large, ce qui implique l’ajout de nouveaux éléments visuels et modèles à notre balisage. Ces étapes décrivent comment effectuer cette opération. Au moyen des conventions d’affectation de noms pour les éléments visuels et les modèles, nous allons inclure le mot « wide » dans le nom de tout élément ou modèle destiné à l’état large. Si un élément ou modèle ne contient pas le mot « wide », vous pouvez supposer qu’il s’agit de l’état étroit, qui constitue l’état par défaut et dont les valeurs de propriété sont définies en tant que valeurs locales sur les éléments visuels dans la page. Seules les valeurs de propriété relatives à l’état large sont définies par le biais d’un état visuel réel dans le balisage.

-   Effectuez une copie du contrôle [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) dans le balisage et définissez `x:Name="narrowSeZo"` sur la copie. Sur l’original, définissez à la fois `x:Name="wideSeZo"` et `Visibility="Collapsed"` afin que l’élément large ne soit pas visible par défaut.
-   Dans `wideSeZo`, remplacez les éléments **ListView** par des éléments **GridView** dans la vue avec zoom avant et dans la vue avec zoom arrière.
-   Effectuez une copie des trois ressources `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate` et `BookTemplate`, puis ajoutez le mot `Wide` aux clés de ces copies. Mettez également à jour l’élément `wideSeZo` pour qu’il référence les clés de ces nouvelles ressources.
-   Remplacez le contenu de l’élément `AuthorGroupHeaderTemplateWide` par `<TextBlock Style="{StaticResource SubheaderTextBlockStyle}" Text="{Binding Name}"/>`.
-   Remplacez le contenu du champ `ZoomedOutAuthorTemplateWide` avec :

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250" >
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
          <TextBlock Foreground="{StaticResource ListViewItemOverlayForegroundThemeBrush}"
              Style="{StaticResource SubtitleTextBlockStyle}"
            Height="80" Margin="15,0" Text="{Binding Group.Name}"/>
        </StackPanel>
    </Grid>
```

-   Remplacez le contenu du champ `BookTemplateWide` avec :

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250">
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <Image Source="{Binding CoverImage}" Stretch="UniformToFill"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}"
                TextWrapping="NoWrap" TextTrimming="CharacterEllipsis"
                Margin="12,0,24,0" Text="{Binding Title}"/>
            <TextBlock Style="{StaticResource CaptionTextBlockStyle}" Text="{Binding Author.Name}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}" TextWrapping="NoWrap"
                TextTrimming="CharacterEllipsis" Margin="12,0,12,12"/>
        </StackPanel>
    </Grid>
```

-   Pour l’état large, les groupes présentés dans la vue avec zoom avant devront être entourés de davantage d’espace sur le plan vertical. Pour obtenir les résultats voulus, nous devons créer et référencer un modèle de panneau d’éléments. Voici comment se présente le balisage.

```xml
   <ItemsPanelTemplate x:Key="ZoomedInItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" GroupPadding="0,0,0,20"/>
    </ItemsPanelTemplate>
    ...

    <SemanticZoom x:Name="wideSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <GridView
            ...
            ItemsPanel="{StaticResource ZoomedInItemsPanelTemplate}">
            ...
```

-   Enfin, ajoutez le balisage du Gestionnaire d’état visuel approprié en tant que premier enfant de l’élément `LayoutRoot`.

```xml
    <Grid x:Name="LayoutRoot" ... >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...
```

## <a name="final-styling"></a>Stylisation finale

Il ne reste plus qu’à procéder à quelques adaptations de stylisation finale.

-   Dans `AuthorGroupHeaderTemplate`, définissez `Foreground="White"` sur l’élément **TextBlock** afin qu’il apparaisse correctement lors de l’exécution sur la famille d’appareils mobiles.
-   Ajoutez `FontWeight="SemiBold"` à l’élément **TextBlock** dans `AuthorGroupHeaderTemplate` et `ZoomedOutAuthorTemplate`.
-   Dans `narrowSeZo`, les en-têtes de groupe et les auteurs affichés dans la vue avec zoom arrière sont alignés à gauche et non étirés. Nous allons donc travailler sur cet aspect. Nous allons créer un élément [**HeaderContainerStyle**](/uwp/api/windows.ui.xaml.controls.groupstyle.headercontainerstyle) pour la vue avec zoom avant, l’élément [**HorizontalContentAlignment**](/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) étant défini sur la valeur `Stretch`. Nous créerons également un élément [**ItemContainerStyle**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) pour la vue avec zoom arrière contenant ce même élément [**Setter**](/uwp/api/Windows.UI.Xaml.Setter). Voici ce que cela donne.

```xml
   <Style x:Key="AuthorGroupHeaderContainerStyle" TargetType="ListViewHeaderItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    <Style x:Key="ZoomedOutAuthorItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    ...

    <SemanticZoom x:Name="narrowSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <ListView
            ...
                <ListView.GroupStyle>
                    <GroupStyle
                    ...
                    HeaderContainerStyle="{StaticResource AuthorGroupHeaderContainerStyle}"
                    ...
        <SemanticZoom.ZoomedOutView>
            <ListView
                ...
                ItemContainerStyle="{StaticResource ZoomedOutAuthorItemContainerStyle}"
                ...
```

Une fois cette séquence d’opérations de stylisation effectuée, l’application ressemble à ceci.

![Application Windows 10 portée, exécutée sur un appareil de bureau (vue avec zoom avant et deux tailles de fenêtres)](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil de bureau, une vue zoomée, deux tailles de fenêtre  
 ![ l’application Windows 10 portée exécutée sur un appareil de bureau, une vue zoomée, deux tailles de fenêtre](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

Application Windows 10 portée, exécutée sur un appareil de bureau (vue avec zoom arrière et deux tailles de fenêtres)

![Application Windows 10 portée, exécutée sur un appareil mobile (vue avec zoom avant)](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil mobile, vue zoomée

![Application Windows 10 portée, exécutée sur un appareil mobile (vue avec zoom arrière)](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

L’application Windows 10 portée s’exécutant sur un appareil mobile, vue zoomée

## <a name="making-the-view-model-more-flexible"></a>Optimisation de la flexibilité du modèle d’affichage

Cette section contient un exemple illustrant les fonctions qui s’offrent à nous suite au déplacement de notre application dans le but d’utiliser UWP. Nous décrivons ici certaines étapes facultatives que vous pouvez effectuer pour optimiser la flexibilité de votre modèle d’affichage en cas d’accès par le biais d’un élément **CollectionViewSource**. Le modèle de vue (le fichier source est dans ViewModel \\ BookstoreViewModel.cs) que nous avons reportés à partir de l’application Windows Phone Silverlight Bookstore2WPSL8 contient une classe nommée Author, qui dérive de **List &lt; t &gt; **, où **T** est BookSku. Cela signifie que la classe Author *est* un groupe associé à BookSku.

Lorsque nous lions l’élément **CollectionViewSource.Source** à « Authors », nous signalons simplement que chaque auteur de la liste d’auteurs est un groupe d’*éléments quelconques*. Nous laissons à l’élément **CollectionViewSource** le soin de déterminer que la classe Author est, en l’occurrence, un groupe associé à BookSku. Cela fonctionne, mais peut s’avérer rigide. Que se passe-t-il si nous voulons que la classe Author corresponde *aussi bien* à un groupe de BookSku *qu’à* un groupe d’adresses géographiques correspondant aux lieux où l’auteur a vécu ? La classe Author ne peut pas *correspondre* à ces deux groupes. En revanche, elle peut *inclure* autant de groupes que vous le souhaitez. La solution est là : utilisons le modèle *has-a-group* à la place (ou en plus) du modèle *is-a-group* que nous avons appliqué jusqu’à présent. Voici comment procéder :

-   Modifiez la classe Author afin qu’elle ne dérive plus de l’élément **List&lt;T&gt;**.
-   Ajoutez ce champ à 
-   Ajoutez cette propriété à 
-   Bien entendu, nous pouvons répéter ces deux étapes de manière à ajouter autant de groupes que nous le voulons.
-   Remplacez l’implémentation de la méthode AddBookSku par `this.BookSkus.Add(bookSku);`.
-   Maintenant que la classe Author *inclut* au moins un groupe, nous devons indiquer à l’élément **CollectionViewSource** quel groupe utiliser. Pour ce faire, ajoutez cette propriété à **CollectionViewSource**: `ItemsPath="BookSkus"`

Ces modifications ne touchent pas les fonctionnalités de l’application, mais vous savez désormais comment étendre la classe Author et l’élément **CollectionViewSource**, si nécessaire. Il nous faut apporter une dernière modification à la classe Author, afin qu’un groupe par défaut (défini par nos soins) soit utilisé lorsque nous tirons parti de cette classe *sans* indiquer l’élément **CollectionViewSource.ItemsPath** :

```csharp
    public class Author : IEnumerable<BookSku>
    {
        ...

        public IEnumerator<BookSku> GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
    }
```

Nous pouvons désormais décider de supprimer l’élément `ItemsPath="BookSkus"` si nous le souhaitons : l’application continuera de se comporter comme d’habitude.

## <a name="conclusion"></a>Conclusion

Cette étude de cas reposait sur une interface utilisateur plus ambitieuse que celle de l’étude précédente. L’ensemble des fonctions et concepts de l’élément  **LongListSelector** de l’application Silverlight pour Windows Phone et d’autres informations utiles se sont révélés disponibles pour une application UWP, sous la forme d’éléments **SemanticZoom**, **ListView**, **GridView** et **CollectionViewSource**. Nous vous avons montré comment réutiliser (ou copier et modifier) le code impératif et le balisage dans une application UWP, afin d’obtenir les fonctionnalités, l’interface utilisateur et les interactions adaptées à tous les facteurs de forme des appareils Windows, des plus étroits aux plus larges, en passant par toutes les tailles intermédiaires.
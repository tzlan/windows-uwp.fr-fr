---
description: Découvrez comment utiliser des commandes contextuelles pour implémenter ces types d’actions d’une manière qui optimise l’utilisation de tous les types d’entrée.
title: Commandes contextuelles
ms.assetid: ''
label: Contextual commanding in collections
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 56c845186c9e5351abefd4a6d53175e43b11ae58
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217532"
---
# <a name="contextual-commanding-for-collections-and-lists"></a>Commandes contextuelles pour les collections et les listes



De nombreuses applications contiennent des collections de contenu sous forme de listes, de grilles et d’arborescences, que les utilisateurs peuvent manipuler. Les utilisateurs peuvent par exemple supprimer, renommer, marquer ou actualiser des éléments. Cet article vous montre comment utiliser des commandes contextuelles pour implémenter ces types d’actions d’une manière qui optimise l’utilisation de tous les types d’entrée.  

> **API importantes** : [Interface ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand), [propriété UIElement.ContextFlyout](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), [interface INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![Utiliser différentes entrées pour exécuter la commande FavoriteCommand](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>Création de commandes pour tous les types d’entrée

Étant donné que les utilisateurs peuvent interagir avec une application Windows via [un large choix d’appareils et d’entrées](../devices/index.md), votre application doit exposer des commandes via des menus contextuels indépendants de l’entrée et des accélérateurs propres à l’entrée. Le fait d’intégrer les deux permet à l’utilisateur d’appeler des commandes dans le contenu, quel que soit le type d’entrée ou d’appareil.

Ce tableau présente certaines commandes et certains modes de collection standard permettant d’exposer ces commandes. 

| Commande          | Indépendant de l’entrée | Accélérateur souris | Accélérateur clavier | Accélérateur tactile |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Supprimer l’élément      | Menu contextuel   | Bouton sensitif      | Touche Suppr              | Balayer pour supprimer   |
| Marquer l’élément        | Menu contextuel   | Bouton sensitif      | Ctrl + Maj + G         | Balayer pour marquer     |
| Actualiser les données     | Menu contextuel   | NON APPLICABLE               | Touche F5               | Tirer pour actualiser   |
| Mettre un élément en favori | Menu contextuel   | Bouton sensitif      | F, Ctrl + S            | Balayer pour mettre en favori |


* **En règle générale, vous devez rendre disponibles toutes les commandes d’un élément dans le [menu contextuel](menus.md) de cet élément.** Les menus contextuels sont accessibles aux utilisateurs, quel que soit le type d’entrée, et ils doivent contenir toutes les commandes contextuelles que l’utilisateur peut exécuter.

* **Pour les commandes fréquemment utilisées, il est recommandé d’utiliser des accélérateurs d’entrée.** Les accélérateurs d’entrée permettent à l’utilisateur d’effectuer des actions rapidement, selon leur appareil d’entrée. Voici des exemples d’accélérateurs d’entrée :
    - Balayer pour effectuer une action (accélérateur tactile)
    - Tirer pour actualiser les données (accélérateur tactile)
    - Raccourcis clavier (accélérateur clavier)
    - Touches d’accès rapide (accélérateur clavier)
    - Boutons sensitifs de la souris et du stylet (accélérateur de type pointeur)

> [!NOTE]
> Les utilisateurs doivent pouvoir accéder à toutes les commandes à partir de n’importe quel type d’appareil. Par exemple, si les commandes de votre application sont uniquement exposées via des accélérateurs de type pointeur (en pointant sur les boutons), les utilisateurs tactiles ne pourront pas y accéder. Utilisez au moins un menu contextuel pour donner accès à toutes les commandes.  

## <a name="example-the-podcastobject-data-model"></a>Exemple : le modèle de données PodcastObject

Pour illustrer nos recommandations concernant les commandes, cet article crée une liste de podcasts pour une application de podcast. L’exemple de code montre comment permettre à l’utilisateur de « mettre en favori » un podcast dans une liste.

Voici la définition de l’objet de podcast avec lequel nous allons travailler : 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

Notez que le PodcastObject implémente [INotifyPropertyChanged](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) pour répondre aux modifications de propriété lorsque l’utilisateur active ou désactive la propriété IsFavorite.

## <a name="defining-commands-with-the-icommand-interface"></a>Définition de commandes avec l’interface ICommand

L’[interface ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand) vous permet de définir une commande disponible pour plusieurs types d’entrée. Par exemple, au lieu d’écrire le même code pour une commande de suppression dans deux gestionnaires d’événements différents lorsque l’utilisateur appuie sur la touche Suppr et lorsque l’utilisateur clique avec le bouton droit sur « Supprimer » dans un menu contextuel, vous pouvez implémenter votre logique de suppression une seule fois à l’aide d’une [ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand), puis la rendre disponible à différents types d’entrée.

Nous devons définir l’ICommand qui représente l’action « Mettre en favori ». Nous allons utiliser la méthode [Execute](/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute) de la commande pour mettre un podcast en favori. Ce podcast sera fourni à la méthode d’exécution via le paramètre de la commande, qu’il est possible de lier à l’aide de la propriété CommandParameter.

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

Pour utiliser la même commande avec plusieurs collections et éléments, vous pouvez stocker la commande en tant que ressource dans la page ou sur l’application.

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

Pour exécuter la commande, vous devez appeler sa méthode [Execute](/uwp/api/Windows.UI.Xaml.Input.ICommand.Execute).

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>Création d’un UserControl pour répondre à différentes entrées

Lorsque vous avez une liste d’éléments et que chacun de ces éléments doit répondre à plusieurs entrées, vous pouvez simplifier votre code en définissant un [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl) pour l’élément et l’utiliser pour définir le menu et les gestionnaires d’événements de vos éléments. 

Pour créer un UserControl dans Visual Studio :
1. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet. Un menu contextuel s’affiche.
2. Sélectionnez **Ajouter > Nouvel élément…** <br />La boîte de dialogue **Ajouter un nouvel élément** s’affiche. 
3. Sélectionnez UserControl dans la liste des éléments. Donnez-lui le nom que vous souhaitez, puis cliquez sur **Ajouter**. Visual Studio va générer un stub UserControl automatiquement. 

Dans notre exemple, chaque podcast s’affichera dans une liste, qui exposera les différentes manières de mettre un podcast en favori. L’utilisateur pourra effectuer les actions suivantes pour mettre le podcast en favori :
- Appeler un menu contextuel
- Utiliser des raccourcis clavier
- Afficher un bouton sensitif
- Effectuer un mouvement de balayage

Afin d’encapsuler ces comportements et d’utiliser la commande FavoriteCommand, nous allons créer un nouvel objet [UserControl](/uwp/api/Windows.UI.Xaml.Controls.UserControl) nommé « PodcastUserControl » pour représenter un podcast dans la liste.

Le PodcastUserControl affiche les champs du PodcastObject sous forme de TextBlocks et répond aux différentes interactions avec l’utilisateur. Le PodcastUserControl sera abordé en détail tout au long de cet article.

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

Notez que le PodcastUserControl conserve une référence au PodcastObject sous forme de DependencyProperty. Cela nous permet de lier les PodcastObjects au PodcastUserControl.

Une fois que vous avez généré certains PodcastObjects, vous pouvez créer une liste de podcasts en liant les PodcastObjects à un contrôle ListView. Comme les objets PodcastUserControl décrivent la visualisation des PodcastObjects, ils sont définis via la propriété ItemTemplate du contrôle ListView.

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>Création de menus contextuels

Les menus contextuels affichent une liste de commandes ou d’options lorsque l’utilisateur les demande. Les menus contextuels fournissent des commandes contextuelles liées à leur élément joint. Ils sont généralement réservés aux actions secondaires propres à cet élément.

![Afficher un menu contextuel sur l’élément](images/ContextualCommand_RightClick.png)

L’utilisateur peut appeler des menus contextuels à l’aide de ces « actions contextuelles » :

| Entrée    | Action contextuelle                          |
| -------- | --------------------------------------- |
| Souris    | Clic droit                             |
| Clavier | Maj + F10, touche Menu                  |
| Toucher    | Appui long sur l’élément                      |
| Stylet      | Appui sur le bouton du stylet, appui long sur l’élément |
| Boîtier de commande  | Bouton Menu                             |

**Dans la mesure où l’utilisateur peut ouvrir un menu contextuel indépendamment du type d’entrée, votre menu contextuel doit contenir toutes les commandes contextuelles disponibles pour l’élément de liste.**

### <a name="contextflyout"></a>ContextFlyout

La [propriété ContextFlyout](/uwp/api/Windows.UI.Xaml.UIElement.ContextFlyout), définie par la classe UIElement, facilite la création d’un menu contextuel qui fonctionne avec tous les types d’entrée. Vous fournissez un menu volant représentant votre menu contextuel à l’aide du contrôle MenuFlyout. Lorsque l’utilisateur effectuera une « action contextuelle », comme indiqué ci-dessus, le contrôle MenuFlyout correspondant à l’élément s’affichera.

Nous allons ajouter un ContextFlyout au PodcastUserControl. Le MenuFlyout spécifié comme ContextFlyout contient un seul élément pour mettre un podcast en favori. Notez que ce MenuFlyoutItem utilise la commande favoriteCommand définie ci-dessus, avec le CommandParamter lié au PodcastObject.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

Notez que vous pouvez également utiliser l’[événement ContextRequested](/uwp/api/Windows.UI.Xaml.UIElement.ContextRequested) pour répondre aux actions contextuelles. L’événement ContextRequested ne se déclenchera pas si un ContextFlyout a été spécifié.

## <a name="creating-input-accelerators"></a>Création d’accélérateurs d’entrée

Même si chaque élément de la collection doit disposer d’un menu contextuel contenant toutes les commandes contextuelles, vous souhaiterez peut-être permettre aux utilisateurs d’accéder rapidement à un ensemble plus réduit de commandes fréquemment exécutées. Par exemple, une application de messagerie peut présenter des commandes secondaires comme Répondre, Archiver, Déplacer vers un dossier, Marquer d’un drapeau et Supprimer, qui s’affichent dans un menu contextuel, tandis que les commandes les plus courantes sont Supprimer et Marquer d’un drapeau. Une fois que vous avez identifié les commandes les plus courantes, vous pouvez utiliser des accélérateurs d’entrée pour faciliter l’exécution de ces commandes par les utilisateurs.

Dans l’application de podcast, la commande fréquemment exécutée est la commande « Mettre en favori ».

### <a name="keyboard-accelerators"></a>Raccourcis clavier

#### <a name="shortcuts-and-direct-key-handling"></a>Gestion des raccourcis et des touches directes

![Appuyez sur Ctrl et F pour effectuer une action](images/ContextualCommand_Keyboard.png)

Selon le type de contenu, vous pouvez identifier certaines combinaisons de touches qui doivent effectuer une action. Dans une application de messagerie, par exemple, il est possible d’utiliser la touche Suppr pour supprimer l’e-mail sélectionné. Dans une application de podcast, les touches Ctrl + S ou F peuvent permettre de mettre un podcast en favori pour le retrouver ultérieurement. Bien que certaines commandes présentent des raccourcis clavier courants, comme Suppr pour supprimer, d’autres commandes présentent des raccourcis propres à l’application ou au domaine. Si possible, utilisez des raccourcis bien connus. Sinon, pensez à fournir un texte de rappel dans une info-bulle pour former l’utilisateur à la commande de raccourci.

Vous pouvez utiliser l’événement [KeyDown](/uwp/api/Windows.UI.Xaml.UIElement.KeyDownEvent) pour permettre à votre application de répondre lorsque l’utilisateur appuie sur une touche. En général, les utilisateurs s’attendent à ce que l’application réponde lorsqu’ils appuient sur la touche, et non lorsqu’ils la relâchent.

Cet exemple montre comment ajouter le gestionnaire KeyDown au PodcastUserControl pour mettre un podcast en favori lorsque l’utilisateur appuie sur Ctrl + S ou F. Il utilise la même commande qu’avant.

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>Accélérateurs souris

![Pointer la souris sur un élément pour faire apparaître un bouton](images/ContextualCommand_HovertoReveal.png)

Les utilisateurs sont habitués aux menus contextuels accessibles d’un clic droit, mais vous souhaiterez peut-être leur permettre d’exécuter des commandes courantes à l’aide d’un simple clic. Pour permettre cela, vous pouvez intégrer des boutons dédiés au canevas des éléments de votre collection. Pour permettre aux utilisateurs d’agir rapidement à l’aide d’une souris, tout en réduisant l’encombrement visuel, vous pouvez choisir de n’afficher ces boutons que lorsque l’utilisateur passe son pointeur sur un élément de liste particulier.

Dans cet exemple, la commande de mise en favori est représentée par un bouton défini directement dans le PodcastUserControl. Notez que le bouton présenté dans cet exemple utilise la même commande, FavoriteCommand, qu’auparavant. Afin d’activer la visibilité de ce bouton, vous pouvez utiliser le VisualStateManager pour basculer entre les états visuels lorsque le pointeur rentre et sort du contrôle.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

Les boutons sensitifs doivent apparaître et disparaître lorsque la souris entre et quitte l’élément. Pour répondre aux événements de souris, vous pouvez utiliser les événements [PointerEntered](/uwp/api/Windows.UI.Xaml.UIElement.PointerEnteredEvent) et [PointerExited](/uwp/api/Windows.UI.Xaml.UIElement.PointerExitedEvent) sur le PodcastUserControl.

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

Les boutons qui présentent un état de pointage ne seront accessibles qu’en utilisant un pointeur comme type d’entrée. Dans la mesure où ces boutons sont réservés à l’entrée de type pointeur, vous pouvez choisir de réduire ou de supprimer le remplissage qui entoure l’icône du bouton pour optimiser l’entrée via le pointeur. Si vous choisissez de faire cela, vérifiez que la taille du bouton est au moins de 20 x 20 px pour qu’il reste utilisable avec un stylet et une souris.

### <a name="touch-accelerators"></a>Accélérateurs tactiles

#### <a name="swipe"></a>Balayer

![Balayer un élément pour afficher la commande](images/ContextualCommand_Swipe.png)

La commande par balayage est un accélérateur tactile qui permet aux utilisateurs d’appareils tactiles d’utiliser des commandes tactiles pour réaliser des actions secondaires courantes. Le balayage tactile permet aux utilisateurs tactiles d’interagir rapidement et naturellement avec du contenu, à l’aide d’actions courantes telles que le balayage pour supprimer ou le balayage pour appeler. Pour plus d’infirmations, consultez l’article dédié aux [commandes par balayage](swipe.md).

Pour intégrer le balayage à votre collection, vous avez besoin de deux composants : SwipeItems, qui héberge les commandes, et SwipeControl, qui wrappe l’élément et permet une interaction par balayage.

SwipeItems peut être défini en tant que ressource dans le PodcastUserControl. Dans cet exemple, SwipeItems contient une commande permettant de mettre un élément en favori.

```xaml
<UserControl.Resources>
    <SymbolIconSource x:Key="FavoriteIcon" Symbol="Favorite"/>
    <SwipeItems x:Key="RevealOtherCommands" Mode="Reveal">
        <SwipeItem IconSource="{StaticResource FavoriteIcon}" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
    </SwipeItems>
</UserControl.Resources>
```

SwipeControl wrappe l’élément et permet à l’utilisateur d’interagir avec celui-ci à l’aide d’un mouvement de balayage. Notez que SwipeControl contient une référence à SwipeItems en tant que RightItems. L’élément favori s’affichera lorsque l’utilisateur effectuera un balayage de droite à gauche.

```xaml
<SwipeControl x:Name="swipeContainer" RightItems="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeControl, since the SwipeControl wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</SwipeControl>
```

Lorsque l’utilisateur effectue un balayage pour appeler la commande de mise en favori, la méthode Invoked est appelée.

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>Tirer pour actualiser

La commande Tirer pour actualiser permet à l’utilisateur de tirer (pull) un ensemble de données à l’aide de la fonction tactile, afin de récupérer des données supplémentaires. Pour plus d’informations, consultez l’article dédié à la commande [Tirer pour actualiser](pull-to-refresh.md).

### <a name="pen-accelerators"></a>Accélérateurs stylet

L’entrée de type stylet offre la précision d’un pointeur. Les utilisateurs peuvent utiliser des accélérateurs basés sur stylet pour effectuer des actions courantes telles que l’ouverture de menus contextuels. Pour ouvrir un menu contextuel, les utilisateurs peuvent appuyer sur l’écran tout en maintenant le bouton du stylet enfoncé ou appuyer longuement sur le contenu. Les utilisateurs peuvent également utiliser le stylet pour pointer sur le contenu afin d’obtenir une meilleure compréhension de l’interface utilisateur, en affichant des info-bulles ou en faisant apparaître des actions de pointage secondaires, comme avec une souris.

Afin d’optimiser votre application pour une entrée à l’aide d’un stylet, consultez l’article dédié aux [interactions avec un stylo et un stylet](../input/pen-and-stylus-interactions.md).


## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

* Vérifiez que les utilisateurs peuvent accéder à toutes les commandes à partir de n’importe quel type d’appareil Windows.
* Intégrez un menu contextuel donnant accès à toutes les commandes disponibles pour un élément de collection. 
* Fournissez des accélérateurs d’entrée pour les commandes fréquemment utilisées. 
* Utilisez l’[interface ICommand](/uwp/api/Windows.UI.Xaml.Input.ICommand) pour implémenter des commandes. 

## <a name="related-topics"></a>Rubriques connexes
* [ICommand, interface](/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [Menus et menus contextuels](menus.md)
* [Balayer](swipe.md)
* [Tirer pour actualiser](pull-to-refresh.md)
* [Interactions avec le stylo et le stylet](../input/pen-and-stylus-interactions.md)
* [Personnaliser votre application pour une utilisation avec un boîtier de commande et une Xbox](../devices/designing-for-tv.md)

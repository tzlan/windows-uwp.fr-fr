---
description: Ajoutez un InkToolbar par défaut à une application d’écriture manuscrite Windows, ajoutez un bouton stylet personnalisé au InkToolbar et liez le bouton stylet personnalisé à une définition de stylet personnalisée.
title: Ajouter un élément InkToolbar à une application Windows
label: Add an InkToolbar to a Windows app
template: detail.hbs
keywords: Windows Ink, entrée manuscrite Windows Ink, DirectInk, InkPresenter, InkCanvas, InkToolbar, plateforme Windows universelle, UWP, interaction utilisateur, entrée
ms.date: 09/24/2020
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: 78585f9734131531db5cfa429770ed8351459d8f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030202"
---
# <a name="add-an-inktoolbar-to-a-windows-app"></a>Ajouter un élément InkToolbar à une application Windows



Il existe deux contrôles différents qui facilitent l’écriture dans des applications Windows : [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) et [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar).

Le contrôle [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) fournit les fonctionnalités Windows Ink de base. Utilisez-le pour restituer une entrée de stylet sous la forme d’un trait d’encre (via les paramètres par défaut de couleur et d’épaisseur) ou d’un trait d’effacement.

> Pour plus d’informations sur l’implémentation d’InkCanvas, consultez [interactions du stylet et du stylet dans les applications Windows](pen-and-stylus-interactions.md).

En tant que superposition totalement transparente, le contrôle InkCanvas ne fournit pas d’interface utilisateur intégrée pour configurer les propriétés relatives aux traits d’encre. Si vous souhaitez modifier l’expérience d’entrée manuscrite par défaut, permettre aux utilisateurs de configurer les propriétés relatives aux traits d’encre et prendre en charge d’autres fonctionnalités d’entrée manuscrite personnalisées, deux options s’offrent à vous :

- Dans le code-behind, utilisez l’objet [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) sous-jacent lié à InkCanvas.

  Les API InkPresenter prennent en charge une personnalisation complète de l’expérience d’entrée manuscrite. Pour plus d’informations, consultez [interactions du stylet et du stylet dans les applications Windows](pen-and-stylus-interactions.md).

- Liez un [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) à InkCanvas. Par défaut, le InkToolbar fournit une collection personnalisable et extensible de boutons permettant d’activer les fonctionnalités liées à l’encre, telles que la taille du trait, la couleur de l’encre et la pointe du stylet.

  Cette rubrique est dédiée au contrôle InkToolbar.

> **API importantes** : [**classe InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas), [**classe InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar), classe [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter), [**Windows. UI. Input. encrage**](/uwp/api/Windows.UI.Input.Inking)

## <a name="default-inktoolbar"></a>Contrôle InkToolbar par défaut

Par défaut, le [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) comprend des boutons de dessin, d’effacement, de mise en surbrillance et d’affichage d’un gabarit (règle ou plan). Selon la fonctionnalité, d’autres paramètres et commandes tels que la couleur de l’encre, l’épaisseur du trait, la suppression totale, sont fournis dans un menu volant.

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*Barre d’outils Windows Ink par défaut*

Pour ajouter un [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) par défaut à une application d’encrage, placez-le simplement sur la même page que votre [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) et associez les deux contrôles.

1. Dans MainPage.xaml, déclarez un objet de conteneur (ici, nous utilisons un contrôle Grid) pour la surface d’entrée manuscrite.
2. Déclarez un objet InkCanvas en tant qu’enfant du conteneur. (La taille du contrôle InkCanvas est héritée du conteneur).
3. Déclarer un contrôle InkToolbar et utilisez l’attribut TargetInkCanvas pour le lier au contrôle InkCanvas.

> [!NOTE]
> Vérifiez que le contrôle InkToolbar est déclaré après le contrôle InkCanvas. Si ce n’est pas le cas, la superposition du contrôle InkCanvas rend inaccessible le contrôle InkToolbar.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>Personnalisation de base

Dans cette section, nous allons aborder quelques scénarios de personnalisation de barre d’outils Windows Ink de base.

### <a name="specify-location-and-orientation"></a>Spécifier l’emplacement et l’orientation

Quand vous ajoutez une barre d’outils Ink à votre application, vous pouvez accepter l’emplacement par défaut et ORIENTAION de la barre d’outils ou les définir comme requis par votre application ou utilisateur.

**XAML**

Spécifiez explicitement l’emplacement et l’orientation de la barre d’outils par le biais de ses propriétés [VerticalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment), [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment)et [orientation](/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation) .

| Default | Explicite |
| --- | --- |
| ![Emplacement et orientation de la barre d’outils d’encre par défaut](./images/ink/location-default-small.png) | ![Emplacement et orientation de la barre d’outils encre explicite](./images/ink/location-explicit-small.png) |
| *Emplacement et orientation par défaut de la barre d’outils Windows Ink* | *Emplacement et orientation explicites de la barre d’outils Windows Ink* |

Voici le code permettant de définir explicitement l’emplacement et l’orientation de la barre d’outils Ink en XAML.
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**Initialiser en fonction des préférences de l’utilisateur ou de l’état de l’appareil**

Dans certains cas, vous souhaiterez peut-être définir l’emplacement et l’orientation de la barre d’outils encre en fonction de la préférence de l’utilisateur ou de l’état de l’appareil. L’exemple suivant montre comment définir l’emplacement et l’orientation de la barre d’outils d’encre en fonction des préférences d’écriture à gauche ou à droite spécifiées par le biais de **paramètres > appareils > pen & stylet > stylet Windows ink > choisir la main avec laquelle vous écrivez** .

![Paramètre main dominant](./images/ink/location-handedness-setting.png)  
*Paramètre main dominant*

Vous pouvez interroger ce paramètre à l’aide de la propriété HandPreference de Windows. UI. ViewManagement et définir l' [alignement horizontal](/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) sur la base de la valeur retournée. Dans cet exemple, nous localisons la barre d’outils sur le côté gauche de l’application pour une personne gauche et sur le côté droit pour une personne droitier.

**Télécharger cet exemple à partir de la [barre d’outils de l’encre emplacement et orientation, exemple (de base)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

```csharp
public MainPage()
{
    this.InitializeComponent();

    Windows.UI.ViewManagement.UISettings settings = 
        new Windows.UI.ViewManagement.UISettings();
    HorizontalAlignment alignment = 
        (settings.HandPreference == 
            Windows.UI.ViewManagement.HandPreference.LeftHanded) ? 
            HorizontalAlignment.Left : HorizontalAlignment.Right;
    inkToolbar.HorizontalAlignment = alignment;
}
```

**Ajuster dynamiquement à l’état de l’utilisateur ou du périphérique**

Vous pouvez également utiliser la liaison pour consulter les mises à jour de l’interface utilisateur en fonction des modifications apportées aux préférences de l’utilisateur, aux paramètres de l’appareil ou aux États des appareils. Dans l’exemple suivant, nous développons l’exemple précédent et montrons comment positionner dynamiquement la barre d’outils d’encre en fonction de l’orientation de l’appareil à l’aide de la liaison, d’un objet ViewMOdel et de l’interface [INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) . 

**Télécharger cet exemple à partir de la [barre d’outils Ink emplacement et orientation, exemple (dynamique)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)**

1. Tout d’abord, nous allons ajouter notre ViewModel.
    1. Ajoutez un nouveau dossier à votre projet et appelez-le **ViewModels** .
    1. Ajoutez une nouvelle classe au dossier ViewModels (pour cet exemple, nous l’avons appelée **InkToolbarSnippetHostViewModel.cs** ).
        > [!NOTE] 
        > Nous avons utilisé le [modèle Singleton](/previous-versions/msp-n-p/ff650849(v=pandp.10)) , car nous n’avons besoin que d’un seul objet de ce type pendant la durée de vie de l’application

    1. Ajoutez `using System.ComponentModel` l’espace de noms au fichier.
    1. Ajoutez une variable membre statique appelée **instance** et une propriété en lecture seule statique nommée **instance** . Rendez le constructeur privé pour vérifier que cette classe n’est accessible qu’à l’aide de la propriété d’instance.   
        > [!NOTE] 
        > Cette classe hérite de l’interface [INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) , qui est utilisée pour informer les clients, généralement les clients de liaison, qu’une valeur de propriété a été modifiée. Nous allons l’utiliser pour gérer les modifications apportées à l’orientation de l’appareil (nous allons développer ce code et expliquer plus en détail dans une étape ultérieure).  

        ```csharp
        using System.ComponentModel;

        namespace locationandorientation.ViewModels
        {
            public class InkToolbarSnippetHostViewModel : INotifyPropertyChanged
            {
                private static InkToolbarSnippetHostViewModel instance;
                
                public static InkToolbarSnippetHostViewModel Instance
                {
                    get
                    {
                        if (null == instance)
                        {
                            instance = new InkToolbarSnippetHostViewModel();
                        }
                        return instance;
                    }
                }
            }

            private InkToolbarSnippetHostViewModel() { }
        }
        ```

    1. Ajoutez deux propriétés bool à la classe InkToolbarSnippetHostViewModel : **LeftHandedLayout** (même fonctionnalité que l’exemple précédent XAML uniquement) et **PortraitLayout** (orientation de l’appareil).
        >[!NOTE] 
        > La propriété PortraitLayout peut être définie et comprend la définition de l’événement [propertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) .

        ```csharp
        public bool LeftHandedLayout
        {
            get
            {
                bool leftHandedLayout = false;
                Windows.UI.ViewManagement.UISettings settings =
                    new Windows.UI.ViewManagement.UISettings();
                leftHandedLayout = (settings.HandPreference ==
                    Windows.UI.ViewManagement.HandPreference.LeftHanded);
                return leftHandedLayout;
            }
        }

        public bool portraitLayout = false;
        public bool PortraitLayout
        {
            get
            {
                Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                    Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
                portraitLayout = 
                    (winOrientation == 
                        Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
                return portraitLayout;
            }
            set
            {
                if (value.Equals(portraitLayout)) return;
                portraitLayout = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
            }
        }
        ```

1. À présent, nous allons ajouter deux classes de convertisseur à notre projet. Chaque classe contient un objet Convert qui retourne une valeur d’alignement ( [HorizontalAlignment](/uwp/api/windows.ui.xaml.horizontalalignment) ou [VerticalAlignment](/uwp/api/windows.ui.xaml.verticalalignment)).
    1. Ajoutez un nouveau dossier à votre projet et appelez-le **convertisseurs** .
    1. Ajoutez deux nouvelles classes au dossier convertisseurs (pour cet exemple, nous les appelons **HorizontalAlignmentFromHandednessConverter.cs** et **VerticalAlignmentFromAppViewConverter.cs** ).
    1. Ajoutez `using Windows.UI.Xaml` des `using Windows.UI.Xaml.Data` espaces de noms et à chaque fichier.
    1. Remplacez chaque classe par `public` et spécifiez qu’elle implémente l’interface [IValueConverter](/uwp/api/windows.ui.xaml.data.ivalueconverter) .
    1. Ajoutez les méthodes [Convert](/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) et [ConvertBack](/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) à chaque fichier, comme indiqué ici (nous laissons la méthode ConvertBack non implémentée).
        - HorizontalAlignmentFromHandednessConverter positionne la barre d’outils de l’encre sur le côté droit de l’application pour les utilisateurs droitiers et sur le côté gauche de l’application pour les utilisateurs de gauche.
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class HorizontalAlignmentFromHandednessConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool leftHanded = (bool)value;
                    HorizontalAlignment alignment = HorizontalAlignment.Right;
                    if (leftHanded)
                    {
                        alignment = HorizontalAlignment.Left;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

        - VerticalAlignmentFromAppViewConverter positionne la barre d’outils de l’encre au centre de l’application pour l’orientation portrait et vers la partie supérieure de l’application pour l’orientation paysage (tout en étant conçu pour améliorer la convivialité, il s’agit simplement d’un choix arbitraire à des fins de démonstration).
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class VerticalAlignmentFromAppViewConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool portraitOrientation = (bool)value;
                    VerticalAlignment alignment = VerticalAlignment.Top;
                    if (portraitOrientation)
                    {
                        alignment = VerticalAlignment.Center;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1. À présent, ouvrez le fichier MainPage.xaml.cs.
    1. Ajoutez `using using locationandorientation.ViewModels` à la liste d’espaces de noms pour associer notre ViewModel.
    1. Ajoutez `using Windows.UI.ViewManagement` à la liste des espaces de noms pour activer l’écoute des modifications apportées à l’orientation de l’appareil.
    1. Ajoutez le code [WindowSizeChangedEventHandler](/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) .
    1. Définissez le [DataContext](/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) pour la vue sur l’instance singleton de la classe InkToolbarSnippetHostViewModel. 
    ```csharp
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;

    using locationandorientation.ViewModels;
    using Windows.UI.ViewManagement;

    namespace locationandorientation
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();

                Window.Current.SizeChanged += (sender, args) =>
                {
                    ApplicationView currentView = ApplicationView.GetForCurrentView();

                    if (currentView.Orientation == ApplicationViewOrientation.Landscape)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = false;
                    }
                    else if (currentView.Orientation == ApplicationViewOrientation.Portrait)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = true;
                    }
                };

                DataContext = InkToolbarSnippetHostViewModel.Instance;
            }
        }
    }
    ```

1. Ensuite, ouvrez le fichier MainPage. Xaml.
    1. Ajoutez `xmlns:converters="using:locationandorientation.Converters"` à l' `Page` élément pour la liaison à nos convertisseurs.
        ```xaml
        <Page
        x:Class="locationandorientation.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:locationandorientation"
        xmlns:converters="using:locationandorientation.Converters"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        ```

    1. Ajoutez un `PageResources` élément et spécifiez des références à nos convertisseurs.
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. Ajoutez les éléments InkCanvas et InkToolbar et liez les propriétés VerticalAlignment et HorizontalAlignment du InkToolbar.
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. Revenez au fichier InkToolbarSnippetHostViewModel.cs pour ajouter les `PortraitLayout` `LeftHandedLayout` Propriétés et bool à la `InkToolbarSnippetHostViewModel` classe, ainsi que la prise en charge de la reliaison `PortraitLayout` lorsque cette valeur de propriété change. 
    ```csharp
    public bool LeftHandedLayout
    {
        get
        {
            bool leftHandedLayout = false;
            Windows.UI.ViewManagement.UISettings settings =
                new Windows.UI.ViewManagement.UISettings();
            leftHandedLayout = (settings.HandPreference ==
                Windows.UI.ViewManagement.HandPreference.LeftHanded);
            return leftHandedLayout;
        }
    }

    public bool portraitLayout = false;
    public bool PortraitLayout
    {
        get
        {
            Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            portraitLayout = 
                (winOrientation == 
                    Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
            return portraitLayout;
        }
        set
        {
            if (value.Equals(portraitLayout)) return;
            portraitLayout = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
        }
    }

    #region INotifyPropertyChanged Members

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }

    #endregion
    ```

Vous devez maintenant disposer d’une application d’entrée manuscrite qui s’adapte à la plupart des préférences de la main de l’utilisateur et répond de manière dynamique à l’orientation de l’appareil de l’utilisateur.

### <a name="specify-the-selected-button"></a>Spécifier le bouton sélectionné  
![Bouton de crayon sélectionné lors de l’initialisation](./images/ink/ink-tools-default-toolbar.png)  
*Barre d’outils Windows Ink avec le bouton de crayon sélectionné lors de l’initialisation*

Par défaut, le premier bouton (ou celui le plus à gauche) est sélectionné lorsque votre application est lancée et que la barre d’outils est initialisée. Dans la barre d’outils Windows Ink par défaut, il s’agit du bouton de stylo à bille.

Étant donné que l’infrastructure définit l’ordre des boutons intégrés, le premier bouton peut ne pas être pas le stylet ou l’outil que vous souhaitez activer par défaut.

Vous pouvez remplacer ce comportement par défaut et spécifier le bouton sélectionné dans la barre d’outils.

Ici, nous initialisons la barre d’outils par défaut avec le bouton de crayon sélectionné et le crayon activé (au lieu du stylo à bille).

1. Utilisez la déclaration XAML pour les contrôles InkCanvas et InkToolbar de l’exemple précédent.
2. Dans le code-behind, configurez un gestionnaire pour l’événement [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) de l’objet [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. Dans le gestionnaire pour l’événement [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) :
    1. Obtenez une référence pour l’élément [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) intégré.

    Transmettre un objet [InkToolbarTool.Pencil](/uwp/api/windows.ui.xaml.controls.inktoolbartool) dans une méthode [GetToolButton](/uwp/api/windows.ui.xaml.controls.inktoolbar.gettoolbutton) renvoie un objet [InkToolbarToolButton](/uwp/api/windows.ui.xaml.controls.inktoolbartoolbutton) pour l’élément [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton).

    2. Définissez [ActiveTool](/uwp/api/windows.ui.xaml.controls.inktoolbar.activetool) sur l’objet renvoyé à l’étape précédente.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>Spécifier les boutons intégrés

![Boutons spécifiques inclus lors de l’initialisation](./images/ink/ink-tools-specific.png)  
*Boutons spécifiques inclus lors de l’initialisation*

Comme mentionné précédemment, la barre d’outils Windows Ink comprend une collection de boutons intégrés par défaut. Ces boutons sont affichés dans l’ordre suivant (de gauche à droite) :

- [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)
- [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)
- [InkToolbarHighlighterButton](/uwp/api/windows.ui.xaml.controls.inktoolbarhighlighterbutton)
- [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)
- [InkToolbarRulerButton](/uwp/api/windows.ui.xaml.controls.inktoolbarrulerbutton)

Ici, nous initialisons la barre d’outils avec les boutons intégrés de stylo à bille, crayon et gomme uniquement.

Vous pouvez effectuer cette opération à l’aide de la déclaration XAML ou du code-behind.

**XAML**

Modifiez la déclaration XAML pour les contrôles InkCanvas et InkToolbar du premier exemple.
- Ajouter un attribut [InitialControls](/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols)et définissez sa valeur sur « [None](/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols) ». Cela permet d’effacer la collection de boutons intégrés par défaut.
- Ajoutez les boutons InkToolbar spécifiques requis par votre application. Ici, nous ajoutons uniquement les boutons [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) et [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton).
> [!NOTE]
> Les boutons sont ajoutés à la barre d’outils dans l’ordre défini par votre infrastructure et non dans l’ordre indiqué ici.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**Code-behind**
1. Utilisez la déclaration XAML pour les contrôles InkCanvas et InkToolbar du premier exemple.

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. Dans le code-behind, configurez un gestionnaire pour l’événement [Loading](/uwp/api/windows.ui.xaml.frameworkelement.loading) de l’objet [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. Définissez [InitialControls](/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) sur « [None](/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols) ».
4. Créez des références d’objet pour les boutons requis par votre application. Ici, nous ajoutons uniquement les boutons [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) et [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton).
  > [!NOTE]
  > Les boutons sont ajoutés à la barre d’outils dans l’ordre défini par votre infrastructure et non dans l’ordre indiqué ici.

5. [Ajoutez](/uwp/api/windows.ui.xaml.dependencyobjectcollection.add) les boutons au contrôle InkToolbar.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>Fonctionnalités d’entrée manuscrite et boutons personnalisés

Vous pouvez personnaliser et étendre la collection de boutons (et de fonctionnalités d’entrée manuscrite associées) qui sont fournis via le contrôle InkToolbar.

Le contrôle InkToolbar se compose de deux groupes de types de boutons :

1. Un groupe de boutons « outil » comprenant les boutons intégrés de dessin, d’effacement et de surlignage. Les outils et stylets personnalisés sont ajoutés ici.
> **Note** &nbsp; Remarque &nbsp; La sélection des fonctionnalités s’exclut mutuellement.

2. Un groupe de boutons « bascules » contenant le bouton intégré de règle. Les boutons bascule personnalisés sont ajoutés ici.
> **Note** &nbsp; Remarque &nbsp; Les fonctionnalités ne sont pas mutuellement exclusives et peuvent être utilisées simultanément avec d’autres outils actifs.

En fonction de votre application et de la fonctionnalité d’entrée manuscrite requise, vous pouvez ajouter n’importe lequel des boutons suivants (liés à vos fonctionnalités d’entrée manuscrite personnalisées) au contrôle InkToolbar :

- Stylet personnalisé : un stylet dont les propriétés de palette de couleurs de l’encre et de pointe de stylet, telles que la forme, la rotation et la taille, sont définies par l’application hôte.
- Outil personnalisé : un outil autre qu’un stylet, défini par l’application hôte.
- Bascule personnalisée : définit l’état d’une fonctionnalité définie par l’application sur activé ou désactivé. Lorsqu’elle est activée, la fonctionnalité fonctionne conjointement avec l’outil actif.

> **Note** &nbsp; Remarque &nbsp; Vous ne pouvez pas modifier l’ordre d’affichage des boutons intégrés. L’ordre d’affichage par défaut est : stylo à bille, crayon, surligneur, gomme et règle. Les stylets personnalisés sont ajoutés au dernier stylet par défaut, les boutons d’outil personnalisé sont ajoutés entre le dernier bouton de stylet et le bouton de gomme, et les boutons de bascule personnalisés sont ajoutés après le bouton de règle. (Les boutons personnalisés sont ajoutés dans l’ordre que vous avez spécifié.)

### <a name="custom-pen"></a>Stylet personnalisé

Vous pouvez créer un stylet personnalisé (activé par le biais d’un bouton de stylet personnalisé) dans lequel vous définissez la palette de couleurs d’entrée manuscrite et les propriétés de la pointe du stylet, telles que la forme, la rotation et la taille.

![Bouton de stylo de calligraphie personnalisé](./images/ink/ink-tools-custompen.png)  
*Bouton de stylo de calligraphie personnalisé*

Ici, nous définissons un stylet personnalisé à pointe large qui permet de tracer des traits de calligraphie de base. Nous personnalisons également la collection de pinceaux dans la palette affichée dans le menu volant de bouton.

**Code-behind**

Tout d’abord, nous définissons notre stylet personnalisé et spécifions les attributs de dessin dans le code-behind. Nous ferons référence à ce stylet personnalisé à partir de la déclaration XAML ultérieurement.

1. Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez Ajouter &gt; Nouvel élément.
2. Sous Visual C# -&gt; Code, ajoutez un nouveau fichier de classe et nommez-le CalligraphicPen.cs.
3. Dans le fichier Calligraphic.cs, remplacez le bloc  « using » par défaut par ce qui suit :
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Spécifiez que la classe CalligraphicPen est dérivée de l’élément [InkToolbarCustomPen](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Remplacez  [CreateInkDrawingAttributesCore](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore)  pour spécifier votre propre pinceau et votre propre taille de trait.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Créez un objet [InkDrawingAttributes](/uwp/api/windows.ui.input.inking.inkdrawingattributes) et définissez la [forme de la pointe du stylet](/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentip), la [rotation de la pointe](/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentiptransform), la [taille des traits](/uwp/api/windows.ui.input.inking.inkdrawingattributes.size), et la [couleur de l’encre](/uwp/api/windows.ui.input.inking.inkdrawingattributes.color).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

Ensuite, nous ajoutons les références nécessaires au stylet personnalisé dans le fichier MainPage.xaml.

1. Nous déclarons un dictionnaire de ressources de page local qui crée une référence au stylet personnalisé (`CalligraphicPen`), définie dans le fichier CalligraphicPen.cs, ainsi qu’une [collection de pinceaux](/uwp/api/Windows.UI.Xaml.Media.BrushCollection) prise en charge par le stylet personnalisé (`CalligraphicPenPalette`).
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. Nous ajoutons ensuite un contrôle InkToolbar à un élément [InkToolbarCustomPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompenbutton) enfant.

  Le bouton de stylet personnalisé inclut les deux références de ressources statiques déclarées dans les ressources de la page : `CalligraphicPen` et `CalligraphicPenPalette`.

  Nous spécifions également l’étendue du curseur de taille de trait ([MinStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth), [MaxStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth) et [SelectedStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty)), le pinceau sélectionné ([SelectedBrushIndex](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex)) et l’icône du bouton de stylet personnalisé ([SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon)).
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>Bascule personnalisée

Vous pouvez créer une bascule personnalisée (activée par le biais d’un bouton bascule personnalisé) pour activer ou désactiver une fonction définie par l’application. Lorsqu’elle est activée, la fonctionnalité fonctionne conjointement avec l’outil actif.

Dans cet exemple, nous définissons un bouton bascule personnalisé qui permet l’entrée manuscrite et l’entrée tactile (par défaut, l’entrée manuscrite tactile n’est pas activée).

> [!NOTE]  
> Si vous avez besoin de prendre en charge l’entrée manuscrite avec une interface tactile, il est recommandé de l’activer à l’aide d’un bouton bascule personnalisé, avec l’icône et l’info-bulle spécifiés dans cet exemple.

En règle générale, l’entrée tactile est utilisée pour la manipulation directe d’un objet ou l’interface utilisateur de l’application. Pour illustrer les différences de comportement lorsque l’entrée manuscrite tactile est activée, nous plaçons InkCanvas au sein d’un conteneur ScrollViewer, et nous définissions les dimensions de ScrollViewer de manière à ce qu’elles soient inférieures à celles de InkCanvas. 

Au démarrage de l’application, seule l’entrée manuscrite à l’aide du stylet est prise en charge et la fonction tactile est utilisée pour faire un panoramique ou un zoom sur la surface d’entrée manuscrite. Lorsque l’entrée manuscrite tactile est activée, il n’est pas possible de faire de panoramique ou de zoom sur la surface d’entrée manuscrite par le biais de l’entrée tactile.

> [!NOTE]
> Voir [Contrôles pour l’entrée manuscrite](../controls-and-patterns/inking-controls.md) pour des recommandations en matière d’expérience utilisateur sur [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) et [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar). Les recommandations suivantes sont pertinentes pour cet exemple :
> - Le [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar), et l’entrée manuscrite en général, est mieux expérimenté grâce à un stylet actif. Toutefois, l’entrée manuscrite avec une souris et la fonctionnalité tactile peut être prise en charge si votre application l’impose. 
> - En cas de prise en charge de l’entrée manuscrite avec la fonctionnalité tactile, nous recommandons d’utiliser l’icône « ED5F » de la police « Segoe MLD2 Assets » pour le bouton bascule, avec une info-bulle « écriture tactile ». 

**XAML**

1. Tout d’abord, nous déclarons un élément [**InkToolbarCustomToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) (toggleButton) avec un écouteur d’événements Click qui spécifie le gestionnaire d’événements (Toggle_Custom).

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**Code-behind**

2. Dans l’extrait de code précédent, nous avons déclaré un écouteur d’événements Click et le gestionnaire (Toggle_Custom) sur le bouton bascule personnalisé pour les entrées manuscrites tactiles (toggleButton). Ce gestionnaire bascule simplement la prise en charge de CoreInputDeviceTypes.Touch sur la propriété InputDeviceTypes de InkPresenter.

   Nous avons également spécifié une icône pour le bouton à l’aide de l’élément SymbolIcon et de l’extension de balisage {x:Bind} qui l’associe à un champ défini dans le fichier code-behind (TouchWritingIcon).

   L’extrait de code suivant inclut à la fois le gestionnaire d’événements Click et la définition de TouchWritingIcon.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>Outil personnalisé

Vous pouvez créer un bouton d’outil personnalisé pour appeler un outil autre qu’un stylet défini par votre application.

Par défaut, l' [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) traite toutes les entrées comme un trait d’encre ou un trait d’effacement. Cela inclut les entrées modifiées par une affordance de matériel secondaire, telle qu’un bouton de stylet, un bouton droit de souris ou un élément similaire. Toutefois, l' [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) peut être configuré pour conserver une entrée spécifique non traitée, qui peut ensuite être transmise à votre application à des fins de traitement personnalisé.

Dans cet exemple, nous définissons un bouton d’outil personnalisé qui, lorsqu’il est sélectionné, entraîne le traitement des traits ultérieurs et leur rendu sous forme de lasso de sélection (ligne en pointillé) et non d’entrée manuscrite. Tous les traits d’encre dans les limites de la zone de sélection sont définis sur [**sélectionné**](/uwp/api/windows.ui.input.inking.inkstroke.selected).

> [!NOTE]
> Voir Contrôles pour l’entrée manuscrite pour des recommandations en matière d’expérience utilisateur sur InkCanvas et InkToolbar. La recommandation suivante est pertinente pour cet exemple :
> - Dans le cas d’une sélection du trait, nous recommandons d’utiliser l’icône « EF20 » de la police « Segoe MLD2 Assets » pour le bouton outil, avec une info-bulle « outil de sélection ». 
 
**XAML**

1. Tout d’abord, nous déclarons un élément [**InkToolbarCustomToolButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) (customToolButton) avec un écouteur d’événements Click qui spécifie le gestionnaire d’événements (customToolButton_Click) où la sélection du trait est configurée. (Nous avons également ajouté un ensemble de boutons pour copier, couper et coller la sélection de traits).

2. En outre, nous ajoutons un élément de zone de dessin pour dessiner notre trait de sélection. Une couche distincte permet de dessiner le trait de sélection en s’abstenant de modifier l’élément [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) ou son contenu. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**Code-behind**

2. Ensuite, nous gérons l’événement Click [**InkToolbarCustomToolButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) dans le fichier code-behind MainPage.xaml.cs

   Ce gestionnaire configure l' [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) pour passer l’entrée non traité à l’application. 

   Pour une étape plus détaillée de ce code : consultez la section entrée directe pour le traitement avancé des interactions avec le [stylet et Windows Ink dans les applications Windows](pen-and-stylus-interactions.md).

   Nous avons également spécifié une icône pour le bouton à l’aide de l’élément SymbolIcon et de l’extension de balisage {x:Bind} qui l’associe à un champ défini dans le fichier code-behind (SelectIcon).

   L’extrait de code suivant inclut à la fois le gestionnaire d’événements Click et la définition de SelectIcon.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>Restitution d’une entrée manuscrite personnalisée

Par défaut, l’entrée manuscrite est traitée sur un thread d’arrière-plan de faible latence et restituée « humide » comme elle est dessinée. Lorsque le trait est terminé (stylet ou doigt levé, ou bouton de la souris relâché), le trait est traité sur le thread d’interface utilisateur et rendu « secs » à la couche [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (au-dessus du contenu de l’application et en remplaçant l’encre humide).

La plateforme d’entrée manuscrite vous permet de remplacer ce comportement et de personnaliser entièrement l’expérience d’entrée manuscrite par un séchage personnalisé de cette dernière.

Pour plus d’informations sur le séchage personnalisé, consultez interactions avec le [stylet et Windows Ink dans les applications Windows](./pen-and-stylus-interactions.md#custom-ink-rendering).

> [!NOTE]
> Séchage personnalisé et [ **InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Si votre application remplace le comportement par défaut du rendu d’entrée manuscrite de l’élément [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) par une implémentation de séchage personnalisé, les traits d’encre restitués ne sont plus disponibles pour l’élément InkToolbar et les commandes d’effacement intégrées de l’élément InkToolbar ne fonctionneront pas comme prévu. Pour fournir des fonctionnalités d’effacement, vous devez gérer tous les événements de pointeur, effectuer le test de positionnement sur chaque trait et remplacer la commande intégrée « Effacer toutes les entrées manuscrites ».

## <a name="related-articles"></a>Articles connexes

- [Interactions avec le stylo et le stylet](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Exemples de la rubrique

- [Exemple d’emplacement et d’orientation de la barre d’outils encre (de base)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [Exemple d’emplacement et d’orientation de la barre d’outils Ink (dynamique)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>Autres exemples

- [Exemple d’encre simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Exemple d’encre complexe (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ink, exemple (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Didacticiel de prise en main : écriture manuscrite dans votre application Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Exemple de livre de coloriage](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Exemple de notes de famille](https://github.com/Microsoft/Windows-appsample-familynotes)

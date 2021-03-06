---
description: Si votre application ne fournit pas un bon accès par le clavier, les non-voyants ou les utilisateurs ayant des problèmes de mobilité peuvent rencontrer des difficultés à utiliser votre application ou risquent de ne pas pouvoir l’utiliser du tout.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: Accessibilité du clavier
label: Keyboard accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79dd977cda50d8573cfeab2628ab6227cc9309c0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032502"
---
# <a name="keyboard-accessibility"></a>Accessibilité du clavier  



Si votre application ne fournit pas un bon accès par le clavier, les non-voyants ou les utilisateurs ayant des problèmes de mobilité peuvent rencontrer des difficultés à utiliser votre application ou risquent de ne pas pouvoir l’utiliser du tout.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>Navigation au clavier parmi les éléments d’interface utilisateur  
Pour utiliser le clavier avec un contrôle, celui-ci doit avoir le focus, et pour recevoir le focus (sans utiliser de pointeur), il doit être accessible dans une conception d’interface utilisateur via la navigation par tabulation. Par défaut, l’ordre de tabulation des contrôles est identique à l’ordre dans lequel ils ont été ajoutés à l’aire de conception, répertoriés en XAML ou ajoutés par programme à un conteneur.

Dans la plupart des cas, l’ordre par défaut (basé sur la façon dont vous avez défini les contrôles en XAML) est le plus approprié, en particulier parce qu’il s’agit de l’ordre dans lequel les contrôles sont lus par les lecteurs d’écran. Toutefois, l’ordre par défaut ne correspond pas nécessairement à l’ordre visuel. La position d’affichage réelle peut dépendre du conteneur de disposition parent et de certaines propriétés que vous pouvez définir sur les éléments enfants pour influencer la disposition. Pour vous assurer que votre application a un bon ordre de tabulation, testez ce comportement vous-même. En particulier, si vous utilisez une métaphore de grille ou de tableau comme disposition, l’ordre de lecture des utilisateurs peut être différent de l’ordre de tabulation. Ce n’est pas toujours un problème en soi. Veillez simplement à tester l’interface utilisateur tactile et l’interface utilisateur accessible au clavier de votre application afin de vérifier son bon fonctionnement et sa logique dans les deux cas.

Vous pouvez faire en sorte que l’ordre de tabulation corresponde à l’ordre visuel en ajustant le code XAML. Vous pouvez aussi remplacer l’ordre de tabulation par défaut en définissant la propriété [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) , comme indiqué dans l’exemple suivant d’une disposition en [**grille**](/uwp/api/Windows.UI.Xaml.Controls.Grid) qui utilise la navigation par onglets sur la première colonne.

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

Vous souhaiterez peut-être exclure un contrôle de l’ordre de tabulation. Pour cela, il suffit généralement de rendre le contrôle non interactif, par exemple en affectant la valeur **false** à sa propriété [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled). Un contrôle désactivé est exclu automatiquement de l’ordre de tabulation. Toutefois, il peut arriver que vous souhaitiez exclure un contrôle de l’ordre de tabulation même s’il n’est pas désactivé. Dans ce cas, vous pouvez affecter la valeur **false** à la propriété [**IsTabStop**](/uwp/api/windows.ui.xaml.controls.control.istabstop).

Tout élément qui peut avoir le focus est généralement dans l’ordre de tabulation par défaut. L’exception concerne certains types d’affichage de texte, tels que [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), qui peuvent avoir le focus afin d’être accessibles par le Presse-papiers pour la sélection de texte ; toutefois, ils ne sont pas dans l’ordre de tabulation car on ne s’attend pas à ce que les éléments de texte statiques soient dans l’ordre de tabulation. Ils ne sont pas interactifs dans le sens conventionnel du terme (ils ne peuvent pas être appelés, et ne nécessitent pas l’entrée de texte, mais ils prennent en charge le [modèle de contrôle de texte](/windows/desktop/WinAuto/uiauto-controlpatternsoverview) qui prend en charge la recherche et le réglage des points de sélection dans le texte). Le texte ne doit pas laisser penser que le fait d’y définir le focus permet de déclencher une action. Les éléments de texte seront toujours détectés par les technologies d’assistance et lus à haute voix sur les lecteurs d’écran, mais cela repose sur des techniques qui diffèrent de la recherche de ces éléments dans l’ordre de tabulation pratique.

Que vous ajustiez les valeurs [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) ou que vous utilisiez l’ordre par défaut, les règles suivantes s’appliquent :

* Les éléments d’interface utilisateur dont la valeur [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) est égale à 0 sont ajoutés à l’ordre de tabulation en fonction de l’ordre de déclaration dans les collections XAML ou enfants.
* Les éléments d’interface utilisateur pour lesquels l’élément [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) est supérieur à 0 sont ajoutés à l’ordre de tabulation en fonction de la valeur **tabIndex** .
* Les éléments d’interface utilisateur avec [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) inférieur à 0 sont ajoutés à l’ordre de tabulation et apparaissent avant toute valeur nulle. Cela peut potentiellement différer de la gestion de l’attribut **tabindex** en HTML (par ailleurs, les spécifications HTML antérieures ne prenaient pas en charge un attribut **tabindex** négatif).

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>Navigation au clavier à l’intérieur d’un élément d’interface utilisateur  
Pour les éléments composites, il est important de garantir une navigation interne correcte entre les éléments contenus. Un élément composite peut gérer son enfant actif actuel pour réduire la surcharge que représenterait le fait que tous les éléments enfants puissent avoir le focus. Un tel élément composite est inclus dans l’ordre de tabulation et il gère les événements de navigation au clavier lui-même. Une grande partie des contrôles composites ont déjà une logique de navigation interne intégrée à la gestion des événements du contrôle. Par exemple, la navigation à travers les éléments via les touches de direction est activée par défaut sur les contrôles [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView), [**GridView**](/uwp/api/windows.ui.xaml.controls.gridview), [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) et [**FlipView**](/uwp/api/Windows.UI.Xaml.Controls.FlipView).

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>Alternatives au clavier pour les actions de pointeur et événements pour des éléments de contrôle spécifiques  
Assurez-vous que les éléments d’interface utilisateur sur lesquels il est possible de cliquer peuvent également être appelés à l’aide du clavier. Pour utiliser le clavier avec un élément d’interface utilisateur, l’élément doit avoir le focus. Seules les classes qui dérivent de l’objet [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) prennent en charge le focus et la navigation par tabulation.

Pour les éléments d’interface utilisateur qui peuvent être appelés, mettez en œuvre des gestionnaires d’événements de clavier pour les touches Espace et Entrée. Cela rend la prise en charge de l’accessibilité du clavier de base complète et permet aux utilisateurs d’accomplir des scénarios d’application de base en utilisant uniquement le clavier ; en d’autres termes, les utilisateurs peuvent atteindre tous les éléments d’interface utilisateur interactifs et activer les fonctionnalités par défaut.

Dans les cas où un élément que vous voulez utiliser dans l’interface utilisateur ne peut pas avoir le focus, vous pourriez créer votre propre contrôle personnalisé. Vous devez affecter la valeur **true** à la propriété [**IsTabStop**](/uwp/api/windows.ui.xaml.controls.control.istabstop) pour activer le focus et vous devez fournir une indication visuelle de l’état de focus en créant un état visuel qui décore l’interface utilisateur avec un indicateur de focus. Toutefois, il est souvent plus facile d’utiliser la composition du contrôle de sorte que la prise en charge des arrêts de tabulation, du focus et des homologues et modèles Microsoft UI Automation soit gérée par le contrôle dans lequel vous choisissez de composer votre contenu.

Par exemple, au lieu de gérer un événement de pointeur appuyé sur un élément [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image), vous pouvez encapsuler cet élément dans un élément [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) pour obtenir une prise en charge du pointeur, du clavier et du focus.

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>

## <a name="keyboard-shortcuts"></a>Raccourcis clavier  
Outre l’implémentation de la navigation au clavier et l’activation du clavier pour votre application, il est recommandé de mettre en œuvre des raccourcis pour les fonctionnalités de votre application. La navigation par tabulation fournit un bon niveau de prise en charge de base du clavier, mais avec des formulaires complexes vous souhaiterez peut-être ajouter une prise en charge de raccourcis clavier. Cela peut rendre l’utilisation de votre application plus efficace, même pour les personnes qui utilisent à la fois un clavier et des appareils de pointage.

Un *raccourci* est une combinaison de touches qui améliore la productivité en fournissant à l’utilisateur un moyen efficace d’accéder aux fonctionnalités de l’application. Il existe deux types de raccourcis :

* Une *touche d’accès rapide* est un raccourci vers un élément de l’interface utilisateur de votre application. Les touches d’accès rapide sont formées à partir de la touche Alt et d’une lettre.
* Une *touche accélérateur* est un raccourci vers une commande d’application. Votre application peut avoir ou non une interface utilisateur qui correspond exactement à la commande. Les touches accélérateur sont formées avec la touche Ctrl et une lettre.

Il est impératif de permettre aux utilisateurs qui se font aider de lecteurs d’écran et d’autres technologies d’assistance de découvrir facilement les touches de raccourci de votre application. Communiquez les touches de raccourci à l’aide d’info-bulles, de noms accessibles, de descriptions accessibles ou d’autres formes de communication à l’écran. Les touches de raccourci doivent au minimum être bien documentées dans le contenu d’aide de votre application.

Vous pouvez documenter les touches d’accès rapide via les lecteurs d’écran en définissant la propriété jointe [**AutomationProperties. AccessKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.accesskeyproperty) sur une chaîne qui décrit la touche de raccourci. Il existe également une propriété jointe [**AutomationProperties.AcceleratorKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.acceleratorkeyproperty) pour la documentation des touches de raccourci non mnémoniques, même si les lecteurs d’écran traitent généralement les deux propriétés de la même façon. Essayez de documenter les touches de raccourci de plusieurs manières, à l’aide d’info-bulles, de propriétés Automation et d’une documentation d’aide écrite.

L’exemple suivant montre comment documenter des raccourcis clavier pour des boutons de lecture, de pause et d’arrêt d’un média.

XAML
```xml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>
  </StackPanel>
</Grid>
```

> [!IMPORTANT]
> La définition de [**AutomationProperties. AcceleratorKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.acceleratorkeyproperty) ou de [**AutomationProperties. AccessKey**](/uwp/api/windows.ui.xaml.automation.automationproperties.accesskeyproperty) n’active pas la fonctionnalité du clavier. Elle n’indique à l’infrastructure UI Automation que les raccourcis à utiliser, de sorte que ces informations puissent être transmises aux utilisateurs via des technologies d’assistance. L’implémentation de la gestion des raccourcis doit encore être effectuée dans le code, pas en XAML. Vous devrez quand même associer des gestionnaires pour les événements [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) ou [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) au contrôle approprié afin de mettre véritablement en œuvre le comportement de la touche de raccourci dans votre application. De même, l’ornement de soulignement du texte n’est pas fourni automatiquement pour une touche d’accès. Si vous souhaitez afficher le texte [**souligné**](/uwp/api/Windows.UI.Xaml.Documents.Underline) dans l’interface utilisateur, vous devez souligner explicitement le texte de la clé spécifique dans votre mnémonique en tant que mise en forme du soulignement.

Pour des raisons de simplicité, l’exemple précédent omet l’utilisation de ressources pour des chaînes telles que Ctrl+A. Toutefois, vous devez également tenir compte des touches de raccourci durant la localisation. La localisation des touches de raccourci est pertinente car le choix de la touche à utiliser dépend généralement de l’étiquette de texte visible pour l’élément.

Pour obtenir de l’aide supplémentaire sur l’implémentation de touches de raccourci, voir la section [Touches de raccourci](/windows/win32/uxguide/inter-keyboard) dans la rubrique Recommandations en matière d’interactions utilisateur Windows.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>Implémentation d’un gestionnaire d’événements de touche  
Les événements d’entrée tels que les événements de touche utilisent un concept d’événement appelé *événements routés* . Un événement routé peut se propager à travers les éléments enfants d’un contrôle composite, de telle sorte qu’un parent de contrôle commun peut gérer des événements pour plusieurs éléments enfants. Ce modèle d’événement est pratique pour définir des actions de touches de raccourci pour un contrôle qui contient plusieurs parties composites qui, de par leur conception, ne peuvent pas avoir le focus ou faire partie de l’ordre de tabulation.

Pour obtenir un exemple de code qui illustre l’écriture d’un gestionnaire d’événements de touche incluant la vérification de modificateurs tels que la touche Ctrl, voir [Interactions avec le clavier](../input/keyboard-interactions.md).

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>Navigation au clavier pour les contrôles personnalisés  
Nous recommandons l’utilisation des touches de direction comme raccourcis clavier pour la navigation parmi les éléments enfants, dans les cas où ceux-ci entretiennent une relation spatiale. Si les nœuds de l’arborescence ont des sous-éléments distincts pour la gestion de la fonctionnalité développer-réduire et l’activation des nœuds, utilisez les touches de direction Gauche et Droite pour fournir au clavier la fonctionnalité développer-réduire. Si vous avez un contrôle orienté qui prend en charge la traversée directionnelle dans le contenu du contrôle, utilisez les touches de direction appropriées.

En général, vous implémentez la gestion des clés personnalisées pour les contrôles personnalisés en incluant une substitution des méthodes [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) et [**onkeyup**](/uwp/api/windows.ui.xaml.controls.control.onkeyup) dans le cadre de la logique de la classe.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>Exemple d’un état visuel pour un indicateur de focus  
Nous avons précédemment indiqué que tout contrôle personnalisé qui permet à l’utilisateur d’ajouter un focus doit être doté d’un indicateur de focus visuel. Généralement, cet indicateur de focus se matérialise simplement par le dessin d’une forme rectangulaire dans le proche contour du rectangle de délimitation normal du contrôle. L’élément [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) du focus visuel est un élément homologue par rapport au reste de la composition du contrôle dans un modèle de contrôle, mais à l’origine, la valeur **Collapsed** est affectée à la propriété [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) car le contrôle n’a pas encore le focus. Ensuite, lorsque le contrôle reçoit le focus, un état visuel est appelé. Ce dernier affecte **Visible** spécifiquement à la propriété **Visibility** de l’état visuel du focus. Une fois que le focus est déplacé ailleurs, un autre état visuel est appelé, et la propriété **Visibility** devient **Collapsed** .

Les contrôles XAML par défaut affichent tous un indicateur de focus visuel approprié lorsqu’ils ont le focus (s’ils peuvent recevoir le focus). Ils peuvent aussi avoir des apparences différentes selon le thème sélectionné par l’utilisateur (en particulier si ce dernier utilise un mode de contraste élevé). Si vous utilisez les contrôles XAML dans votre interface utilisateur et que vous ne remplacez pas les modèles de contrôle, aucune opération supplémentaire n’est requise pour obtenir les indicateurs de focus visuels sur les contrôles qui se comportent et qui s’affichent correctement. Mais si vous avez l’intention de redéfinir le modèle d’un contrôle ou si vous souhaitez savoir comment les contrôles XAML fournissent leurs indicateurs de focus visuels, le reste de cette section examine ce qui se passe dans le code XAML et dans la logique des contrôles.

Voici un exemple de code XAML qui provient du modèle XAML par défaut pour un [**bouton**](/uwp/api/Windows.UI.Xaml.Controls.Button).

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

Jusqu’à présent, il s’agit simplement de la composition. Pour contrôler la visibilité de l’indicateur de focus, vous définissez des États visuels qui basculent la propriété de [**visibilité**](/uwp/api/windows.ui.xaml.uielement.visibility) . Cette opération s’effectue à l’aide de la propriété jointe [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) et VisualStateManager. VisualStateGroups, telle qu’elle est appliquée à l’élément racine qui définit la composition.

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

Notez que seul l’un des états nommés règle la propriété [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) directement tandis que les autres semblent vides. La façon dont les États visuels fonctionnent est que dès que le contrôle utilise un autre État du même [**VisualStateGroup**](/uwp/api/Windows.UI.Xaml.VisualStateGroup), toutes les animations appliquées par l’état précédent sont immédiatement annulées. Le rectangle n’apparaîtra pas car la propriété **Visibility** par défaut à partir de la composition a la valeur **Collapsed** . La logique de contrôle le vérifie en écoutant les événements de focus comme [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) et en modifiant les états à l’aide de la méthode [**GoToState**](/uwp/api/windows.ui.xaml.visualstatemanager.gotostate). Souvent, cette modification est déjà gérée pour vous si vous utilisez un contrôle par défaut ou effectuez une personnalisation en fonction d’un contrôle qui possède déjà ce comportement.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>Accessibilité du clavier et Windows Phone
En règle générale, un appareil Windows Phone ne possède pas de clavier matériel dédié. Toutefois, un clavier virtuel (SIP, Soft Input Panel) peut prendre en charge plusieurs scénarios d’accessibilité du clavier. Les lecteurs d’écran peuvent lire une entrée texte à partir du clavier virtuel **texte** , y compris les annonces de suppressions. Les utilisateurs peuvent savoir à quel endroit se trouvent leurs doigts, car le lecteur d’écran peut détecter la navigation de l’utilisateur parmi les touches, qu’il lit à haute voix une à une. En outre, certains concepts d’accessibilité orientée clavier peuvent être associés à des comportements de technologies d’assistance connexes qui n’utilisent pas du tout de clavier. Par exemple, même si un clavier virtuel ne comprend pas de touche Tabulation, le Narrateur prend en charge un mouvement tactile équivalent à l’appui de la touche Tabulation. Ainsi, le fait de disposer d’un ordre de tabulation pratique dans les contrôles d’une interface utilisateur est un principe d’accessibilité important. L’utilisation des touches de direction pour naviguer entre les parties de contrôles complexes est également prise en charge via les mouvements tactiles du Narrateur. Une fois que le focus a atteint un contrôle qui ne sert pas à entrer du texte, le Narrateur prend en charge un mouvement qui appelle l’action de ce contrôle.

En règle générale, les raccourcis clavier ne concernent pas les applications Windows Phone, car un clavier virtuel ne comporte pas de touche Ctrl ou Alt.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes

* [Accessibilité](accessibility.md)
* [Interactions avec le clavier](../input/keyboard-interactions.md)
* [Exemple de clavier tactile](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)

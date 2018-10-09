---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: Menu volant de barre de commandes
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 10/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ed17299051ae7da32f238eb57876b81597c8effa
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4431131"
---
# <a name="command-bar-flyout"></a>Menu volant de barre de commandes

Le menu volant barre de commande vous permet de fournir aux utilisateurs d’accéder facilement aux tâches courantes en affichant des commandes dans une barre d’outils flottant relatifs à un élément sur votre canevas de l’interface utilisateur.

![Un barre de commandes texte développée menu volant](images/command-bar-flyout-text-full.png)

> Pour plus d’informations connexes, voir [les menus volants](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menus et menus contextuels](menus.md)et [les barres de commandes](app-bars.md).

Comme [CommandBar](app-bars.md), CommandBarFlyout possède les propriétés **PrimaryCommands** et **SecondaryCommands** , que vous pouvez utiliser pour ajouter des commandes. Vous pouvez placer les commandes dans une collection de, ou les deux. Quand et comment les commandes principales et secondaires sont affichées varie selon le mode d’affichage.

Le menu volant barre de commandes possède deux modes d’affichage: *réduit* et *développé*.

- En mode réduit, uniquement les commandes principales sont affichent. Si votre barre de commandes menu volant a principales et secondaires des commandes, un bouton «en savoir plus», qui est représenté par les points de suspension \ [•••\], s’affiche. Cela permet à l’utilisateur d’accéder aux commandes secondaires en transition vers le mode étendu.
- Dans le mode étendu, les commandes principales et secondaires sont affichées. (Si le contrôle a uniquement les éléments secondaires, ils sont affichés de manière similaire au contrôle MenuFlyout.)

| **Obtenir la bibliothèque d’interface utilisateur Windows** |
| - |
| Ce contrôle est inclus dans le cadre de la bibliothèque de l’interface utilisateur de Windows, un package NuGet qui contient les nouveaux contrôles et fonctionnalités de l’interface utilisateur pour les applications UWP. Pour plus d’informations, y compris les instructions d’installation, consultez la [vue d’ensemble de la bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de la plateforme** | **API de bibliothèque de l’interface utilisateur Windows** |
| - | - |
| [Classe CommandBarFlyout](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [classe AppBarButton](/uwp/api/windows.ui.xaml.controls.appbarbutton), [classe AppBarToggleButton](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [classe AppBarSeparator](/uwp/api/windows.ui.xaml.controls.appbarseparator) | [Classe CommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [classe TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) |

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié?

Utiliser le contrôle CommandBarFlyout pour afficher une collection de commandes à l’utilisateur, comme les boutons et éléments de menu, dans le contexte d’un élément sur le canevas d’application.

Le TextCommandBarFlyout affiche les commandes de texte dans les contrôles TextBox, TextBlock, RichEditBox, RichTextBlock et PasswordBox. Les commandes sont automatiquement configurés correctement pour la sélection de texte en cours. Utilisez un CommandBarFlyout pour remplacer les commandes de texte par défaut sur les contrôles de texte.

Pour afficher les contextuelles les commandes sur des éléments de liste suivent les recommandations de [contextuelles les commandes pour les listes et les collections](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout Visual Studio MenuFlyout

Pour afficher les commandes dans un menu contextuel, vous pouvez utiliser CommandBarFlyout ou MenuFlyout. Nous vous recommandons CommandBarFlyout car elle offre davantage de fonctionnalités que MenuFlyout. Vous pouvez utiliser CommandBarFlyout avec uniquement des commandes secondaires pour obtenir le comportement et recherchez d’un MenuFlyout ou utiliser le menu volant barre de commande complète avec les commandes principales et secondaires.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong> est installée, cliquez ici pour <a href="xamlcontrolsgallery:/item/CommandBarFlyout">Ouvrir l’application et voir le CommandBarFlyout en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Proactive ou réactive appel

Il existe généralement deux façons d’appeler un menu volant ou un menu qui est associé à un élément sur votre canevas de l’interface utilisateur: _appel proactive_ et _l’appel réactive_.

Dans l’appel proactive, les commandes apparaissent automatiquement lorsque l’utilisateur interagit avec l’élément qui les commandes sont associés. Par exemple, les commandes de mise en forme du texte peuvent s’affichent lorsque l’utilisateur sélectionne le texte dans une zone de texte. Dans ce cas, le menu volant barre de commande ne prend pas le focus. Au lieu de cela, elle présente proche de l’élément d’avec que l’utilisateur interagit des commandes appropriées. Si l’utilisateur n’a pas interagir avec les commandes, ils sont rejetés.

Dans l’appel réactive, les commandes sont affichées en réponse à une action explicite de l’utilisateur pour demander les commandes; par exemple, un clic droit. Cela correspond au concept traditionnel d’un [menu contextuel](menus.md).

Vous pouvez utiliser le CommandBarFlyout dans moyen ou même un mélange des deux.

## <a name="create-a-command-bar-flyout"></a>Créer un menu volant barre de commande

> **Version d’évaluation**: CommandBarFlyout nécessite [dernière build Windows 10 Insider Preview et Kit de développement](https://insider.windows.com/for-developers/) ou la [Bibliothèque d’interface utilisateur Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Cet exemple montre comment créer un menu volant barre de commande et l’utiliser à la fois de façon proactive et réactive. Lorsque l’image est sélectionnée, le menu volant s’affiche en mode réduit. Lorsque apparaît comme un menu contextuel, le menu volant s’affiche en mode étendu. Dans les deux cas, l’utilisateur peut développer ou réduire le menu volant qu’il est ouvert.

:::row:::
    :::column:::
        A collapsed command bar flyout<br/>
        ![Example of a collapsed command bar flyout](images/command-bar-flyout-img-collapsed.png)
    :::column-end:::
    :::column:::
        An expanded command bar flyout<br/>
        ![Example of an expanded command bar flyout](images/command-bar-flyout-img-expanded.png)
    :::column-end:::
:::row-end:::

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Rotate" Icon="Rotate"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>Afficher les commandes de manière proactive

Lorsque vous affichez des commandes contextuelles de manière proactive, uniquement les commandes principales doivent être affichées par défaut (le menu volant barre de commande doit être réduit). Placez les commandes les plus importantes dans la collection de commandes principales et des commandes supplémentaires qui iraient traditionnellement dans un menu contextuel dans la collection de commandes secondaires.

Pour afficher les commandes de manière proactive, vous gérez en règle générale, l’événement de [clic](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) ou [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) pour afficher le menu volant barre de commandes. La valeur du menu volant [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **temporaire** ou **TransientWithDismissOnPointerMoveAway** pour ouvrir le menu volant en mode réduit, sans mettre le focus.

À compter de Windows 10 Insider Preview, les contrôles de texte ont une propriété **SelectionFlyout** . Lorsque vous affectez un menu volant à cette propriété, elle s’affiche automatiquement lorsque le texte est sélectionné.

### <a name="show-commands-reactively"></a>Afficher les commandes de manière réactive

Lorsque vous affichez des commandes contextuelles de manière réactive, comme un menu contextuel, les commandes secondaires sont affichées par défaut (le menu volant barre de commande doit être développé). Dans ce cas, le menu volant barre de commande commandes principales et secondaires, ou peut-être uniquement les commandes secondaires.

Pour afficher les commandes dans un menu contextuel, vous affectez en règle générale, le menu volant à la propriété [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) d’un élément d’interface utilisateur. De cette façon, vous ouvrez le menu volant est géré par l’élément, et vous n’avez rien à faire plus.

Si vous gérez montrant le menu volant (par exemple, sur un événement [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) ), définissez la du menu volant [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) vers **Standard** pour ouvrir le menu volant dans son mode étendu et lui donner le focus.

> [!TIP]
> Pour plus d’informations sur les options lors de l’affichage d’un menu volant et comment contrôler le positionnement du menu volant, consultez [les menus volants](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Commandes et contenu

Le contrôle CommandBarFlyout dispose de 2 propriétés, vous pouvez utiliser pour ajouter des commandes et contenu: [Propriétés PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) et [SecondaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Par défaut, les éléments de la barre de commandes sont ajoutés à la collection **PrimaryCommands**. Ces commandes sont affichées dans la barre de commandes et sont visibles dans les modes développés et réduites. Contrairement aux CommandBar, commandes principales ne garantissent pas automatiquement dépassement aux commandes secondaires et risquent d’être tronqués.

Vous pouvez également ajouter des commandes à la collection **SecondaryCommands** . Commandes secondaires sont affichées dans la partie de menu du contrôle et sont visibles uniquement en mode étendu.

### <a name="app-bar-buttons"></a>Boutons de la barre de l’application

Vous pouvez remplir les propriétés PrimaryCommands et SecondaryCommands directement avec les contrôles [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)et [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) .

Les contrôles des boutons de la barre de l’application sont caractérisés par une icône et une étiquette avec un libellé. Ces contrôles sont optimisés pour une utilisation dans une barre de commandes, et leur apparence change selon que le contrôle est affiché dans la barre de commandes ou le menu de dépassement.

- Les boutons de barre d’application utilisées en tant que commandes principales sont affichées dans la barre de commandes avec uniquement leur icône; l’étiquette de texte n’est pas affiché. Nous vous recommandons d’utiliser une info-bulle pour afficher une description de la commande, comme illustré ici.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- Les boutons de barre d’application utilisées en tant que commandes secondaires sont affichées dans le menu, avec l’étiquette et icône visible.

### <a name="other-content"></a>Autre contenu

Vous pouvez ajouter des autres contrôles à un menu volant barre de commandes par les encapsuler dans un AppBarElementContainer. Cela vous permet d’ajouter des contrôles comme [DropDownButton]() ou un [bouton partagé](), ou ajouter des conteneurs tels que [StackPanel]() pour créer une interface utilisateur plus complexe.

> [!NOTE]
> Pour pouvoir être ajoutés aux collections de commande principale ou secondaire d’un menu volant barre de commande, un élément doit implémenter l’interface [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) . AppBarElementContainer est un wrapper qui implémente cette interface, vous pouvez ajouter un élément à une barre de commandes même si elle n’a pas implémenter l’interface proprement dite.

Ici, un AppBarElementContainer est utilisé pour ajouter des éléments supplémentaires à un menu volant barre de commandes. Un bouton partagé est ajouté aux commandes principales pour autoriser la sélection de couleurs. Un élément StackPanel est ajouté aux commandes secondaires pour permettre à une disposition plus complexe pour les contrôles de zoom.

> [!NOTE]
> Cet exemple ne montre que la commande barre menu volant l’interface utilisateur, qu'il n’implémente pas une des commandes qui sont affichent. Pour plus d’informations sur l’implémentation des commandes, voir les [boutons](buttons.md) et les [principes fondamentaux de conception de commande](../basics/commanding-basics.md).

:::row:::
    :::column:::
        A collapsed command bar flyout with an open SplitButton<br/>
        ![A command bar flyout with a split button](images/command-bar-flyout-split-button.png)
    :::column-end:::
    :::column:::
        An expanded command bar flyout with custom zoom UI in the menu<br/>
        ![A command bar flyout with complex UI](images/command-bar-flyout-complex-ui.png)
    :::column-end:::
:::row-end:::

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Color controls -->
    <AppBarElementContainer>
        <SplitButton Height="Auto" Margin="0,4,0,0"
                     ToolTipService.ToolTip="Colors"
                     Background="{ThemeResource AppBarItemBackgroundThemeBrush}">
            <SplitButton.Content>
                <Rectangle Width="20" Height="20">
                    <Rectangle.Fill>
                        <SolidColorBrush Color="Red"/>
                    </Rectangle.Fill>
                </Rectangle>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Text="Red"/>
                    <MenuFlyoutItem Text="Yellow"/>
                    <MenuFlyoutItem Text="Green"/>
                    <MenuFlyoutItem Text="Blue"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Color controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <Style TargetType="Button">
                    <Setter Property="Background"
                            Value="{ThemeResource AppBarItemBackgroundThemeBrush}"/>
                </Style>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,0">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="86"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Zoom"/>
                <StackPanel Orientation="Horizontal" Grid.Column="1">
                    <Button>
                        <SymbolIcon Symbol="Remove"/>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button>
                        <SymbolIcon Symbol="Add"/>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Créer un menu contextuel avec les commandes secondaires uniquement

Vous pouvez utiliser un CommandBarFlyout avec uniquement des commandes secondaires comme un [menu contextuel](menus.md), à la place d’un MenuFlyout.

![Un menu volant barre de commandes avec uniquement des commandes secondaires](images/command-bar-flyout-context-menu.png)

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Pin" Icon="Pin"/>
                <AppBarButton Label="Unpin" Icon="UnPin"/>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

Vous pouvez également utiliser un CommandBarFlyout avec un DropDownButton pour créer un menu standard.

![Un volant de barre de commande avec comme un bouton menu déroulant](images/command-bar-flyout-button-menu.png)

```xaml
<DropDownButton Content="Mail">
    <DropDownButton.Flyout>
        <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Icon="MailForward" Label="Forward"/>
                <AppBarButton Icon="MailReply" Label="Reply"/>
                <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Menus volants de barre de commandes pour les contrôles de texte

Le [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) est un menu volant barre de commande spécialisé qui contient des commandes pour modifier du texte. Chaque contrôle de texte affiche automatiquement le TextCommandBarFlyout en tant qu’un menu contextuel (clic droit), ou quand le texte est sélectionné. Le volant de barre de commande de texte s’adapte à la sélection de texte à afficher uniquement les commandes appropriées.

:::row:::
    :::column:::
        A text command bar flyout on text selection<br/>
        ![A collapsed text command bar flyout](images/command-bar-flyout-text-selection.png)
    :::column-end:::
    :::column:::
        An expanded text command bar flyout<br/>
        ![An expanded text command bar flyout](images/command-bar-flyout-text-full.png)
    :::column-end:::
:::row-end:::

### <a name="available-commands"></a>Commandes disponibles

Ce tableau indique les commandes qui sont inclus dans un TextCommandBarFlyout, et quand elles s’affichent.

| Commande | Illustré … |
| ------- | -------- |
| Mettre en gras | Lorsque le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Mettre en italiques | Lorsque le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Souligner | Lorsque le contrôle de texte n’est pas en lecture seule (RichEditBox uniquement). |
| Vérification | Lorsque IsSpellCheckEnabled a la **valeur true** et mal orthographié texte est sélectionné. |
| Cut | Lorsque le contrôle de texte n’est pas en lecture seule et texte est sélectionné. |
| Copy | Lorsque le texte est sélectionné. |
| Paste | Lorsque le contrôle de texte n’est pas en lecture seule et le Presse-papiers contient du contenu. |
| Annuler | Lorsqu’il existe une action qui peut être annulée. |
| Tout sélectionner | Lorsque le texte peut être sélectionné. |

### <a name="custom-text-command-bar-flyouts"></a>Texte personnalisé menus volants de barre de commandes

TextCommandBarFlyout ne peuvent pas être personnalisée et est gérée automatiquement par chaque contrôle de texte. Toutefois, vous pouvez remplacer la valeur par défaut TextCommandBarFlyout avec des commandes personnalisées.

- Pour remplacer la valeur par défaut TextCommandBarFlyout qui s’affiche sur la sélection de texte, vous pouvez créer un CommandBarFlyout personnalisé (ou tout autre type de menu volant) et affectez-le à la propriété **SelectionFlyout** . Si vous définissez SelectionFlyout sur la **valeur null**, aucune commandes ne sont affichées sur la sélection.
- Pour remplacer la valeur par défaut TextCommandBarFlyout qui s’affiche en tant que le menu contextuel, attribuer un CommandBarFlyout personnalisé (ou tout autre type de menu volant) à la propriété **ContextFlyout** sur un contrôle de texte. Si vous définissez ContextFlyout sur la **valeur null**, le menu volant illustré dans les versions précédentes du contrôle de texte s’affiche au lieu du TextCommandBarFlyout.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemples de la Galerie de contrôles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - Affichez tous les contrôles XAML dans un format interactif.
- [Exemple de commandes XAML](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Articles associés

- [Informations de base relatives à la conception des commandes pour les applications UWP](../basics/commanding-basics.md)
- [Classe CommandBar](https://msdn.microsoft.com/library/windows/apps/dn279427)
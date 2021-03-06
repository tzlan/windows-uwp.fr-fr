---
description: Découvrez comment utiliser des zones de liste et des zones de liste modifiable, également appelées « listes déroulantes », pour permettre aux utilisateurs de sélectionner des élément dans des listes.
title: Zone de liste modifiable et zone de liste
label: Combo box and list box
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: bb903be202724927d60ee5bcd1edb9e16bc4c982
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829488"
---
# <a name="combo-box-and-list-box"></a>Zone de liste modifiable et zone de liste

Utilisez une liste déroulante pour présenter à l’utilisateur une liste d’éléments parmi lesquels il peut effectuer son choix. Une liste déroulante démarre à l’état compact et se développe pour afficher une liste d’éléments sélectionnables. Un ListBox est semblable à une zone de liste modifiable, mais il n’est pas réductible et n’a donc pas d’état compact. Vous trouverez d’autres informations sur les zones de liste à la fin de cet article.

Quand la liste déroulante est fermée, elle affiche la sélection actuelle ou elle est vide si aucun élément n’est sélectionné. Quand l’utilisateur développe la liste déroulante, elle affiche la liste des éléments sélectionnables.

![Brève vidéo montrant une liste déroulante dans sa forme compacte et sa forme développée.](images/combo-box-expand.gif)

> _Zone de liste modifiable à l’état compact avec un en-tête_

**Obtenir la bibliothèque d’interface utilisateur Windows**

:::row:::
   :::column:::
      ![Logo WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      La bibliothèque d’interface utilisateur Windows version 2.2 ou ultérieure inclut pour ce contrôle un nouveau modèle qui utilise des angles arrondis. Pour plus d’informations, consultez [Rayons des angles](../style/rounded-corner.md). WinUI est un package NuGet qui contient de nouveaux contrôles et des fonctionnalités d’interface utilisateur pour les applications Windows. Pour plus d’informations, notamment des instructions d’installation, consultez la [bibliothèque d’interface utilisateur Windows](/uwp/toolkits/winui/).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de plateforme :** [classe ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [propriété IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propriété Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [événement TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

- Utilisez une liste déroulante pour permettre aux utilisateurs de sélectionner une valeur unique parmi un ensemble d’éléments qui peuvent être représentés correctement à l’aide de simples lignes de texte.
- Utilisez un affichage Liste ou Grille au lieu d’une zone de liste déroulante pour afficher des éléments contenant plusieurs lignes de texte ou images.
- En présence de moins de cinq éléments, utilisez des [cases d’option](radio-button.md) (si un seul élément peut être sélectionné) ou des [cases à cocher](checkbox.md) (si plusieurs éléments peuvent être sélectionnés).
- Utilisez cette zone de liste déroulante pour les éléments de sélection d’importance secondaire au sein de votre application. Si l’option par défaut est recommandée pour la plupart des utilisateurs dans la majorité des situations, l’affichage de tous les éléments à l’aide d’un affichage Liste risque d’attirer l’attention sur les options plus qu’il n’est nécessaire. Pour économiser de l’espace et éviter de distraire l’utilisateur, utilisez une zone de liste déroulante.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/ComboBox">ouvrir l’application et voir le contrôle ComboBox en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Une zone de liste déroulante en état compact peut afficher un en-tête.

![Capture d’écran d’une liste déroulante dans sa forme compacte.](images/combo_box_collapsed.png)

Bien que les zones de listes déroulantes se développent pour prendre en charge des chaînes plus longues, évitez les chaînes excessivement longues qui rendent la lecture difficile.

![Exemple de liste déroulante avec une longue chaîne de texte](images/combo_box_listitemstate.png)

Si la collection figurant dans une zone de liste déroulante est suffisamment longue, une barre de défilement s’affiche. Regroupez les éléments logiquement dans la liste.

![Exemple de barre de défilement dans une liste déroulante](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>Créer une liste déroulante

Vous remplissez la liste déroulante en ajoutant des objets directement à la collection [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) ou en liant la propriété [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) à une source de données. Les éléments ajoutés à la liste déroulante sont wrappés dans des conteneurs [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem).

Voici une liste déroulante simple avec des éléments ajoutés en code XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

L’exemple suivant illustre la liaison d’une liste déroulante à une collection d’objets FontFamily.

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>Sélection d’élément

Tout comme ListView et GridView, ComboBox est dérivé de [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector) pour vous permettre d'obtenir la sélection de l’utilisateur de la même façon standard.

Vous pouvez obtenir ou définir l’élément sélectionné de la liste déroulante à l’aide de la propriété [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) et obtenir ou définir l’index de l’élément sélectionné à l’aide de la propriété [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex).

Pour obtenir la valeur d’une propriété particulière sur l’élément de données sélectionné, vous pouvez utiliser la propriété [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue). Dans ce cas, définissez [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) pour spécifier la propriété sur l’élément sélectionné de laquelle vous souhaitez obtenir la valeur.

> [!TIP]
> Si vous définissez la propriété SelectedItem ou SelectedIndex pour indiquer la sélection par défaut, une exception se produit si la propriété est définie avant que la collection des éléments de la liste déroulante ne soit remplie. Sauf si vous définissez vos éléments en XAML, il est préférable de gérer l’événement Loaded de la liste déroulante et de définir SelectedItem ou SelectedIndex dans le gestionnaire de l’événement Loaded.

Vous pouvez établir des liaisons à ces propriétés en XAML, ou gérer l’événement [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) pour répondre aux modifications de sélection.

Dans le code du gestionnaire d’événements, vous pouvez obtenir la l’élément sélectionné auprès de la propriété [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Vous pouvez obtenir l’élément précédemment sélectionné (le cas échéant) à partir de la propriété [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems). Les collections AddedItems et RemovedItems contiennent chacune un seul élément, car la liste déroulante ne prend pas en charge la sélection multiple.

Cet exemple montre comment gérer l’événement SelectionChanged et également établir une liaison à l’élément sélectionné.

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged et navigation au clavier

Par défaut, l’événement SelectionChanged se produit quand un utilisateur clique ou appuie sur un élément dans la liste, ou appuie sur la touche Entrée pour valider sa sélection et la liste déroulante se ferme. La sélection ne change pas quand l’utilisateur parcourt la liste déroulante ouverte avec les touches de direction du clavier.

Pour créer une liste déroulante avec « mise à jour dynamique » pendant que l’utilisateur parcourt la liste ouverte à l’aide des touches de direction (comme une liste déroulante de sélection de police), définissez [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) sur [Always](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Ainsi, l’événement SelectionChanged se produit quand le focus passe à un autre élément de la liste ouverte.

#### <a name="selected-item-behavior-change"></a>Changement de comportement de l’élément sélectionné

Dans Windows 10 version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou ultérieure, le comportement des éléments sélectionnés est mis à jour pour prendre en charge les listes déroulantes modifiables.

Avant le SDK 17763, la valeur de la propriété SelectedItem (et donc, SelectedValue et SelectedIndex) devait se trouver dans la collection Items de la liste déroulante. Si nous reprenons l’exemple précédent, le paramétrage `colorComboBox.SelectedItem = "Pink"` aboutit à ce qui suit :

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

Dans le SDK 17763 et versions ultérieures, il n’est pas nécessaire que la valeur de la propriété SelectedItem (et donc, SelectedValue et SelectedIndex) se trouve dans la collection Items de la zone déroulante. Si nous reprenons l’exemple précédent, le paramétrage `colorComboBox.SelectedItem = "Pink"` aboutit à ce qui suit :

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>Recherche en texte

Les zones de liste déroulante prennent automatiquement en charge la recherche au sein de leurs collections. Lorsque les utilisateurs tapent des caractères sur un clavier physique dans une zone de liste déroulante ouverte ou fermée, des candidats correspondant à la chaîne entrée apparaissent. Cette fonctionnalité est particulièrement utile lors de la navigation dans une liste longue. Par exemple, quand un utilisateur interagit avec une liste déroulante contenant une liste des États américains, le fait d’appuyer sur la touche « w » affiche « Washington » pour une sélection rapide. La recherche de texte ne respecte pas la casse.

Vous pouvez définir la propriété [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) sur **false** pour désactiver cette fonctionnalité.

## <a name="make-a-combo-box-editable"></a>Rendre une liste déroulante modifiable

> [!IMPORTANT]
> Cette fonctionnalité nécessite Windows 10 version 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) ou ultérieure.

Par défaut, une liste déroulante permet à l’utilisateur d’effectuer son choix dans une liste d’options prédéfinie. Toutefois, dans certains cas, la liste contient uniquement un sous-ensemble de valeurs valides et l’utilisateur doit être en mesure d’entrer d’autres valeurs que celles listées. Pour ce faire, vous pouvez rendre la liste déroulante modifiable.

Pour rendre une liste déroulante modifiable, définissez la propriété [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) sur **true**. Gérez ensuite l’événement [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) pour traiter la valeur entrée par l’utilisateur.

Par défaut, la valeur de la propriété SelectedItem est mise à jour quand l’utilisateur valide le texte personnalisé. Vous pouvez remplacer ce comportement en définissant **Handled** sur **true** dans les arguments de l’événement TextSubmitted. Quand l’événement est marqué comme étant géré, la liste déroulante n’effectue aucune action supplémentaire après l’événement et est conservée à l’état de modification. SelectedItem n’est pas mis à jour.

Cet exemple montre une liste déroulante modifiable simple. La liste contient des chaînes simples, et toute valeur entrée par l’utilisateur est utilisée comme entrée.

Un sélecteur de noms utilisés récemment permet à l’utilisateur d’entrer des chaînes personnalisées. La liste « RecentlyUsedNames » contient des valeurs parmi lesquelles l’utilisateur peut effectuer son choix, mais il peut également ajouter une nouvelle valeur personnalisée. La propriété « CurrentName » représente le nom actuellement entré.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texte envoyé

Vous pouvez gérer l’événement [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) pour traiter la valeur entrée par l’utilisateur. Dans le gestionnaire d’événement, vous vérifiez généralement que la valeur entrée par l’utilisateur est valide, puis utilisez la valeur dans votre application. Selon la situation, vous pouvez également ajouter la valeur à la liste des options de la zone de liste modifiable pour une utilisation ultérieure.

L’événement TextSubmitted se produit quand les conditions ci-après sont remplies :

- La propriété IsEditable est **true**.
- L’utilisateur entre du texte qui ne correspond pas à une entrée existante dans la liste déroulante.
- L’utilisateur appuie sur Entrée ou déplace le focus à partir de la liste déroulante.

L’événement TextSubmitted n’a pas lieu si l’utilisateur entre du texte, puis navigue vers le haut ou vers le bas de la liste.

### <a name="sample---validate-input-and-use-locally"></a>Exemple : validation de l’entrée et utilisation locale

Dans cet exemple, un sélecteur de taille de police contient un ensemble de valeurs correspondant à la gamme de tailles de police, mais l’utilisateur peut entrer des tailles de police qui ne se trouvent pas dans la liste.

Quand l’utilisateur ajoute une valeur non listée, la taille de police est mise à jour, mais la valeur n’est pas ajoutée à la liste des tailles de police.

Si la valeur nouvellement entrée n’est pas valide, vous utilisez SelectedValue pour rétablir la propriété Text à la dernière valeur correcte connue.

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app's font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Exemple : validation de l’entrée et ajout à la liste

Ici, un sélecteur de couleur favorite contient les couleurs favorites les plus courantes (rouge, bleu, vert, orange), mais l’utilisateur peut entrer une couleur favorite qui ne se trouve pas dans la liste. Quand l’utilisateur ajoute une couleur valide (par exemple, rose), la couleur nouvellement entrée est ajoutée à la liste et définie comme « couleur favorite » active.

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

- Limitez le contenu texte des éléments de zone de liste déroulante à une seule ligne.
- Triez les éléments d’une zone de liste déroulante dans l’ordre le plus logique. Regroupez les options associées et placez les options les plus courantes en haut. Triez les noms par ordre alphabétique, les nombres par ordre numérique et les dates par ordre chronologique.

## <a name="list-boxes"></a>Zones de liste

Une zone de liste permet à l’utilisateur de choisir un ou plusieurs éléments d’une collection. Les zones de liste sont similaires aux listes déroulantes, sauf qu’elles sont toujours ouvertes, c’est-à-dire qu’elles n’ont pas d’état compact (non développé). Les éléments de la liste peuvent défiler si l’espace est insuffisant pour les afficher tous.

### <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

- Une zone de liste peut être utile quand des éléments de la liste sont suffisamment importants pour être mis en avant, et quand l’écran offre suffisamment d’espace pour afficher la liste complète.
- Une zone de liste doit attirer l’attention de l’utilisateur sur toutes les possibilités d’un choix important. En revanche, une liste déroulante attire initialement l’attention de l’utilisateur sur l’élément sélectionné.
- Évitez d’utiliser une zone de liste dans les cas suivants :
    - Il existe un très petit nombre d’éléments pour la liste. Si une zone de liste à sélection unique comporte toujours les deux mêmes options, mieux vaut utiliser des [cases d’option](radio-button.md). Pensez également à utiliser des cases d’option quand 3 ou 4 éléments statiques figurent dans la liste.
    - La zone de liste est à sélection unique, et propose toujours les deux mêmes options, l’une étant l’inverse de l’autre (par exemple, « activé » et « désactivé »). Utilisez une case à cocher ou un bouton bascule.
    - Le nombre d’éléments est très élevé. Pour les longues listes, mieux vaut utiliser un affichage Grille ou Liste. Pour les très longues listes de données groupées, utilisez de préférence un zoom sémantique.
    - Les éléments sont des valeurs numériques contiguës. Si tel est le cas, pensez à utiliser un [curseur](slider.md).
    - Les éléments de sélection ont une importance secondaire dans le flux de votre application, ou l’option par défaut est recommandée pour la plupart des utilisateurs dans la majorité des situations. Dans ce cas, utilisez plutôt une liste déroulante.

### <a name="recommendations"></a>Recommandations

- La plage idéale d’éléments dans une zone de liste est de 3 à 9.
- Une zone de liste est efficace quand ses éléments peuvent varier de manière dynamique.
- Dans la mesure du possible, la taille de la zone de liste doit être suffisante pour que vous n’ayez pas à faire défiler la liste des éléments.
- Vérifiez qu’il n’y a aucune ambiguïté quand à la fonction de la zone de liste et aux éléments sélectionnés actuellement.
- Réservez les effets visuels et les animations pour le retour tactile et pour l’état sélectionné des éléments.
- Limitez le contenu textuel de l’élément de zone de liste à une seule ligne. Si les éléments sont visuels, vous pouvez personnaliser la taille. Si un élément contient plusieurs lignes de texte ou images, utilisez plutôt un affichage de Grille ou Liste.
- Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
- N’utilisez pas une zone de liste pour exécuter des commandes ou pour afficher ou masquer de manière dynamique d’autres contrôles.

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.
- [Exemple AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Articles connexes

- [Contrôles de texte](text-controls.md)
- [Vérification de l’orthographe](text-controls.md)
- [Recherche](search.md)
- [TextBox, classe](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [PasswordBox Windows.UI.Xaml.Controls, classe](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length, propriété](/dotnet/api/system.string.length)

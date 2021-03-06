---
description: Permet de sélectionner ou de désélectionner des éléments d’action. Peut être utilisée pour un ou plusieurs éléments de liste.
title: Cases à cocher
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e18f750f7a442fdfe5d5ffc0119a8a64f5571407
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030962"
---
# <a name="check-boxes"></a>Cases à cocher

Une case à cocher permet de sélectionner ou de désélectionner des éléments d’action. Elle peut être utilisée pour un élément unique ou pour une liste de plusieurs éléments parmi lesquels l’utilisateur peut en choisir quelques-uns. Le contrôle possède trois états de sélection : l’état désélectionné, l’état sélectionné et l’état indéterminé. Sélectionnez l’état indéterminé lorsqu’une collection de sous-choix possède des états désélectionné et sélectionné.

![Exemple d’états de case à cocher](images/templates-checkbox-states-default.png)

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

> **API de plateforme :** [classe CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox), [événement Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [propriété IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)


## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Utilisez une **case à cocher unique** pour un choix binaire Oui/Non, comme avec la question « Mémoriser mes informations ? » scénario de connexion avec conditions d’utilisation du service.

![Case à cocher unique pour un choix individuel](images/checkbox1.png)

Dans le cas d’un choix binaire, la seule différence entre une **case à cocher** et un [bouton bascule](toggles.md) est que le premier désigne un état, tandis que le deuxième indique une action. Vous pouvez retarder la validation d’une opération de case à cocher (par exemple, dans le cadre de l’envoi d’un formulaire) alors que l’utilisation d’un bouton bascule nécessite une action immédiate de votre part. Seules les cases à cocher autorisent une sélection multiple.

Utilisez **plusieurs cases à cocher** pour les scénarios à sélection multiple dans lesquels un utilisateur choisit un ou plusieurs éléments à partir d’un groupe d’options qui ne s’excluent pas mutuellement.

Créez un groupe de cases à cocher quand les utilisateurs peuvent choisir une combinaison quelconque d’options.

![Sélection de plusieurs options à l’aide de cases à cocher](images/checkbox2.png)

Lorsque les options peuvent être regroupées, vous pouvez utiliser une case à cocher indéterminée pour représenter le groupe entier. Utilisez l’état indéterminé de la case à cocher quand un utilisateur sélectionne une partie, et non l’ensemble, des éléments dans le groupe.

![Cases à cocher indiquant un choix mixte](images/checkbox3.png)

Les deux contrôles **case à cocher** et **case d’option** permettent à l’utilisateur d’effectuer un choix dans une liste d’options. Les cases à cocher permettent à l’utilisateur de sélectionner une combinaison quelconque d’options. En revanche, les cases d’option permettent à l’utilisateur d’effectuer un choix unique à partir d’options qui s’excluent mutuellement. Lorsque plusieurs options sont disponibles mais qu’une seule peut être choisie, utilisez une case d’option à la place.

## <a name="examples"></a>Exemples

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/CheckBox">ouvrir l’application et voir l’objet CheckBox en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>Créer une case à cocher

Pour attribuer une étiquette à la case à cocher, définissez la propriété [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content). L’étiquette s’affiche en regard de la case à cocher.

Ce code XAML crée une case à cocher unique qui sert à accepter les conditions d’utilisation avant l’envoi d’un formulaire. 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

Voici la même case à cocher sous forme de code.

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### <a name="bind-to-ischecked"></a>Lier à IsChecked

Utilisez la propriété [IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked) pour déterminer si la case à cocher est activée ou désactivée. Vous pouvez lier la valeur de la propriété IsChecked à une autre valeur binaire.
Toutefois, IsChecked étant une valeur booléenne [Nullable](/dotnet/api/system.nullable-1), vous devez utiliser un cast ou un convertisseur de valeur pour la lier à une propriété booléenne. Cela dépend du type de liaison réel que vous utilisez ; vous trouverez des exemples ci-dessous pour chaque type possible. 

Dans cet exemple, la propriété **IsChecked** de la case à cocher pour accepter les conditions d’utilisation est liée à la propriété [IsEnabled](/uwp/api/windows.ui.xaml.controls.control.isenabled) du bouton Envoyer. Le bouton Envoyer est activé uniquement si l’utilisateur accepte les conditions d’utilisation.

#### <a name="using-xbind"></a>Utilisation de x:Bind

> Remarque&nbsp;&nbsp;Nous affichons ici uniquement le code approprié. Pour plus d’informations sur la liaison de données, consultez [Vue d’ensemble de la liaison de données](../../data-binding/data-binding-quickstart.md). Les informations propres à {x:Bind} (telles que la réalisation d’un cas) sont détaillées [ici](../../xaml-platform/x-bind-markup-extension.md).

```xaml
<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind (x:Boolean)termsOfServiceCheckBox.IsChecked, Mode=OneWay}"/>
</StackPanel>
```

Si la case à cocher peut également être dans l’état **indéterminé** , nous utilisons la propriété [FallbackValue](/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) de la liaison pour spécifier la valeur booléenne représentant cet état. Dans ce cas, nous ne souhaitons pas que le bouton Envoyer soit également activé :

```xaml
<Button Content="Submit" 
        IsEnabled="{x:Bind (x:Boolean)termsOfServiceCheckBox.IsChecked, Mode=OneWay, FallbackValue=False}"/>
```

#### <a name="using-xbind-or-binding"></a>Utilisation de x:Bind ou Binding

> Remarque&nbsp;&nbsp;Nous affichons ici uniquement le code approprié pour {x:Bind}. Dans l’exemple {Binding}, nous remplacerions {x:Bind} par {Binding}. Pour plus d’informations sur la liaison de données, les convertisseurs de valeurs et les différences entre les extensions de balisage {x:Bind} et {Binding}, consultez [Vue d’ensemble de la liaison de données](../../data-binding/data-binding-quickstart.md).

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```


```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### <a name="handle-click-and-checked-events"></a>Gérer les événements Click et Checked

Pour effectuer une action lorsque l’état de la case à cocher change, vous pouvez gérer soit l’événement [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), soit les événements [Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) et [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked). 

Les événements **Click** se produisent chaque fois que l’état d’activation change. Si vous gérez l’événement Click, utilisez la propriété **IsChecked** pour déterminer l’état de la case à cocher.

Les événements **Checked** et **Unchecked** se produisent de manière indépendante. Si vous gérez ces événements, vous devez gérer les deux pour répondre aux changements d’état de la case à cocher.

Les exemples suivants vous présentent la gestion de l’événement Click et des événements Checked et Unchecked.

Plusieurs cases à cocher peuvent partager le même gestionnaire d’événements. Cet exemple crée quatre cases à cocher pour la sélection des ingrédients de garniture d’une pizza. Les quatre cases à cocher partagent le même gestionnaire d’événements **Click** pour mettre à jour la liste des ingrédients sélectionnés.

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

Voici le gestionnaire d’événements pour l’événement Click. Chaque fois que vous cliquez sur une case à cocher, il examine les cases à cocher pour vérifier celles qui sont cochées et met à jour la liste des ingrédients sélectionnés.

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### <a name="use-the-indeterminate-state"></a>Utiliser l’état indéterminé

Le contrôle CheckBox hérite de [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) et peut avoir trois états : 

État | Propriété | Value
------|----------|------
checked | IsChecked | **true** 
unchecked | IsChecked | **false** 
indeterminate | IsChecked | **null** 

Pour que la case à cocher indique l’état indéterminé, vous devez définir la propriété [IsThreeState](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.isthreestate) sur **true**. 

Lorsque les options peuvent être regroupées, vous pouvez utiliser une case à cocher indéterminée pour représenter le groupe entier. Utilisez l’état indéterminé de la case à cocher quand un utilisateur sélectionne une partie, et non l’ensemble, des éléments dans le groupe.

Dans l’exemple suivant, la propriété IsThreeState de la case à cocher « Select all » est définie sur **true**. La case à cocher « Select all » est sélectionnée si tous les éléments enfants sont sélectionnés ; si tous les éléments enfants sont désélectionnés, la case à cocher l’est également. Sinon, la case à cocher reste indéterminée.

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## <a name="dos-and-donts"></a>Pratiques conseillées et déconseillées

-   Assurez-vous que le but et l’état actuel de la case à cocher sont clairs.
-   Limitez le texte de la case à cocher à deux lignes.
-   Formulez le libellé de la case à cocher comme une instruction indiquant que la présence d’une coche correspond à une activation, et qu’à l’inverse, l’absence d’une coche correspond à une désactivation.
-   Utilisez la police par défaut à moins que vos instructions de personnalisation imposent d’en utiliser une autre.
-   Si le contenu du texte est dynamique, songez au redimensionnement du contrôle et à ses conséquences sur les effets visuels environnants.
-   Si vous avez le choix entre plusieurs options mutuellement exclusives, pensez à utiliser des [cases d’option](radio-button.md).
-   Ne placez pas deux groupes de cases à cocher en regard l’un de l’autre. Utilisez des étiquettes de groupe pour les distinguer.
-   N’utilisez pas de case à cocher comme contrôle actif/inactif ou pour exécuter une commande ; utilisez de préférence un bouton bascule.
-   N’utilisez pas de case à cocher pour afficher d’autres contrôles, tels qu’une boîte de dialogue.
-   Utilisez l’état indéterminé pour signaler qu’une option est définie pour une poignée de sous-choix, mais pas pour tous.
-   Si vous avez recours à l’état indéterminé, utilisez des cases à cocher subordonnées pour indiquer les options sélectionnées et celles qui ne le sont pas. Concevez l’interface utilisateur afin que l’utilisateur puisse voir les sous-choix.
-   N’utilisez pas l’état indéterminé pour désigner un troisième état. L’état indéterminé sert à indiquer qu’une option est définie pour une poignée de sous-choix et non la totalité. N’autorisez donc pas les utilisateurs à définir directement un état indéterminé. Parfait exemple à ne pas suivre, la case à cocher qui suit utilise l’état indéterminé pour indiquer un goût moyennement épicé :

    ![Case à cocher avec un état indéterminé](images/checkbox4_spicy.png)

    Utilisez plutôt un groupe de cases d’options proposant trois options : Non épicé, Épicé et Très épicé.

    ![Groupe de cases d’options proposant trois options : Non épicé, Épicé et Très épicé](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Classe CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 
- [Boutons de radio](radio-button.md)
- [Commutateur bascule](toggles.md)

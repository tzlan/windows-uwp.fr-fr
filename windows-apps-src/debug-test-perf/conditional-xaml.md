---
title: XAML conditionnel
description: Utiliser de nouvelles API dans le balisage XAML tout en préservant la compatibilité avec les versions précédentes
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f4d2e4c2c1cfde922e46ddea189ab93447f2b323
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89411953"
---
# <a name="conditional-xaml"></a>XAML conditionnel

Le *XAML conditionnel* permet d'utiliser la méthode [ApiInformation.IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) dans le balisage XAML. Cela vous permet de définir des propriétés et d’instancier des objets sur la base de la présence d’une API sans avoir à utiliser le code-behind. Il analyse de façon sélective les éléments ou attributs pour déterminer s’ils seront disponibles au moment de l’exécution. Les instructions conditionnelles sont évaluées au moment de l’exécution, et les éléments qualifiés associés à une balise XAML conditionnelle sont analysés si leur valeur est évaluée à **true** (vrai) ; sinon, ils sont ignorés.

Le XAML conditionnel est disponible depuis Creators Update (version 1703, build 15063). Pour utiliser le XAML conditionnel, la version minimale de votre projet Visual Studio doit correspondre à la build 15063 (Creators Update) ou ultérieur, et la version cible doit correspondre à une version postérieure à la version minimale. Pour plus d’informations sur la configuration de votre projet Visual Studio, consultez [Applications adaptatives de version](version-adaptive-apps.md).

> [!NOTE]
> Pour créer une application adaptative de version avec une version minimale inférieure à la build 15063, vous devez utiliser le [code adaptatif de version](version-adaptive-code.md), et non le XAML.

Pour obtenir des informations générales importantes sur ApiInformation et les contrats API, consultez [Applications adaptatives de version](version-adaptive-apps.md).

## <a name="conditional-namespaces"></a>Espaces de noms conditionnels

Pour utiliser une méthode conditionnelle en XAML, vous devez d’abord déclarer un [espace de noms XAML](../xaml-platform/xaml-namespaces-and-namespace-mapping.md) conditionnel en haut de votre page. Voici un exemple de pseudo-code d’un espace de noms conditionnel :

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

Un espace de noms conditionnel peut être décomposé en deux parties séparées par le délimiteur « ? ». 

- Le contenu qui précède le délimiteur indique l’espace de noms ou le schéma qui contient l’API référencée. 
- Le contenu qui suit le délimiteur « ? » représente la méthode conditionnelle qui détermine si l’espace de noms conditionnel a la valeur **true** ou **false**.

Dans la plupart des cas, le schéma sera l’espace de noms XAML par défaut :

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

Le XAML conditionnel prend en charge les méthodes conditionnelles suivantes :

Méthode | Inverse
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

Nous discuterons de ces méthodes plus en détail dans les sections suivantes de cet article.

> [!NOTE]
> Nous vous recommandons d’utiliser IsApiContractPresent et IsApiContractNotPresent. Les autres instructions conditionnelles ne sont pas intégralement prises en charge dans l’expérience de conception Visual Studio.

## <a name="create-a-namespace-and-set-a-property"></a>Créer un espace de noms et définir une propriété

Dans cet exemple, vous allez afficher « Bonjour, XAML conditionnel » comme contenu d’un bloc de texte si l’application s’exécute sur Fall Creators Update ou version ultérieure. Par défaut, aucun contenu ne sera affiché si l’application s’exécute sur une version antérieure.

Tout d’abord, définissez un espace de noms personnalisé avec le préfixe « contract5Present » et utilisez l’espace de noms XAML par défaut https://schemas.microsoft.com/winfx/2006/xaml/presentation) comme schéma contenant la propriété [TextBlock.Text](/uwp/api/windows.ui.xaml.controls.textblock.Text). Pour en faire un espace de noms conditionnel, ajoutez le séparateur « ? » après le schéma.

Définissez ensuite une instruction conditionnelle qui retourne **true** sur les appareils exécutant Fall Creators Update ou une version ultérieure. Utilisez la méthode ApiInformation **IsApiContractPresent** pour rechercher la version 5 de UniversalApiContract. La version 5 de UniversalApiContract a été publiée avec Fall Creators Update (SDK 16299).

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Une fois l’espace de noms défini, ajoutez le préfixe d’espace de noms à la propriété Text de votre contrôle TextBox pour la qualifier de propriété à définir de manière conditionnelle au moment de l’exécution.

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

Voici le code XAML complet.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

Quand vous exécutez cet exemple sur Fall Creators Update, le texte « Bonjour, XAML conditionnel » s’affiche. Quand vous l’exécutez sur Creators Update, aucun texte n’apparaît.

Le XAML conditionnel vous permet d’effectuer les vérifications d’API que vous pouvez faire sinon dans le code de votre balisage. Voici le code équivalent pour cette vérification.

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

Notez que même si la méthode IsApiContractPresent utilise une chaîne pour le paramètre *contractName*, vous n’utilisez pas de guillemets (" ") dans la déclaration d’espace de noms XAML.

## <a name="use-ifelse-conditions"></a>Utiliser des conditions if/else

Dans l’exemple précédent, la propriété Text est définie uniquement quand l’application s’exécute sur Fall Creators Update. Mais que faire pour afficher un texte différent quand elle s’exécute sur Creators Update ? Vous pouvez essayer de définir la propriété Text sans qualificateur conditionnel, comme suit.

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

Cela fonctionnera quand elle sera exécutée sur Creators Update. En revanche, si vous l’exécutez sur Fall Creators Update, vous recevrez un message d’erreur indiquant que la propriété Text est définie plusieurs fois.

Pour définir un texte différent pour une exécution de l’application sur d’autres versions de Windows 10, vous avez besoin d’une autre condition. Le XAML conditionnel fournit l’inverse de chaque méthode ApiInformation prise en charge pour vous permettre de créer des scénarios conditionnels if/else comme suit.

La méthode IsApiContractPresent retourne **true** si l’appareil actif contient le contrat et le numéro de version spécifiés. Supposons, par exemple, que votre application est en cours d’exécution sur Creators Update, qui intègre la version 4 du contrat API universel.

Plusieurs appels à IsApiContractPresent retourneraient ces résultats :

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true.

IsApiContractNotPresent retourne le résultat inverse de IsApiContractPresent. Les appels à IsApiContractNotPresent retourneraient ces résultats :

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

Pour utiliser la condition inverse, créez un deuxième espace de noms XAML conditionnel qui utilise l’instruction conditionnelle **IsApiContractNotPresent**. Ici, il contient le préfixe « contract5NotPresent ».

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

Une fois les deux espaces de noms définis, vous pouvez définir deux fois la propriété Text à condition toutefois de les faire précéder de qualificateurs qui garantiront qu’un seul paramètre de propriété sera utilisé au moment de l’exécution, comme suit :

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

Voici un autre exemple qui définit l’arrière-plan d’un bouton. La fonctionnalité [matériau Acrylique](../design/style/acrylic.md) n’est disponible qu’à partir de Fall Creators Update. Vous ne pourrez donc utiliser un arrière-plan acrylique que si l’application s’exécute sur Fall Creators Update. Comme l’arrière-plan acrylique n’est pas disponible sur les versions antérieures, définissez un arrière-plan rouge qui s’appliquera le cas échéant.

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>Créer des contrôles et lier des propriétés

Jusqu’ici, nous avons vu comment définir des propriétés à l’aide du XAML conditionnel, mais il est aussi possible d’instancier de manière conditionnelle des contrôles basés sur le contrat API disponible au moment de l’exécution.

Ici, un contrôle [ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker) est instancié quand l’application s’exécute sur Fall Creators Update où le contrôle est disponible. ColorPicker n’étant pas disponible avant Fall Creators Update, utilisez une zone de liste déroulante ([ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)) quand l’application s’exécute sur des versions antérieures de façon à proposer des choix de couleurs simplifiés à l’utilisateur.

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

Vous pouvez utiliser des qualificateurs conditionnels avec différentes formes de [syntaxe de propriété XAML](../xaml-platform/xaml-syntax-guide.md). Ici, la propriété Fill du rectangle est définie à l’aide de la syntaxe d’élément de propriété pour Fall Creators Update et de la syntaxe d’attribut pour les versions précédentes.

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

Quand vous liez une propriété à une autre propriété qui dépend d’un espace de noms conditionnel, vous devez utiliser la même condition dans les deux propriétés. Ici, sachant que `colorPicker.Color` dépend de l’espace de noms conditionnel « contract5Present », vous devez aussi placer le préfixe « contract5Present » dans la propriété SolidColorBrush.Color. (Vous pouvez aussi placer le préfixe « contract5Present » dans SolidColorBrush et non dans la propriété Color). À défaut, vous obtiendrez une erreur de compilation.

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

Voici le code XAML complet qui illustre ces scénarios. Cet exemple contient un rectangle et une interface utilisateur qui vous permet de définir la couleur du rectangle.

Quand l’application s’exécute sur Fall Creators Update, vous utilisez un contrôle ColorPicker pour permettre à l’utilisateur de définir la couleur. ColorPicker n’étant pas disponible avant Fall Creators Update, utilisez une zone de liste déroulante quand l’application s’exécute sur des versions antérieures de façon à proposer des choix de couleurs simplifiés à l’utilisateur.

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>Articles connexes

- [Guide des applications UWP](../get-started/universal-application-platform-guide.md)
- [Dynamically detecting features with API contracts](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API Contracts](https://channel9.msdn.com/Events/Build/2015/3-733) (Vidéo Build 2015)
- [Programmation avec les kits SDK d’extension](/uwp/extension-sdks/device-families-overview)

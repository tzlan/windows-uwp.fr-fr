---
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
description: Le bloc de texte est le contrôle principalement utilisé pour afficher du texte en lecture seule dans les applications.
title: Bloc de texte
label: Text block
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: aa72b01e6c567e55e36e7f182ca962367346980c
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220422"
---
# <a name="text-block"></a>Bloc de texte

Le bloc de texte est le contrôle principalement utilisé pour afficher du texte en lecture seule dans les applications. Ce contrôle vous permet d’afficher une ou plusieurs lignes de texte, des liens hypertexte inclus et du texte avec mise en forme de type gras, italique ou souligné.
 
 > **API de plateforme** : [Classe TextBlock class](/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [propriété Text](/uwp/api/windows.ui.xaml.controls.textblock.text), [propriété Inlines](/uwp/api/windows.ui.xaml.controls.textblock.inlines)

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Un bloc de texte est généralement plus facile à utiliser et offre de meilleures performances en termes de rendu de texte qu’un bloc de texte enrichi. C’est pourquoi il est recommandé pour la plupart des textes d’interface utilisateur d’application. Vous pouvez facilement visualiser et utiliser le texte d’un bloc de texte dans votre application en obtenant la valeur de la propriété [Text](/uwp/api/windows.ui.xaml.controls.textblock.text). Il propose également de nombreuses options de mise en forme similaires pour personnaliser la manière dont votre texte est restitué.

Même si vous pouvez placer des sauts de ligne dans le texte, un bloc de texte est conçu pour afficher un seul paragraphe et ne prend pas en charge le retrait du texte. Utilisez un contrôle **RichTextBlock** si vous devez prendre en charge plusieurs paragraphes, du texte sur plusieurs colonnes, d’autres dispositions de texte complexes ou des éléments d’interface utilisateur inclus, tels que des images.

Pour plus d’informations sur le choix du contrôle de texte approprié, consultez l’article [Contrôles de texte](text-controls.md).

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/TextBlock">ouvrir l’application et voir le contrôle TextBlock en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-text-block"></a>Créer un bloc de texte

Voici comment définir un contrôle TextBlock simple et sa propriété Text sur une chaîne.

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

### <a name="content-model"></a>Modèle de contenu

Vous pouvez utiliser deux propriétés pour ajouter du contenu à un élément TextBlock : [Text](/uwp/api/windows.ui.xaml.controls.textblock.text) et [Inlines](/uwp/api/windows.ui.xaml.controls.textblock.inlines).

La méthode la plus courante pour afficher du texte est de définir la propriété Text sur une valeur de chaîne, comme illustré dans l’exemple précédent.

Vous pouvez également ajouter du contenu en plaçant des éléments de contenu de flux inline dans la propriété TextBox.Inlines, comme suit.
```xaml
<TextBlock>Text can be <Bold>bold</Bold>, <Underline>underlined</Underline>, 
    <Italic>italic</Italic>, or a <Bold><Italic>combination</Italic></Bold>.</TextBlock>
```

Les éléments dérivés de la classe Inline, tels que Bold, Italic, Run, Span et LineBreak activent une mise en forme différente pour les diverses parties du texte. Pour en savoir plus, voir la section [Mise en forme du texte](#formatting-text). L’élément Hyperlink inline vous permet d’ajouter un lien hypertexte à votre texte. Toutefois, l’utilisation de Inlines désactive également le rendu du texte du chemin rapide, qui est abordé dans la section suivante.


## <a name="performance-considerations"></a>Considérations relatives aux performances

Dans la mesure du possible, XAML utilise un chemin de code plus efficace pour la disposition du texte. Ce chemin rapide permet à la fois de diminuer l’utilisation de mémoire globale et de réduire considérablement le temps processeur requis pour mesurer et organiser le texte. Ce chemin rapide s’applique uniquement au contrôle TextBlock. Celui-ci doit donc être préféré au contrôle RichTextBlock dans la mesure du possible.

Certaines conditions exigent un contrôle TextBlock afin de revenir à un chemin de code plus riche en fonctionnalités et exigeant en ressources de processeur pour le rendu du texte. Pour conserver le rendu du texte sur le chemin rapide, veillez à suivre ces recommandations lors de la définition des propriétés répertoriées ici.
- [Text](/uwp/api/windows.ui.xaml.controls.textblock.text) : La condition la plus importante est que le chemin rapide soit utilisé seulement quand vous définissez le texte en définissant explicitement la propriété Text en XAML ou dans le code (comme illustré dans les exemples précédents). La définition du texte via la collection Inlines du contrôle TextBlock (telle que `<TextBlock>Inline text</TextBlock>`) désactive le chemin rapide en raison de la complexité potentielle liée à de multiples formats.
- [CharacterSpacing](/uwp/api/windows.ui.xaml.controls.textblock.characterspacing) : Seule la valeur par défaut 0 permet d’utiliser le chemin rapide.
- [TextTrimming](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) : Seules les valeurs **None**, **CharacterEllipsis** et **WordEllipsis** permettent d’utiliser le chemin rapide. La valeur **Clip** désactive le chemin rapide.

> **Remarque**&nbsp;&nbsp;Dans les versions antérieures à Windows 10, version 1607, des propriétés supplémentaires ont également un impact sur le chemin rapide. Si votre application est exécutée dans une version antérieure de Windows, ces conditions entraînent également l’affichage de votre texte sur le chemin lent. Pour plus d’informations sur les versions, consultez [Code adaptatif de version](../../debug-test-perf/version-adaptive-code.md).
- [Typography](/uwp/api/Windows.UI.Xaml.Documents.Typography) : Seules les valeurs par défaut des différentes propriétés Typography permettent d’utiliser le chemin rapide.
- [LineStackingStrategy](/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy) : Si [LineHeight](/uwp/api/windows.ui.xaml.controls.textblock.lineheight) est différent de 0, les valeurs **BaselineToBaseline** et **MaxHeight** désactivent le chemin rapide.
- [IsTextSelectionEnabled](/uwp/api/windows.ui.xaml.controls.textblock.istextselectionenabled) : Seul **false** permet d’utiliser le chemin rapide. La définition de cette propriété sur **true** désactive le chemin rapide.

Vous pouvez définir la propriété [DebugSettings.IsTextPerformanceVisualizationEnabled](/uwp/api/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled) sur **true** pendant le débogage pour déterminer si le texte utilise le chemin rapide pour le rendu. Quand cette propriété est définie sur true, le texte qui utilise le chemin rapide s’affiche en vert clair.

>**Conseil**&nbsp;&nbsp;Cette fonctionnalité est expliquée en détail dans cette session à partir de la build 2015 - [Performances de XAML : Techniques d’optimisation des expériences d’application Windows universelles générées avec XAML](https://channel9.msdn.com/Events/Build/2015/3-698).



Vous définissez généralement les paramètres de débogage dans la substitution de méthode [OnLaunched](/uwp/api/windows.ui.xaml.application.onlaunched) dans la page code-behind pour App.xaml, comme ceci :
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

Dans cet exemple, le premier contrôle TextBlock est affiché à l’aide du chemin rapide, mais pas le second.
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

Lorsque vous exécutez ce code XAML en mode débogage avec la propriété IsTextPerformanceVisualizationEnabled définie sur true, le résultat se présente ainsi :

![Texte affiché en mode débogage](images/text-block-rendering-performance.png)

>**Attention**&nbsp;&nbsp;La couleur du texte qui n’est pas sur le chemin rapide n’est pas changée. Si vous avez du texte dans votre application dont la couleur spécifiée est vert clair, cette couleur reste inchangée quand il est sur le chemin de rendu plus lent. Ne pas confondre le texte défini comme étant vert dans l’application avec le texte du chemin d’accès rapide apparaissant en vert en raison des paramètres de débogage.

## <a name="formatting-text"></a>Mise en forme du texte

Bien que la propriété Text stocke du texte brut, vous pouvez appliquer différentes options de mise en forme au contrôle TextBlock afin de personnaliser la manière dont le texte est restitué dans votre application. Vous pouvez définir des propriétés de contrôle standard comme FontFamily, FontSize, FontStyle, Foreground et CharacterSpacing pour modifier l’apparence du texte. Vous pouvez également utiliser des éléments de texte inline et des propriétés Typography associées pour mettre en forme votre texte. Ces options affectent uniquement la manière dont TextBlock affiche le texte localement. Par conséquent, si vous copiez et collez le texte dans un contrôle de texte enrichi, par exemple, aucune mise en forme n’est appliquée.

>**Remarque**&nbsp;&nbsp;N’oubliez pas, comme indiqué dans la section précédente, que les éléments de texte insérés et les valeurs typographiques par défaut ne sont pas restitués sur le chemin rapide.


### <a name="inline-elements"></a>Éléments inline

L’espace de noms [Windows.UI.Xaml.Documents](/uwp/api/Windows.UI.Xaml.Documents) propose une variété d’éléments de texte inline utilisables pour mettre en forme votre texte, par exemple Bold, Italic, Run, Span, et LineBreak.

Vous pouvez afficher, dans un contrôle TextBlock, une série de chaînes présentant chacune une mise en forme différente. Pour ce faire, utilisez un élément Run pour afficher chaque chaîne avec sa mise en forme et séparez chaque élément Run par un élément LineBreak.

Voici comment définir, dans un contrôle TextBlock, plusieurs chaînes de texte dont la mise en forme diffère à l’aide d’objets Run séparés par un élément LineBreak.
```xaml
<TextBlock FontFamily="Segoe UI" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Segoe UI Light" FontSize="24">
        Segoe UI Light 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Georgia" FontSize="18" FontStyle="Italic">
        Georgia Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="Black" FontFamily="Arial" FontSize="14" FontWeight="Bold">
        Arial Bold 14
    </Run>
</TextBlock>
```

Résultat :

![Texte mis en forme avec des éléments Run](images/text-block-run-examples.png)

### <a name="typography"></a>Typographie

Les propriétés associées de la classe [Typography](/uwp/api/Windows.UI.Xaml.Documents.Typography) donnent accès à un ensemble de propriétés typographiques Microsoft OpenType. Vous pouvez définir ces propriétés associées sur TextBlock ou sur des éléments de texte inline individuels. Ces exemples portent sur les deux types.
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

- [Contrôles de texte](text-controls.md)
- [Recommandations sur la vérification orthographique](text-controls.md)
- [Ajout de la fonctionnalité de recherche](search.md)
- [Recommandations sur la saisie de texte](text-controls.md)
- [TextBox, classe](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [PasswordBox Windows.UI.Xaml.Controls, classe](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length, propriété](/dotnet/api/system.string.length)
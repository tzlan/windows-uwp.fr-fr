---
description: Découvrez comment utiliser la typographie de votre application pour aider les utilisateurs à comprendre facilement le contenu.
title: Typographie des applications Windows
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3660417f6e88268dd2ad814ce4dba62354029828
ms.sourcegitcommit: 05f9bb0c563e12daaacf7732d46f1c7f578dbe6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97529286"
---
# <a name="typography-in-windows-apps"></a>Typographie des applications Windows

![Image Hero](images/header-typography.svg)

En tant que représentation visuelle du langage, la typographie a pour mission principale de communiquer des informations. Son style doit toujours être aligné sur cet objectif. Dans cet article, nous décrirons comment appliquer un style à la typographie de votre application Windows pour aider les utilisateurs à comprendre facilement et efficacement le contenu.

## <a name="font"></a>Police

Vous devez utiliser une police dans toute l’interface utilisateur de votre application, et nous vous recommandons d’utiliser la police par défaut pour les applications Windows, **Segoe UI**. Elle est conçue pour conserver une lisibilité optimale, quelles que soient les tailles et les densités en pixels. Elle se caractérise par une esthétique nette, légère et aérée en parfaite harmonie avec le contenu du système.

![Exemple de texte dans la police Segoe UI.](images/type/segoe-sample.svg)

Pour afficher les langues autres que l’anglais ou sélectionner une autre police pour votre application, consultez [Langues](#languages) et [Polices](#fonts) afin de connaître nos polices recommandées pour les applications Windows.

:::row:::
    :::column:::
![Première capture d’écran d’une barre verte comprenant une coche verte et les mots « À faire ».](images/do.svg)
Sélectionnez une seule police pour votre interface utilisateur.
    :::column-end:::
    :::column:::
![à ne pas faire](images/dont.svg) Utiliser plusieurs polices.
    :::column-end:::
:::row-end:::

## <a name="size-and-scaling"></a>Taille et mise à l’échelle

Les tailles de police utilisées dans les applications UWP sont automatiquement mises à l’échelle sur tous les appareils. L’algorithme de mise à l’échelle garantit qu’une police de 24 pixels sur un appareil Surface Hub placé à une distance de 3 mètres est aussi lisible qu’une police de 24 pixels sur un téléphone doté d’un écran 5 pouces distant de quelques centimètres.

![Distances d’affichage des différents appareils.](images/type/scaling-chart.svg)

En raison du mode de fonctionnement du système de mise à l’échelle, la conception est effectuée en pixels effectifs, et non en pixels physiques réels. En outre, il n’est pas nécessaire de modifier les tailles de police pour différentes tailles ou résolutions d’écran.

:::row:::
    :::column:::
![Deuxième capture d’écran d’une barre verte comprenant une coche verte et les mots « À faire ».](images/do.svg)
Respectez les tailles de la [gamme de caractères](#type-ramp) Windows.
    :::column-end:::
    :::column:::
![à ne pas faire](images/dont.svg) Utiliser une taille de police inférieure à12 px.
    :::column-end:::
:::row-end:::

## <a name="hierarchy"></a>Hiérarchie

:::row:::
    :::column:::
Les utilisateurs s’appuient sur la hiérarchie visuelle lors de l’analyse d’une page : les en-têtes résument le contenu et le texte du corps fournit d’autres informations. Pour créer une hiérarchie visuelle précise dans votre application, suivez la gamme de caractères Windows.
    :::column-end:::
    :::column:::
![Capture d’écran de trois lignes de texte où la taille de police devient plus petite d’une ligne à l’autre.](images/type/type-hierarchy.svg)
    :::column-end:::
:::row-end:::

### <a name="type-ramp"></a>Gamme de caractères

La gamme de caractères Windows établit des relations cruciales entre les styles de caractère sur une page, afin d’aider les utilisateurs à lire facilement le contenu. Toutes les tailles sont exprimées en pixels effectifs et sont optimisées pour les applications UWP s’exécutant sur tous les appareils.

![Gamme de caractères Windows.](images/type/type-ramp.png)

### <a name="using-the-type-ramp"></a>Utilisation de la gamme de caractères

:::row:::
    :::column:::
Vous pouvez accéder aux niveaux de la gamme de caractères en tant que [ressources statiques](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp) XAML. Les styles suivent la convention de nommage `*TextBlockStyle` présentée ici.

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```
    :::column-end:::
    :::column:::
![Capture d’écran des styles de texte Header, Subheader, Title, Subtitle, Base, Body et Caption.](images/type/text-block-type-ramp.svg)
    :::column-end:::
:::row-end:::



:::row:::
    :::column:::
![Troisième capture d’écran d’une barre verte comprenant une coche verte et les mots « À faire ».](images/do.svg)
Utilisez le style « Body » pour la majeure partie du texte.

Utilisez « Base » pour les titres lorsque l’espace est restreint.
    :::column-end:::
    :::column:::
![à ne pas faire](images/dont.svg) Utiliser « Légende » pour une action principale ou des chaînes longues.

Utiliser « En-tête » ou « Sous-titre » si le texte doit faire l’objet d’un retour automatique à la ligne.
    :::column-end:::
:::row-end:::

## <a name="alignment"></a>Alignement

La valeur [TextAlignment](/uwp/api/windows.ui.xaml.textalignment) par défaut est Left, et dans la plupart des cas, cette approche d’alignement à gauche et de non-alignement à droite garantit un ancrage cohérent du contenu et une disposition uniforme. Pour les langues RTL, voir [Ajuster la disposition et les polices pour prendre en charge la globalisation](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

![Montre un texte aligné à gauche.](images/type/alignment.svg)

```xaml
<TextBlock TextAlignment="Left">
```

## <a name="character-count"></a>Nombre de caractères

:::row:::
    :::column:::
![Quatrième capture d’écran d’une barre verte comprenant une coche verte et les mots « À faire ».](images/do.svg)
Utilisez des lignes de 50 à 60 caractères pour faciliter la lecture.
    :::column-end:::
    :::column:::
![à ne pas faire](images/dont.svg) Utiliser des lignes de moins de 20 caractères ou de plus de 60 caractères qui rendent la lecture difficile.
    :::column-end:::
:::row-end:::

## <a name="clipping-and-ellipses"></a>Détourage et ellipses

Lorsque la quantité de texte s’étend au-delà de l’espace disponible, nous vous recommandons de dérouter le texte, ce qui correspond au comportement par défaut de la plupart des [contrôles de texte UWP](../controls-and-patterns/text-controls.md).

![Cadre d’appareil avec détourage de texte.](images/type/clipping.svg)

```xaml
<TextBlock TextWrapping="WrapWholeWords" TextTrimming="Clip"/>
```

:::row:::
    :::column:::
![Cinquième capture d’écran d’une barre verte comprenant une coche verte et les mots « À faire ».](images/do.svg)
Détourez le texte et renvoyez-le automatiquement à la ligne si plusieurs lignes sont activées.
    :::column-end:::
    :::column:::
![à ne pas faire](images/dont.svg) Utiliser des ellipses pour éviter tout encombrement visuel.
    :::column-end:::
:::row-end:::

**Remarque** : Si les conteneurs ne sont pas clairement définis (par exemple, sans couleur d’arrière-plan de différenciation), ou s’il existe un lien pour afficher plus de texte, utilisez des ellipses.

## <a name="languages"></a>Langues 

Segoe UI est notre police pour l’anglais, les langues européennes, le grec, l’hébreu, l’arménien, le géorgien et l’arabe. Pour les autres langues, consultez les recommandations suivantes.

### <a name="globalizinglocalizing-fonts"></a>Polices de globalisation/localisation

Utilisez les API de [mappage de police LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont) pour l’accès par programmation à la gamme de polices, à la taille, à l’épaisseur et au style recommandés pour une langue particulière. L’objet LanguageFont assure l’accès aux informations de police appropriées pour diverses catégories de contenu, notamment les en-têtes d’interface utilisateur, les notifications, le texte de corps et les polices de corps de document modifiables par l’utilisateur. Pour plus d’informations, voir [Ajuster la disposition et les polices pour prendre en charge la globalisation](../globalizing/adjust-layout-and-fonts--and-support-rtl.md).

### <a name="fonts-for-non-latin-languages"></a>Polices pour les langues non latines

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Afrique (éthiopien, n’ko, osmanya, tifinagh, vaï).</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">Normal, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Amérique du Nord (syllabiques canadiens, cherokee).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">Normal, Semi-fin, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud-Est (bugi, lao, khmer, thaï).</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">Normale</td>
<td align="left">Police d’interface utilisateur pour le coréen.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">Normal, Gras, Fin</td>
<td align="left">Police d’interface utilisateur pour le chinois traditionnel.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">Normal, Gras, Fin</td>
<td align="left">Police d’interface utilisateur pour le chinois simplifié.</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">Normale</td>
<td align="left">Police de substitution pour le script du Myanmar.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">Normal, Semi-fin, Gras</td>
<td align="left">Police d’interface utilisateur pour les scripts d’Asie du Sud (bangla, dévanâgarî, goudjrati, gurmukhi, kannada, malayalam, odia, ol tchiki, cingalais, sora sompeng, tamoul et telugu)</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">Normale</td>
<td align="left">Police Chinese UI héritée. </td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">Clair, Semi-clair, Normal, Semi-gras, Gras</td>
<td align="left">Police d’interface utilisateur pour le japonais.</td>
</tr>
</tbody>
</table>

## <a name="fonts"></a>Fonts

### <a name="sans-serif-fonts"></a>Polices sans-serif

Les polices sans-serif sont un excellent choix pour les titres et les éléments d’interface utilisateur. 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">Normal, Italique, Gras, Italique gras, Noir</td>
<td align="left">Prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe, arménien et hébreu). L’épaisseur de police Noir prend uniquement en charge les scripts d’Europe.</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">Normal, Italique, Gras, Italique gras, Fin, Italique fin</td>
<td align="left">Prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe et hébreu). Arabe disponible uniquement en écriture verticale.</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>Normal, Italique, Gras, Italique gras</td>
<td>Police de largeur fixe qui prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>Normal, Italique, Italique fin, Italique noir, Gras, Italique gras, Fin, Semi-fin, Semi-gras, Noir</td>
<td>Police d’interface utilisateur pour les scripts d’Europe et du Moyen-Orient (arabe, arménien, cyrillique, géorgien, grec, hébreu, latin) et le script lisu.</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">Normal, Semi-fin, Fin, Gras, Semi-gras</td>
<td align="left">Police open source métriquement compatible avec Segoe UI destinée aux applications sur d’autres plateformes qui ne veulent pas intégrer Segoe UI. <a href="https://github.com/Microsoft/Selawik">Obtenir Selawik sur GitHub.</a></td>
</tr>

</tbody>
</table>

### <a name="serif-fonts"></a>Polices serif

Les polices serif sont parfaites pour présenter de grandes quantités de texte. 

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">Normale</td>
<td align="left">Police serif qui prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Police serif à largeur fixe qui prend en charge les scripts d’Europe et du Moyen-Orient (latin, grec, cyrillique, arabe, arménien et hébreu).</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Géorgie</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Prend en charge les scripts d’Europe (latin, grec et cyrillique).</td>
</tr>

<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">Normal, Italique, Gras, Italique gras</td>
<td align="left">Police héritée qui prend en charge les scripts d’Europe (latin, grec, cyrillique, arabe, arménien, hébreu).</td>
</tr>

</tbody>
</table>

### <a name="symbols-and-icons"></a>Symboles et icônes

<table>
<thead>
<tr class="header">
<th align="left">Famille de polices</th>
<th align="left">Styles</th>
<th align="left">Remarques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">Normale</td>
<td align="left">Police d’interface utilisateur pour les icônes d’application. Pour plus d’informations, voir l’article <a href="segoe-ui-symbol-font.md">Recommandations en matière d’icônes Segoe MDL2</a>.</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">Normale</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">Normale</td>
<td align="left">Police de substitution pour les symboles</td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>Articles connexes

* [Contrôles de texte](../controls-and-patterns/text-controls.md)
* [Ressources de thème XAML](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp)
* [Styles XAML](../controls-and-patterns/xaml-styles.md)
* [Typographie Microsoft](/typography/)

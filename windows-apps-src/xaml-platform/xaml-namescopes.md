---
description: Un namescope XAML stocke les relations entre les noms définis en XAML des objets et leurs instances équivalentes. Ce concept est similaire à celui dont la signification plus large correspond au terme namescope dans d’autres langages et technologies de programmation.
title: Namescopes XAML
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4fa430cdd6c2f7b47576478ec30f0f139b80363
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173753"
---
# <a name="xaml-namescopes"></a>Namescopes XAML


Un *namescope XAML* stocke les relations entre les noms définis en XAML des objets et leurs instances équivalentes. Ce concept est similaire à celui dont la signification plus large correspond au terme *namescope* dans d’autres langages et technologies de programmation.

## <a name="how-xaml-namescopes-are-defined"></a>Mode de définition des namescopes XAML

Les noms dans les namescopes XAML permettent au code utilisateur de référencer les objets qui ont été initialement déclarés en XAML. Le résultat interne de l’analyse du code XAML est que l’exécution crée un ensemble d’objets qui conservent une partie ou la totalité des relations que ces objets entretenaient dans les déclarations XAML. Ces relations sont gérées en tant que propriétés d’objets spécifiques des objets créés ou sont exposées aux méthodes utilitaires dans les API du modèle de programmation.

L’utilisation la plus fréquente d’un nom dans un namescope XAML est en tant que référence directe à une instance d’objet, laquelle est activée par une passe du compilateur du balisage en tant qu’action de génération de projet, associée à une méthode **InitializeComponent** générée dans les modèles de classe partielle.

Vous pouvez également utiliser la méthode utilitaire [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) vous-même au moment de l’exécution pour renvoyer une référence à des objets qui ont été définis avec un nom dans le balisage XAML.

### <a name="more-about-build-actions-and-xaml"></a>Plus d’informations sur les actions de génération et le langage XAML

Ce qui se passe techniquement, c’est que le code XAML lui-même subit une passe du compilateur de balisage en même temps que la classe partielle qu’il définit pour le code-behind et lui-même sont compilés ensemble. Chaque élément objet avec un attribut **Name** ou [x:Name](x-name-attribute.md) défini dans le balisage génère un champ interne portant un nom qui correspond au nom XAML. Ce champ est initialement vide. Ensuite, la classe génère une méthode **InitializeComponent** qui est appelée uniquement à l’issue du chargement de tout le code XAML. Au sein de la logique **InitializeComponent**, chaque champ interne est alors renseigné à l’aide de la valeur de retour [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) de la chaîne de nom équivalente. Vous pouvez observer cette infrastructure à titre personnel en examinant les fichiers « .g » (générés) créés pour chaque page XAML dans le sous-dossier /obj d’un projet d’application Windows Runtime à l’issue de la compilation. Vous pouvez également considérer les champs et la méthode **InitializeComponent** comme des membres de vos assemblys obtenus en y réfléchissant ou en examinant le contenu linguistique de leur interface.

**Remarque**    En particulier pour les applications Visual C++ Component extensions (C++/CX), un champ de stockage pour une référence **x :Name** n’est pas créé pour l’élément racine d’un fichier XAML. Si vous devez référencer l’objet racine à partir d’un fichier code-behind C++/CX, utilisez d’autres API ou une traversée d’arborescence. Par exemple, vous pouvez appeler [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) pour un élément enfant nommé connu avant d’appeler [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent).

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>Création d’objets au moment de l’exécution avec XamlReader.Load

Le code XAML peut également servir d’entrée de chaîne pour la méthode [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load), qui agit de façon analogue à l’opération d’analyse initiale de la source XAML. **XamlReader.Load**crée une arborescence déconnectée d’objets au moment de l’exécution. Cette arborescence déconnectée peut ensuite être attachée à un certain point de l’arborescence d’objets principale. Vous devez connecter de manière explicite votre arborescence d’objets créée, soit en l’ajoutant à une collection de propriétés de contenu telle que **Children**, soit en définissant une autre propriété qui prend une valeur d’objet (par exemple en chargeant une nouvelle classe [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) pour une valeur de propriété [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)).

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>Implications du namescope XAML de XamlReader.Load

La portée de nom XAML préliminaire définie par la nouvelle arborescence d’objets créée par [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) évalue l’unicité des noms définis dans le XAML fourni. S’il existe des noms dans le code XAML fourni qui ne sont pas uniques en interne à ce stade, **XamlReader.Load** lève une exception. L’arborescence d’objets déconnectée n’essaie pas de fusionner son namescope XAML avec le namescope XAML de l’application principale, si ou lorsqu’elle est connectée à l’arborescence d’objets de l’application principale. Après avoir connecté les arborescences, votre application possède une arborescence d’objets unifiée, mais celle-ci comporte des namescopes XAML discrets. Les divisions ont lieu aux points de connexion entre les objets, où vous définissez une propriété en tant que valeur renvoyée à partir d’un appel **XamlReader.Load**.

La complication des portées de code XAML discrètes et déconnectées est que les appels à la méthode [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) et aux références d’objets managés directs ne fonctionnent plus par rapport à une portée de code XAML unifiée. Au contraire, l’objet particulier sur lequel la méthode **FindName** est appelée implique l’étendue, en sachant que l’étendue correspond au namescope XAML dans lequel se trouve l’objet appelant. Dans le cas d’une référence d’objet managé directe, l’étendue est impliquée par la classe dans laquelle le code existe. En général, le code-behind d’interaction de l’exécution d’une « page » de contenu d’application existe dans la classe partielle qui stocke la « page » racine ; par conséquent, le namescope XAML est le namescope XAML racine.

Si vous appelez [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) pour obtenir un objet nommé dans le namescope XAML racine, la méthode de trouve pas les objets provenant d’un namescope XAML discret créé par [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load). Inversement, si vous appelez **FindName** à partir d’un objet obtenu depuis l’extérieur du namescope XAML discret, la méthode ne trouve pas les objets nommés dans le namescope XAML racine.

Ce problème de namescope XAML discret affecte uniquement la recherche d’objets par nom dans des namescopes XAML dans le cadre de l’utilisation de l’appel [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname).

Pour obtenir les références à des objets définis dans un autre namescope XAML, vous pouvez utiliser plusieurs techniques :

-   Parcourez toute l’arborescence par étapes discrètes avec la propriété [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent) et/ou les propriétés de collection connues pour exister dans la structure de votre arborescence d’objets (telle que la collection renvoyée par [**Panel.Children**](/uwp/api/windows.ui.xaml.controls.panel.children)).
-   Si vous appelez depuis un namescope XAML discret alors que vous souhaitez le namescope XAML racine, il est toujours facile d’obtenir une référence à la fenêtre principale actuellement affichée. Vous pouvez obtenir la racine visuelle (l’élément XAML racine, également connu sous le nom de source de contenu) depuis la fenêtre d’application actuelle dans une seule ligne de code à l’aide de l’appel `Window.Current.Content`. Vous pouvez alors effectuer un transtypage vers [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) et appeler [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) depuis cette étendue.
-   Si vous appelez à partir de la portée de code XAML racine et souhaitez un objet dans une portée de code XAML discrète, il est préférable de planifier à l’avance votre code et de conserver une référence à l’objet qui a été retourné par [**XamlReader. Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) , puis ajouté à l’arborescence d’objets principale. Cet objet est maintenant un objet valide pour appeler [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) au sein du namescope XAML discret. Vous pouvez garder cet objet disponible en tant que variable globale ou bien la passer à l’aide de paramètres de méthode.
-   Vous pouvez totalement éviter les considérations liées aux noms et aux namescopes XAML en examinant l’arborescence visuelle. L’API [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) vous permet de balayer l’arborescence visuelle en termes d’objets parents et de collections enfants, purement selon la position et de l’index.

## <a name="xaml-namescopes-in-templates"></a>Namescopes XAML dans les modèles

Les modèles en langage XAML offrent la possibilité de réutiliser et réappliquer du contenu de manière directe, mais les modèles peuvent également inclure des éléments avec des noms définis au niveau du modèle. Ce même modèle peut être utilisé plusieurs fois dans une page. C’est pourquoi les modèles définissent leurs propres namescopes XAML, indépendamment de la page qui les contient dans laquelle le style ou modèle est appliqué. Prenons l’exemple suivant :

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

Ici, le même modèle est appliqué à deux contrôles différents. Si les modèles n’avaient pas de namescopes XAML discrets, le nom « MyTextBlock » utilisé dans le modèle entraînerait un conflit de noms. Chaque instanciation du modèle a sa propre portée de nom XAML. Ainsi, dans cet exemple, la portée de nom XAML de chaque modèle instancié contient un et un seul nom. Toutefois, le namescope XAML racine ne contient pas le nom de l’un ou l’autre des modèles.

En raison des namescopes XAML distincts, la recherche d’éléments nommés au sein d’un modèle à partir de l’étendue de la page dans laquelle le modèle est appliqué requiert une autre technique. Au lieu d’appeler [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) sur un objet dans l’arborescence des objets, vous obtenez tout d’abord l’objet auquel le modèle est appliqué, puis appelez [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild). Si vous êtes l’auteur d’un contrôle et que vous générez une convention selon laquelle un élément nommé particulier dans un modèle appliqué est la cible d’un comportement défini par le contrôle lui-même, vous pouvez utiliser la méthode **GetTemplateChild** de votre code d’implémentation du contrôle. La méthode **GetTemplateChild** est protégée, donc seul l’auteur du contrôle y a accès. En outre, il existe des conventions que les auteurs de contrôles doivent suivre afin de nommer des composants et des composants de modèles pour signaler ces derniers comme des valeurs d’attribut appliquées à la classe des contrôles. Cette technique rend détectables les noms des composants importants par les utilisateurs des contrôles, susceptibles de vouloir appliquer un autre modèle, ce qui impliquerait de remplacer les composants nommés afin de conserver les fonctionnalités des contrôles.

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Attribut x:Name](x-name-attribute.md)
* [Démarrage rapide : modèles de contrôle](/previous-versions/windows/apps/hh465374(v=win.10))
* [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)
* [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)
 
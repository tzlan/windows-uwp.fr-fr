---
description: Découvrez comment intégrer des images dans votre application et comment utiliser les API des deux principales classes XAML, Image et ImageBrush.
title: Images et pinceaux image
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7cbe6de9f6c01ee2adca8e9aa716c92491c04e8a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033442"
---
# <a name="images-and-image-brushes"></a>Images et pinceaux image

Pour afficher une image, vous pouvez utiliser l’objet **Image** ou l’objet **ImageBrush**. Un objet Image affiche une image, tandis qu’un objet ImageBrush peint un autre objet avec une image. 

> **API importantes**  : [classe Image](/uwp/api/Windows.UI.Xaml.Controls.Image), [propriété Source](/uwp/api/windows.ui.xaml.controls.image.source), [classe ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush), [propriété ImageSource](/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)

## <a name="are-these-the-right-elements"></a>S’agit-il des éléments appropriés ?
Utilisez un élément **Image** pour afficher une image autonome dans votre application.

Utilisez un élément **ImageBrush** pour appliquer une image à un autre objet. L’utilisation d’un objet ImageBrush comprend des effets d’ornement pour le texte ou des arrière-plans pour les contrôles ou les conteneurs de disposition.

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l'application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour <a href="xamlcontrolsgallery:/item/Image">ouvrir l’application et voir l'Image en action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>Créer une image

### <a name="image"></a>SE conteneur
Cet exemple montre comment créer une image à l’aide de l’objet [Image](/uwp/api/Windows.UI.Xaml.Controls.Image).


```XAML
<Image Width="200" Source="sunset.jpg" />
```

Voici l’objet Image affiché.

![Exemple d’un élément d’image](images/Image_Licorice.jpg)

Dans cet exemple, la propriété [Source](/uwp/api/windows.ui.xaml.controls.image.source) indique l’emplacement de l’image à afficher. Vous pouvez définir la propriété Source en spécifiant une URL absolue (par exemple, http://contoso.com/myPicture.jpg) ) ou en spécifiant une URL relative à la structure de votre package d’application. Dans le cadre de notre exemple, nous plaçons le fichier image « licorice.jpg » dans le fichier racine de notre projet, puis nous déclarons les paramètres du projet qui incluent le fichier image en tant que contenu.

### <a name="imagebrush"></a>ImageBrush

Avec l’objet [ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush), vous pouvez utiliser une image pour peindre une zone qui accepte un objet [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush). Par exemple, vous pouvez utiliser un objet ImageBrush pour la valeur de la propriété [Fill](/uwp/api/windows.ui.xaml.shapes.shape.fill) d’un objet [Ellipse](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) ou la propriété [Background](/uwp/api/windows.ui.xaml.controls.control.background) d’un objet [Canvas](/uwp/api/Windows.UI.Xaml.Controls.Canvas).

L’exemple suivant montre comment utiliser un objet ImageBrush pour peindre un objet Ellipse.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Voici l’objet Ellipse peint par l’objet ImageBrush.

![Objet Ellipse peint par un objet ImageBrush.](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>Étirer une image

Si vous ne définissez pas les valeurs [Width](/uwp/api/windows.ui.xaml.frameworkelement.width) ou [Height](/uwp/api/windows.ui.xaml.frameworkelement.height) d’un objet **Image** , ce dernier est affiché avec les dimensions de l’image spécifiée par la propriété **Source**. Le fait de définir les valeurs **Width** et **Height** crée une zone rectangulaire dans laquelle l’image est affichée. Vous pouvez spécifier la façon dont l’image remplit cette zone à l’aide de la propriété [Stretch](/uwp/api/windows.ui.xaml.controls.image.stretch). La propriété Stretch accepte ces valeurs qui sont définies par l’énumération [Stretch](/uwp/api/Windows.UI.Xaml.Media.Stretch) :

-   **Aucune**  : L’image n’est pas étirée pour remplir les dimensions de sortie. Faites preuve de prudence avec le paramètre Stretch : si l’image source est plus grande que la zone qui doit la contenir, votre image sera découpée. Cela n’est pas généralement souhaitable car, contrairement à une propriété [Clip](/uwp/api/windows.ui.xaml.uielement.clip) délibérée, vous n’avez aucun contrôle sur la fenêtre d’affichage.
-   **Uniform**  : L’image est mise à l’échelle afin de correspondre aux dimensions de sortie. Mais les proportions du contenu sont conservées. Il s'agit de la valeur par défaut.
-   **UniformToFill** : L’image est mise à l’échelle de sorte qu’elle remplisse complètement la zone de sortie tout en conservant ses proportions d’origine.
-   **Fill**  : L’image est mise à l’échelle afin de correspondre aux dimensions de sortie. Étant donné que la hauteur et la largeur du contenu sont mises à l’échelle indépendamment, les dimensions d’origine de l’image risquent de ne pas être conservées. C’est-à-dire que l’image risque d’être déformée afin de remplir complètement la zone de sortie.

![Exemple de paramètres d’étirement.](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>Rogner une image

Vous pouvez utiliser la propriété [Clip](/uwp/api/windows.ui.xaml.uielement.clip) pour découper une zone de la sortie image. Vous affectez à la propriété Clip la valeur [Geometry](/uwp/api/Windows.UI.Xaml.Media.Geometry). Le découpage non rectangulaire n’est actuellement pas pris en charge.

L’exemple suivant montre comment utiliser un objet [RectangleGeometry](/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry) en tant que région de découpage pour une image. Dans cet exemple, nous définissons un objet **Image** et affectons à la propriété Height la valeur 200. Un objet **RectangleGeometry** définit un rectangle pour la zone de l’image à afficher. La propriété [Rect](/uwp/api/windows.ui.xaml.media.rectanglegeometry.rect) a la valeur 25,25,100,150, ce qui définit un rectangle avec pour position de départ 25,25, une largeur de 100 et une hauteur de 150. Seule la partie de l’image qui se trouve dans la zone du rectangle est affichée.

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

Voici l’image découpée sur un arrière-plan noir.

![Objet Image rogné par un objet RectangleGeometry.](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>Appliquer une opacité

Vous pouvez appliquer une propriété [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) à une image de façon à ce qu’elle soit affichée de manière semi-translucide. Les valeurs d’opacité vont de 0,0 à 1,0, où 1,0 correspond à une opacité totale et 0,0 correspond à une transparence totale. Cet exemple montre comment appliquer une opacité de 0,5 à un objet Image.

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

Voici l’objet Image affiché avec une opacité de 0,5 et un arrière-plan noir laissant passer l’opacité partielle.

![Objet Image avec une opacité de 0,5.](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>Formats de fichier d’image

Les objets **Image** et **ImageBrush** permettent d’afficher les formats de fichier d’image suivants :

-   Joint Photographic Experts Group (JPEG)
-   format PNG (Portable Network Graphics)
-   image bitmap (BMP)
-   format GIF (Graphics Interchange Format)
-   format TIFF (Tagged Image File Format)
-   JPEG XR
-   icônes (ICO)

L’API pour [Image](/uwp/api/Windows.UI.Xaml.Controls.Image), [BitmapImage](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) et [BitmapSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) n’inclut aucune méthode dédiée d’encodage ou de décodage des formats de média. Toutes les opérations d’encodage et de décodage sont intégrées et, au plus, feront émerger les aspects de l’encodage ou du décodage dans le cadre des données d’événement pour les événements de chargement. Si vous voulez effectuer des tâches spéciales avec encodage ou décodage d’image, que vous pouvez utiliser si votre application effectue des conversions ou des manipulations d’image, vous devez utiliser les API disponibles dans l’espace de noms [Windows.Graphics.Imaging](/uwp/api/Windows.Graphics.Imaging). Ces API sont également prises en charge par le composant Imagerie Windows (WIC) de Windows.

À compter de Windows 10, version 1607, l’élément **Image** prend en charge les images GIF animées. Quand vous utilisez un **BitmapImage** en tant qu’image **Source** , vous pouvez accéder aux APIs BitmapImage pour contrôler la lecture de l’image GIF animée. Pour plus d’informations, voir les remarques dans la page de la classe [BitmapImage](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage).

> **Remarque**&nbsp;&nbsp;La prise en charge des images GIF animées est disponible quand votre application est compilée pour Windows 10, version 1607 et qu’elle s’exécute sur la version 1607 (ou version ultérieure). Quand votre application est compilée pour des versions antérieures ou qu’elle s’exécute sur ces versions, la première image de l’image GIF s’affiche, mais elle n’est pas animée.

Pour plus d’informations sur les ressources d’application et la création d’un package de sources d’image dans une application, voir [Définition des ressources d’application](/previous-versions/windows/apps/hh965321(v=win.10)).

### <a name="writeablebitmap"></a>WriteableBitmap

Un élément [WriteableBitmap](/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) fournit un élément [BitmapSource](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) qui peut être modifié et qui n’utilise pas le décodage standard basé sur les fichiers à partir du composant WIC. Vous pouvez modifier des images dynamiquement et réafficher l’image mise à jour. Pour définir le contenu du tampon d’un élément **WriteableBitmap** , utilisez la propriété [PixelBuffer](/uwp/api/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer) pour accéder au tampon et utilisez un flux ou un type de tampon propre à un langage pour le remplir. Pour obtenir un exemple de code, voir [WriteableBitmap](/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap).

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

La classe [RenderTargetBitmap](/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) peut capturer l’arborescence de l’interface utilisateur XAML d’une application en cours d’exécution, puis représenter une source d’image bitmap. Après la capture, cette source d’image peut être appliquée à d’autres parties de l’application, enregistrée en tant que ressource ou données d’application par l’utilisateur, ou utilisée à d’autres fins. Il peut notamment être très utile de créer une miniature au moment de l’exécution d’une page XAML pour un schéma de navigation, par exemple dans le but de fournir un lien vers une image à partir d’un contrôle [Hub](/uwp/api/Windows.UI.Xaml.Controls.Hub). **RenderTargetBitmap** limite toutefois le contenu pouvant apparaître dans l’image capturée. Pour plus d’informations, voir la rubrique de référence de l’API pour [RenderTargetBitmap](/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap).

### <a name="image-sources-and-scaling"></a>Sources d’images et mise à l’échelle

Vous devez créer vos sources d’images selon plusieurs tailles recommandées afin de vous assurer que votre application garde son aspect esthétique lorsque Windows la redimensionne. Lorsque vous spécifiez une propriété **Source** pour un objet **Image** , vous pouvez utiliser une convention d’affectation de noms qui référencera automatiquement la ressource appropriée pour la mise à l’échelle actuelle. Pour connaître les spécificités de la convention d’affectation de noms et obtenir plus d’informations, voir [Démarrage rapide : Utilisation de ressources de fichiers ou d’images](/previous-versions/windows/apps/hh965325(v=win.10)).

Pour plus d’informations sur la conception prenant en charge la mise à l’échelle, voir [Recommandations en matière d’expérience utilisateur pour la disposition et la mise à l’échelle](https://developer.microsoft.com/windows/apps/design).

### <a name="image-and-imagebrush-in-code"></a>Image et ImageBrush dans le code

Il est courant de spécifier les éléments Image et ImageBrush en XAML plutôt que sous forme de code. Cela est dû au fait que ces éléments sont souvent la sortie d’outils de conception dans le cadre d’une définition d’interface utilisateur XAML.

Si vous définissez un élément Image ou ImageBrush à l’aide de code, utilisez les constructeurs par défaut, puis définissez la propriété Source appropriée ([Image.Source](/uwp/api/windows.ui.xaml.controls.image.source) ou [ImageBrush.ImageSource](/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)). Celle-ci nécessite un élément [BitmapImage](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (et non un URI) quand vous la définissez à l’aide de code. Si votre source est un flux, utilisez la méthode [SetSourceAsync](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) pour initialiser la valeur. Si votre source est un URI incluant du contenu de votre application qui utilise les modèles **ms-appx** ou **ms-resource** , utilisez le constructeur [BitmapImage](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) qui prend un URI. Vous pouvez également envisager de gérer l’événement [ImageOpened](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened) s’il existe des problèmes de délai liés à la récupération ou au décodage de la source de l’image, auquel cas un contenu alternatif peut s’avérer nécessaire à afficher tant que la source de l’image n’est pas disponible. Pour obtenir un exemple de code, consultez [Galerie de contrôles XAML](/samples/microsoft/xaml-controls-gallery/xaml-controls-gallery/).

> [!NOTE]
> Si vous établissez des images à l’aide de code, vous pouvez utiliser la gestion automatique pour accéder à des ressources non qualifiées avec les qualificateurs d’échelle et de culture actuels, ou vous pouvez utiliser [ResourceManager](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) et [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) avec des qualificateurs pour la culture et l’échelle afin d’obtenir les ressources directement. Pour plus d’informations, voir [Système de gestion des ressources](/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="get-the-sample-code"></a>Obtenir l’exemple de code

- [Exemple de Galerie de contrôles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) : tous les contrôles XAML dans un format interactif.

## <a name="related-articles"></a>Articles connexes

-   [Audio, vidéo et appareil photo](../../audio-video-camera/index.md)
-   [Classe Image](/uwp/api/Windows.UI.Xaml.Controls.Image)
-   [Classe ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)

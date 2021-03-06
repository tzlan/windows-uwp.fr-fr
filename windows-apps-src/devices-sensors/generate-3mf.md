---
description: Décrit la structure du type de fichier 3D Manufacturing Format, ainsi que les procédures de création et de manipulation de ce type de fichier avec l’API Windows.Graphics.Printing3D.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Générer un package 3MF
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7beafb754ead837dd9218d4a9e0223334e12bfb1
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034652"
---
# <a name="generate-a-3mf-package"></a>Générer un package 3MF

**API importantes**

-   [**Windows. Graphics. Printing3D**](/uwp/api/windows.graphics.printing3d)

Ce guide décrit la structure du document 3D Manufacturing Format, ainsi que les procédures de création et de manipulation de ce type de fichier avec l’API [**Windows.Graphics.Printing3D**](/uwp/api/windows.graphics.printing3d).

## <a name="what-is-3mf"></a>Qu’est-ce que 3MF ?

3MF (3D Manufacturing Format) est un ensemble de conventions régissant la description en langage XML de l’apparence et de la structure de modèles 3D à des fins de fabrication (impression 3D). Ce format définit une série de pièces (dont certaines sont requises et d’autres facultatives) et leurs relations, de façon à fournir toutes les informations nécessaires à un périphérique de fabrication 3D. Un jeu de données conforme au format 3MF peut être enregistré dans un fichier présentant l’extension .3mf.

Dans Windows 10, la classe [**Printing3D3MFPackage**](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage) de l’espace de noms **Windows.Graphics.Printing3D** est analogue à un fichier .3mf unique, tandis que d’autres classes sont mappées sur les différents éléments XML de ce fichier. Ce guide décrit la façon dont chacune des parties principales d’un document 3MF peut être créée et définie par programmation, le mode d’utilisation de l’extension 3MF Materials, ainsi que les procédures de conversion et d’enregistrement d’un objet **Printing3D3MFPackage** en fichier .3mf. Pour plus d’informations sur les normes 3MF ou sur l’extension 3MF Materials, voir la [Spécification 3MF](https://3mf.io/what-is-3mf/3mf-specification/).

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>Classes principales de la structure 3MF

La classe **Printing3D3MFPackage** représente un document 3MF complet, au cœur duquel se trouve la partie modèle, représentée par la classe [**Printing3DModel**](/uwp/api/windows.graphics.printing3d.printing3dmodel). Nous spécifierons la plupart des informations relatives au modèle 3D considéré en définissant les propriétés de la classe **Printing3DModel** et les propriétés de leurs classes sous-jacentes.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetInitClasses":::

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>Métadonnées

La partie modèle d’un document 3MF peut contenir des métadonnées sous la forme de paires de chaînes clé/valeur stockées dans la propriété **Metadata** . Il existe un certain nombre de noms de métadonnées prédéfinis, mais d’autres paires peuvent être ajoutées sous la forme d’une extension (décrite plus en détail dans la [spécification 3MF](https://3mf.io/what-is-3mf/3mf-specification/)). Le destinataire du package (périphérique de fabrication 3D) est chargé de déterminer si et comment les métadonnées doivent être traitées, mais il est conseillé d’inclure dans le package 3MF le maximum d’informations de base :

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetMetadata":::

## <a name="mesh-data"></a>Données de maillage

Dans le contexte de ce guide, un maillage est un corps de géométrie en 3 dimensions construit à partir d’un seul ensemble de vertex (même s’il n’a pas besoin d’apparaître sous la forme d’un solide unique). Une partie maillage est représentée par la classe [**Printing3DMesh**](/uwp/api/windows.graphics.printing3d.printing3dmesh). Un objet de maillage valide doit contenir des informations sur l’emplacement de tous ses vertex et de toutes les faces triangulaires figurant entre certains ensembles de vertex.

La méthode suivante ajoute des vertex à un maillage et leur attribue des emplacements dans l’espace 3D :

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetVertices":::

La méthode suivante définit tous les triangles qui doivent être dessinés entre ces sommets :

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetTriangleIndices":::

> [!NOTE]
> Les index de tous les triangles doivent être définis dans le sens inverse des aiguilles d’une montre (quand on regarde le triangle à partir de l’extérieur de l’objet de maillage), afin que leurs vecteurs normaux à la face pointent sur l’extérieur.

Une fois qu’un objet Printing3DMesh contient des ensembles valides de sommets et de triangles, il doit être ajouté à la propriété **Meshes** du modèle. Tous les objets **Printing3DMesh** d’un package doivent être stockés sous la propriété **Meshes** de la classe **Printing3DModel** .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetMeshAdd":::


## <a name="create-materials"></a>Créer des matériaux


Un modèle 3D peut contenir des données concernant différents matériaux. Cette convention est destinée à tirer parti des périphériques de fabrication 3D qui peuvent utiliser plusieurs matériaux dans le cadre d’un même travail d’impression. Il existe également plusieurs *types* de groupe de matériaux, chacun d’eux pouvant prendre en charge un certain nombre de matériaux spécifiques. Chaque groupe de matériaux doit comporter un numéro d’identification de référence unique, et chaque matériau figurant dans un groupe doit également être doté d’un identifiant unique.

Les différents objets de maillage au sein d’un modèle peuvent alors référencer ces matériaux. En outre, les divers triangles de chaque maillage peuvent spécifier différents matériaux. Il est même possible de représenter plusieurs matériaux à l’intérieur d’un même triangle, en attribuant un matériau différent à chacun des vertex du triangle et en calculant le matériau de la face comme correspondant au gradient entre ces derniers.

Ce guide commence par expliquer comment créer différents types de matériaux au sein de leurs groupes respectifs et comment les stocker sous forme de ressources sur l’objet de modèle. Nous étudierons ensuite la procédure d’attribution de différents matériaux à des maillages et triangles spécifiques.

### <a name="base-materials"></a>Matériaux de base

Le type de matériau par défaut est le type **Matériau de base** , qui comporte à la fois une valeur **Matériau de couleur** (décrite ci-après) et un attribut de nom destiné à spécifier le *type* de matériau à utiliser.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetBaseMaterialGroup":::

> [!NOTE]
> Le périphérique de fabrication 3D détermine le mappage entre les matériaux physiques disponibles et les éléments de matériau virtuels stockés dans le fichier 3MF. Le mappage des matériaux n’est pas nécessairement de type 1:1. En effet, si une imprimante 3D n’utilise qu’un seul matériau, elle imprimera la totalité du modèle dans ce matériau, quels que soient les différents matériaux attribués à des objets ou faces spécifiques.

### <a name="color-materials"></a>Matériaux couleur

Les **matériaux de couleur** sont semblables aux **matériaux de base** , mais n’indiquent pas de nom. Ils ne fournissent donc aucune instruction sur le type de matériau à utiliser par le périphérique. Ils ne comportent que des données de couleur et laissent le périphérique choisir le type de matériau (le périphérique peut alors demander à l’utilisateur d’effectuer lui-même ce choix). Dans le code ci-dessous, les objets `colrMat` issus de la méthode précédente sont utilisés séparément.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetColorMaterialGroup":::

### <a name="composite-materials"></a>Matériaux composites

Les **matériaux composites** demandent simplement au périphérique de fabrication de combiner uniformément différents **matériaux de base** . Chaque **groupe de matériaux composites** doit référencer très précisément un **groupe de matériaux de base** à partir duquel les ingrédients seront dessinés. En outre, les **matériaux de base** au sein de ce groupe qui doivent être disponibles doivent être répertoriés dans une liste d’ **index de matériau** , que chaque **matériau composite** référencera alors lors de la spécification des proportions (chaque **matériau composite** constitue simplement une proportion de **matériaux de base** ).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetCompositeMaterialGroup":::

### <a name="texture-coordinate-materials"></a>Matériaux de coordonnées de texture

3MF prend en charge l’utilisation d’images 2D pour la coloration des surfaces de modèles 3D. De cette façon, le modèle peut véhiculer beaucoup plus de données de couleur par face triangulaire (plutôt qu’une seule valeur de couleur par vertex de triangle). À l’instar des **matériaux de couleur** , les matériaux de coordonnées de texture transmettent uniquement des données de couleur. Pour utiliser une texture 2D, une ressource de texture doit d’abord être déclarée :

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetTextureResource":::

> [!NOTE]
> Les données de texture appartiennent au package 3MF proprement dit, et non à la partie modèle du package.

Nous renseignons ensuite les **matériaux Texture3Coord** . Chacun d’eux référence une ressource de texture et spécifie un point spécifique sur l’image (en coordonnées UV).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetTexture2CoordMaterialGroup":::

## <a name="map-materials-to-faces"></a>Mapper des matériaux sur les faces

Afin de préciser les matériaux mappés sur les vertex de chaque triangle, nous devons effectuer quelques opérations supplémentaires sur l’objet de maillage de notre modèle (si un modèle contient plusieurs maillages, les matériaux doivent être attribués séparément à chacun d’eux). Comme indiqué précédemment, les matériaux sont attribués par vertex et par triangle. Le code ci-dessous vous indique la façon dont ces informations sont entrées et interprétées.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetMaterialIndices":::

## <a name="components-and-build"></a>Composants et builds

La structure des composants permet à l’utilisateur de placer plusieurs objets de maillage dans un modèle 3D imprimable. Un objet [**Printing3DComponent**](/uwp/api/windows.graphics.printing3d.printing3dcomponent) contient un seul maillage et une liste de références à d’autres composants. Il s’agit en fait d’une liste d’objets [**Printing3DComponentWithMatrix**](/uwp/api/windows.graphics.printing3d.printing3dcomponentwithmatrix) . Chaque objet **Printing3DComponentWithMatrix** contient un élément **Printing3DComponent** et, plus important encore, une matrice de transformation qui s’applique au maillage et aux composants de cet élément **Printing3DComponent** .

Par exemple, un modèle de voiture peut être constitué d’un élément **Printing3DComponent** « Châssis » qui contient le maillage du châssis de la voiture. Le composant « Châssis » peut alors comporter des références à quatre objets **Printing3DComponentWithMatrix** distincts, qui référencent tous le même élément **Printing3DComponent** avec le maillage « Roue » et contiennent quatre matrices de transformation (mappant les roues sur quatre positions différentes du châssis de la voiture). Dans ce scénario, le maillage « Châssis » et le maillage « Roue » ne doivent être stockés qu’une seule fois, même si le produit final comporte cinq maillages au total.

Tous les objets **Printing3DComponent** doivent être directement référencés dans la propriété **Components** du modèle. Le composant à utiliser spécifiquement dans le travail d’impression est stocké dans la propriété **Build** .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetComponents":::

## <a name="save-package"></a>Enregistrer un package
Une fois que nous disposons d’un modèle dont nous avons défini les matériaux et les composants, nous pouvons l’enregistrer dans le package.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetSavePackage":::

Cette fonction garantit que la texture est correctement spécifiée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetFixTexture":::

Nous pouvons alors initialiser un travail d’impression dans l’application (voir [Impression 3D à partir de votre application](./3d-print-from-app.md)), ou enregistrer cet objet **Printing3D3MFPackage** sous la forme d’un fichier .3mf.

La méthode suivante sélectionne un objet **Printing3D3MFPackage** finalisé et enregistre ses données dans un fichier .3mf.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/devices-sensors/3dprinthowto/cs/Generate3MFMethods.cs" id="SnippetSaveTo3mf":::

## <a name="related-topics"></a>Rubriques connexes

[impression 3D à partir de votre application](./3d-print-from-app.md)  
[Exemple d’impression 3D UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 

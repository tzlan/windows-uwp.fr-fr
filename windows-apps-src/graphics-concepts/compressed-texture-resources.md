---
title: Ressources de texture compressées
description: Les mappages de textures sont des images numérisées dessinées sur des formes 3D pour ajouter des détails visuels.
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords:
- Ressources de texture compressées
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9318479fffc415e94407166bd1be20a93691a179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646214"
---
# <a name="compressed-texture-resources"></a>Ressources de texture compressées


Les mappages de textures sont des images numérisées dessinées sur des formes 3D pour ajouter des détails visuels. Ils sont associés à ces formes lors de la rastérisation et le processus peut consommer de grandes quantités de bus système et de mémoire. Pour réduire la quantité de mémoire consommée par les textures, Direct3D prend en charge la compression des surfaces de texture. Certains appareils Direct3D prennent en charge des surfaces de texture compressées en mode natif. Sur ces appareils, une fois que vous avez créé une surface compressée et que vous y avez chargé les données, la surface peut être utilisée dans Direct3D comme n'importe quelle autre surface de texture. Direct3D gère la décompression lorsque la texture est mappée à un objet 3D.

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>Compression de l’efficacité et la texture du stockage


Tous les formats de compression de texture utilisent des puissances de deux. Cela ne signifie pas qu’une texture doit être nécessairement carrée, mais simplement que x et y sont des puissances de deux. Par exemple, si la texture initiale est de 512 par 128 octets, le prochain mipmap serait de 256 x 64 et ainsi de suite, chaque niveau diminuant d’une puissance de deux. À des niveaux inférieurs où la texture est filtrée sur 16 par 2 et 8 par 1, il y aura une perte de bits car le bloc de compression est toujours un bloc de 4 x 4 texels. Les parties inutilisées du bloc sont remplies.

Bien que l'on observe une perte de bits aux niveaux les plus bas, le gain reste globalement significatif. En théorie, le pire des cas serait une texture de 2 Ko par 1 (puissance 2⁰). Ici, une seule ligne de pixels est codée par bloc, le reste du bloc étant inutilisé.

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>Mélange de formats dans une texture unique


Il est important de noter que toute texture unique doit spécifier que ses données sont stockées au format 64 ou 128 bits par groupe d’éléments de 16 texels. Si des blocs de 64 bits (format de compression de bloc BC1) sont utilisés pour la texture, il est possible de combiner les formats opaque et alpha 1 bit pour chaque bloc au sein de la même texture. En d’autres termes, la comparaison de l’ampleur de l’entier non signé de couleur\_0 et la couleur\_1 est effectuée de façon unique pour chaque bloc de 16 texels.

Dès lors que des blocs de 128 bits sont utilisés, le canal alpha doit être spécifié au format explicite (BC2) ou en mode interpolé (format BC3) pour l'ensemble de la texture. Comme pour la couleur, lorsque le mode interpolé (format BC3) est sélectionné, il est possible d'appliquer, bloc par bloc, le mode huit valeurs alpha interpolées ou six valeurs alpha interpolées. À nouveau la comparaison de grandeur d’une valeur alpha\_alpha et 0\_1 s’effectue de façon unique sur une base de bloc par bloc.

Direct3D fournit des services permettant de compresser les surfaces qui sont utilisées pour les textures de modèles 3D. Cette section fournit des informations sur la création et la manipulation des données dans une surface de textures compressées.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="opaque-and-1-bit-alpha-textures.md">Opaques et 1 bits textures alphabétiques</a></p></td>
<td align="left"><p>Le format BC1 est dédié aux structures opaques ou présentant une couleur transparente unique.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures-with-alpha-channels.md">Textures des canaux alpha</a></p></td>
<td align="left"><p>Il existe deux façons de coder les mappages de texture qui présentent une transparence plus complexe. Dans chaque cas, un bloc décrivant la transparence précède le bloc de 64 bits déjà décrit. La transparence est représentée sous forme d’un bitmap 4 x 4 avec 4 bits par pixel (codage explicite) ou avec moins de bits et une interpolation linéaire similaire à ce qui est utilisé pour le codage de couleurs.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="block-compression.md">Compression de bloc</a></p></td>
<td align="left"><p>La compression de blocs est une technique de compression de textures avec perte qui permet de réduire la taille de la texture et l'encombrement de la mémoire de manière à produire de meilleures performances. Une texture à blocs compressés peut être plus petite qu'une texture avec 32 bits par couleur.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="compressed-texture-formats.md">Formats de texture compressé</a></p></td>
<td align="left"><p>Cette section contient des informations sur l’organisation interne des formats de texture compressés. Vous n'avez pas besoin de ces informations pour utiliser des textures compressées, car vous pouvez utiliser les fonctions Direct3D pour effectuer la conversion entre les formats compressés. Ces informations vous seront cependant utiles si vous souhaitez utiliser directement des données de surface compressées.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Textures](textures.md)

 

 





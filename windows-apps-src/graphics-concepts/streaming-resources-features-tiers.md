---
title: Niveaux de fonctionnalité des ressources de diffusion en continu
description: Direct3D prend en charge les ressources de diffusion en continu sur trois niveaux de fonctionnalités.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Niveaux de fonctionnalité des ressources de diffusion en continu
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 35fcedcdfa6470a4c17c964cd0ae341e1ccf3b3a
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652868"
---
# <a name="streaming-resources-features-tiers"></a>Niveaux de fonctionnalité des ressources de diffusion en continu


Direct3D prend en charge les ressources de diffusion en continu sur trois niveaux de fonctionnalités.

Le niveau1 fournit des fonctionnalités de base pour les ressources de diffusion en continu.

La niveau2 ajoute des fonctionnalités par rapport au niveau1, telles que la garantie d'un mipmap de texture non compressé lorsque la taille est au minimum de forme de tuile standard, les instructions de nuanceur pour le niveau de détail Clamp et pour obtenir l'état de l’opération du nuanceur et la lecture à partir de vignettes mappées NULL qui ont échantillonné une valeur de zéro.

Le niveau3 ajoute des fonctionnalités Texture3D, au-delà du niveau2.

Les fonctions de requête sont disponibles dans les versions de Direct3D, pour valider la prise en charge du matériel et des pilotes pour les ressources de diffusion en continu selon le niveau.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tier-1.md">Niveau1</a></p></td>
<td align="left"><p>Cette section décrit la prise en charge du niveau1.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Niveau2</a></p></td>
<td align="left"><p>La prise en charge du niveau2 pour les ressources de diffusion en continu ajoute des fonctionnalités par rapport au niveau1, telles que la garantie d'un mipmap de texture non compressé lorsque la taille est au minimum de forme de tuile standard, les instructions de nuanceur pour le niveau de détail Clamp et pour obtenir l'état de l’opération du nuanceur et la lecture à partir de vignettes mappées NULL qui ont échantillonné une valeur de zéro.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Niveau3</a></p></td>
<td align="left"><p>Le niveau3 comprend la prise en charge de Texture3D pour les ressources de diffusion en continu, en plus des capacités de <a href="tier-2.md">niveau2</a> .</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Ressources de diffusion en continu](streaming-resources.md)

 

 




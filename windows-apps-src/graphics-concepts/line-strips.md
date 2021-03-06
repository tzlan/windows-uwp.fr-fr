---
title: Bandes de lignes
description: Une bande de lignes est une primitive composée de segments de ligne connectés. Votre application peut utiliser des bandes de lignes pour créer des polygones non fermés. Un polygone fermé est un polygone dont le dernier vertex est connecté au premier vertex par un segment de ligne.
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords:
- Bandes de lignes
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d17a79c14e981ab0c2c0414074aff17c90a0b478
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291627"
---
# <a name="line-strips"></a>Bandes de lignes

Une bande de lignes est une primitive composée de segments de ligne connectés. Votre application peut utiliser des bandes de lignes pour créer des polygones non fermés. Un polygone fermé est un polygone dont le dernier vertex est connecté au premier vertex par un segment de ligne. Si votre application génère des polygones basés sur des bandes de lignes, les vertex ne seront pas obligatoirement coplanaires.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Exemple


L’illustration suivante représente une bande de lignes rendue.

![illustration d’une bande de ligne](images/linstrip.gif)

Le code suivant montre comment créer des vertex pour cette bande de lignes.

```cpp
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

L’exemple de code ci-dessous montre comment effectuer le rendu d’une bande de lignes dans Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Primitives](primitives.md)

 

 





---
title: Format BC7
description: Le format BC7 est un format de compression de texture utilisé pour la compression de haute qualité des données RVB et RVBA.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- Format BC7
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4ca22bc897acf0d4cbd924002de96d13f2ba8549
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159023"
---
# <a name="bc7-format"></a>Format BC7


Le format BC7 est un format de compression de texture utilisé pour la compression de haute qualité des données RVB et RVBA.

Pour plus d’informations sur les modes de blocage du format BC7, consultez [Référence du mode de formatage Bc7](/windows/desktop/direct3d11/bc7-format-mode-reference).

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgi_format_bc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>À propos du format BC7/DXGI \_ \_ Bc7


BC7 est spécifié par les valeurs d' \_ énumération de format dxgi suivantes :

-   **Dxgi \_ FORMAT \_ Bc7 non \_ typé**.
-   **Dxgi \_ FORMAT \_ Bc7 \_ UNORM**.
-   **Dxgi \_ FORMAT \_ Bc7 \_ UNORM \_ sRVB**.

Le format BC7 peut être utilisé pour les ressources de texture [Texture2D](/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (y compris les tableaux), Texture3D ou TextureCube (y compris les tableaux). De même, ce format s’applique à toutes les surfaces de la carte MIP associées à ces ressources.

BC7 utilise une taille de bloc fixe de 16 octets (128 bits) et une taille de vignette fixe de texels 4x4. Comme avec les formats BC précédents, les images de texture plus volumineuses que la taille de vignette prise en charge (4x4) sont compressées à l’aide de plusieurs blocs. Cette identité d’adressage s’applique également aux images en trois dimensions et aux tableaux MIP-Maps, cubemaps et texture. Toutes les vignettes d’image doivent avoir le même format.

BC7 compresse les images de données à virgule fixe à trois canaux (RVB) et à quatre canaux (RVBA). En règle générale, les données sources sont de 8 bits par composant de couleur (canal), bien que le format puisse encoder les données sources avec des bits plus élevés par composant de couleur. Toutes les vignettes d’image doivent avoir le même format.

Le décodeur BC7 effectue la décompression avant l’application du filtrage de texture.

Le matériel de décompression BC7 doit être d’un bit précis ; autrement dit, le matériel doit retourner des résultats qui sont identiques aux résultats retournés par le décodeur décrit dans ce document.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>Implémentation de BC7


Une implémentation de BC7 peut spécifier l’un des 8 modes, le mode spécifié dans le bit le moins significatif du bloc de 16 octets (128 bits). Le mode est encodé par zéro, un ou plusieurs bits, avec la valeur 0 suivie d’un 1.

Un bloc BC7 peut contenir plusieurs paires de points de terminaison. L’ensemble d’index qui correspond à une paire de points de terminaison peut être appelé « sous-ensemble ». En outre, dans certains modes de blocage, la représentation du point de terminaison est encodée dans un format qui peut être appelé « RBGP », où le bit « P » représente un bit de poids faible partagé pour les composants de couleur du point de terminaison. Par exemple, si la représentation du point de terminaison pour le format est « RGB 5.5.5.1 », le point de terminaison est interprété comme une valeur 6.6.6 RVB, où l’état du bit P définit le bit le moins significatif de chaque composant. De même, pour les données sources avec un canal alpha, si la représentation du format est « RGBAP 5.5.5.5.1 », le point de terminaison est interprété comme RVBA 6.6.6.6. En fonction du mode de blocage, vous pouvez spécifier le bit le moins significatif partagé pour les deux points de terminaison d’un sous-ensemble, individuellement (2 P-bits par sous-ensemble), ou partagé entre les points de terminaison d’un sous-ensemble (1 P-bit par sous-ensemble).

Pour les blocs BC7 qui n’encodent pas explicitement le composant alpha, un bloc BC7 se compose de bits en mode, de bits de partition, de points de terminaison compressés, d’index compressés et d’un P-bit facultatif. Dans ces blocs, les points de terminaison ont une représentation RVB uniquement et le composant alpha est décodé comme 1,0 pour tous les texels dans les données sources.

Pour les blocs BC7 qui ont des composants alpha et de couleur combinés, un bloc se compose de bits en mode, de points de terminaison compressés, d’index compressés et de bits de partition facultatifs et d’un bit P. Dans ces blocs, les couleurs du point de terminaison sont exprimées au format RVBA et les valeurs des composants alpha sont interpolées avec les valeurs du composant de couleur.

Pour les blocs BC7 qui possèdent des composants alpha et de couleur distincts, un bloc se compose de bits en mode, de bits de rotation, de points de terminaison compressés, d’index compressés et d’un bit sélecteur d’index facultatif. Ces blocs ont un vecteur RVB effectif \[ R, G, B \] et un canal alpha scalaire \[ est \] encodé séparément.

Le tableau suivant répertorie les composants de chaque type de bloc.

| Le bloc BC7 contient...     | bits de mode | bits de rotation | bit de sélecteur d’index | bits de partition | points de terminaison compressés | Bit P    | index compressés |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| composants de couleur uniquement     | obligatoire  | N/A           | N/A                | obligatoire       | obligatoire             | facultatif | obligatoire           |
| couleur + alpha combiné    | obligatoire  | N/A           | N/A                | facultatif       | obligatoire             | facultatif | obligatoire           |
| Color et alpha séparés | obligatoire  | obligatoire      | facultatif           | N/A            | obligatoire             | N/A      | obligatoire           |

 

BC7 définit une palette de couleurs sur une ligne approximative entre deux points de terminaison. La valeur de mode détermine le nombre de paires de points de terminaison d’interpolation par bloc. BC7 stocke un index de palette par Texel.

Pour chaque sous-ensemble d’index qui correspond à une paire de points de terminaison, l’encodeur résout l’état d’un bit des données d’index compressées pour ce sous-ensemble. Pour ce faire, il choisit un ordre de point de terminaison qui permet à l’index de l’index « Fix-up » désigné de définir son bit le plus significatif à 0 et qui peut ensuite être ignoré, ce qui permet d’économiser un bit par sous-ensemble. Pour les modes de blocage avec un seul sous-ensemble, l’index de correction est toujours l’index 0.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>Décodage du format BC7


Le pseudocode suivant décrit les étapes pour décompresser le pixel à (x, y) en fonction du bloc BC7 de 16 octets.

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

Le pseudocode au décrit les étapes permettant de décoder entièrement la couleur du point de terminaison et les composants alpha pour chaque sous-ensemble en fonction d’un bloc BC7 de 16 octets.

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

Pour générer chaque composant interpolé pour chaque sous-ensemble, utilisez l’algorithme suivant : « c » est le composant à générer ; Let « E0 » est ce composant du point de terminaison 0 du sous-ensemble ; et laissent « E1 » le composant du point de terminaison 1 du sous-ensemble.

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

Le pseudo-code suivant montre comment extraire les index et les nombres de bits pour les composants de couleur et alpha. Les blocs avec des couleurs distinctes et alpha ont également deux jeux de données d’index : un pour le canal vectoriel et un pour le canal scalaire. Pour le mode 4, ces index se distinguent par des largeurs (2 ou 3 bits) et un sélecteur à un bit spécifie si les données vectorielles ou scalaires utilisent les index 3 bits. (L’extraction du nombre de bits alpha est similaire à l’extraction du nombre de bits de couleur, mais avec un comportement inverse basé sur le bit **idxMode** .)

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>Référence du mode de format BC7


Cette section contient la liste des 8 modes de blocage et des allocations de bits pour les blocs de format de compression de texture BC7.

Les couleurs de chaque sous-ensemble au sein d’un bloc sont représentées par deux couleurs de point de terminaison explicites et un ensemble de couleurs interpolées entre elles. Selon la précision de l’index du bloc, chaque sous-ensemble peut avoir 4, 8 ou 16 couleurs possibles.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>Mode 0

Le mode BC7 0 présente les caractéristiques suivantes :

-   Composants de couleur uniquement (sans alpha)
-   3 sous-ensembles par bloc
-   Points de terminaison RGBP 4.4.4.1 avec un bit P unique par point de terminaison
-   index 3 bits
-   16 partitions

![disposition en mode 0 bits](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>Mode 1

BC7 mode 1 présente les caractéristiques suivantes :

-   Composants de couleur uniquement (sans alpha)
-   2 sous-ensembles par bloc
-   Points de terminaison RGBP 6.6.6.1 avec un bit P partagé par sous-ensemble)
-   index 3 bits
-   64 partitions

![disposition 1 bit mode](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>Mode 2

Le mode BC7 2 présente les caractéristiques suivantes :

-   Composants de couleur uniquement (sans alpha)
-   3 sous-ensembles par bloc
-   Points de terminaison 5.5.5 RVB
-   index 2 bits
-   64 partitions

![disposition en mode 2 bits](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>Mode 3

Le mode BC7 3 présente les caractéristiques suivantes :

-   Composants de couleur uniquement (sans alpha)
-   2 sous-ensembles par bloc
-   Points de terminaison RGBP 7.7.7.1 avec un bit P unique par sous-ensemble)
-   index 2 bits
-   64 partitions

![disposition en mode 3 bits](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>Mode 4

Le mode BC7 4 présente les caractéristiques suivantes :

-   Composants de couleur avec composant alpha distinct
-   1 sous-ensemble par bloc
-   Points de terminaison de couleur RVB 5.5.5
-   points de terminaison alpha 6 bits
-   16 x index 2 bits
-   16 x index 3 bits
-   rotation des composants 2 bits
-   sélecteur d’index 1 bit (si les index à 2 ou 3 bits sont utilisés)

![disposition en mode 4 bits](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>Mode 5

Le mode BC7 5 présente les caractéristiques suivantes :

-   Composants de couleur avec composant alpha distinct
-   1 sous-ensemble par bloc
-   Points de terminaison de couleur RVB 7.7.7
-   points de terminaison alpha 6 bits
-   16 x index de couleurs 2 bits
-   16 x index alpha 2 bits
-   rotation des composants 2 bits

![disposition en mode 5 bits](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>Mode 6

Le mode BC7 6 présente les caractéristiques suivantes :

-   Composants de couleur et alpha combinés
-   Un seul sous-ensemble par bloc
-   Points de terminaison de couleur RGBAP 7.7.7.7.1 (et alpha) (P-bit unique par point de terminaison)
-   16 x index 4 bits

![disposition en mode 6 bits](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>Mode 7

Le mode BC7 7 présente les caractéristiques suivantes :

-   Composants de couleur et alpha combinés
-   2 sous-ensembles par bloc
-   Points de terminaison de couleur RGBAP 5.5.5.5.1 (et alpha) (P-bit unique par point de terminaison)
-   index 2 bits
-   64 partitions

![disposition en mode 7 bits](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Concernant

Le mode 8 (l’octet le moins significatif est défini sur 0x00) est réservé. Ne l’utilisez pas dans votre encodeur. Si vous transmettez ce mode au matériel, un bloc initialisé à tous les zéros est retourné.

Dans BC7, vous pouvez encoder le composant alpha de l’une des manières suivantes :

-   Types de bloc sans encodage de composant alpha explicite. Dans ces blocs, les points de terminaison de couleur ont un encodage RVB uniquement, le composant alpha étant décodé sur 1,0 pour tous les éléments de texture.
-   Types de blocs avec des composants alpha et de couleur combinés. Dans ces blocs, les valeurs de couleur du point de terminaison sont spécifiées au format RVBA et les valeurs des composants alpha sont interpolées avec les valeurs de couleur.
-   Types de bloc avec des composants alpha et de couleur séparés. Dans ces blocs, les valeurs de couleur et alpha sont spécifiées séparément, chacune avec son propre jeu d’index. Par conséquent, ils ont un vecteur effectif et un canal scalaire qui sont encodés séparément, où le vecteur spécifie généralement les canaux \[ de couleur R, G, B \] et le scalaire spécifie le canal alpha \[ a \] . Pour prendre en charge cette approche, un champ 2 bits distinct est fourni dans l’encodage, qui permet la spécification de l’encodage de canal distinct comme valeur scalaire. Par conséquent, le bloc peut avoir l’une des quatre représentations suivantes de cet encodage alpha (comme indiqué par le champ de 2 bits) :
    -   RVB | A : canal alpha distinct
    -   AGB | R : canal de couleur « rouge » distinct
    -   RAB | G : canal de couleur « vert » distinct
    -   RGA | B : canal de couleur « bleu » distinct

    Le décodeur réorganise l’ordre des canaux à la valeur RVBA après le décodage, de sorte que le format de bloc interne est invisible pour le développeur. Les noirs avec des composants alpha et de couleur distincts ont également deux jeux de données d’index : un pour l’ensemble vectorisé de canaux et un pour le canal scalaire. (Dans le cas du mode 4, ces index sont de largeur différente de \[ 2 ou 3 bits \] . Le mode 4 contient également un sélecteur 1 bit qui spécifie si le vecteur ou le canal scalaire utilise les index 3 bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Compression de blocs de texture](texture-block-compression.md)

 

 
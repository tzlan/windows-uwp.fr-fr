---
author: normesta
description: "Entrées et propriétés d’une feuille de style de carte"
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Référence de feuille de style de carte"
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, cartes, feuille de style de carte
ms.openlocfilehash: 9e2605917027d7be96ac86421e83082aa5f368f9
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/19/2017
---
# <a name="map-style-sheet-reference"></a>Référence de feuille de style de carte

Vous pouvez créer des feuilles de style de carte à l’aide du format JavaScript Object Notation (JSON).

Par exemple, le code JSON ci-après vous permet de faire apparaître les plans d’eau en rouge, les légendes des plans d’eau en vert et les zones continentales en bleu:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000", "labelColor":"#00FF00"}}
    }
```
Vous pouvez également utiliser un code JSON pour supprimer la totalité des légendes et des objets ponctuels d’une carte.

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

Parfois, la valeur d’une propriété est transformée pour produire le résultat final.  Par exemple, végétation fillColor a des nuances légèrement différentes selon le type de l’entité affichée.  Ce comportement peut être désactivé, ce qui permet d'utiliser la valeur précise fournie, à l’aide de la propriété ignoreTransform.

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

Cet article indique les entrées et [propriétés](#properties) JSON que vous pouvez utiliser pour personnaliser l’apparence de vos cartes.

<span id="entries" />
## <a name="entries"></a>Entrées
Ce tableau utilise des caractères «>» pour représenter les différents niveaux de la hiérarchie des entrées.   

| Nom                         | Groupe de propriétés            | Description    |
|------------------------------|---------------------------|----------------|
| version                      | [Version](#version)       | Version de feuille de style que vous souhaitez utiliser. |
| settings                     | [Settings](#settings)     | Paramètres qui s’appliquent à la totalité de la feuille de style. |
| mapElement                   | [MapElement](#mapelement) | Entrée parente de toutes les entrées de la carte. |
| > baseMapElement             | [MapElement](#mapelement) | Entrée parente de toutes les entrées non utilisateur. |
| >> area                      | [MapElement](#mapelement) | Utilisation de zones continentales (à ne pas confondre avec l’entrée «structure»). |
| >>> airport                  | [MapElement](#mapelement) | Aéroports. |
| >>> cemetery                 | [MapElement](#mapelement) | Cimetières. |
| >>> continent                | [MapElement](#mapelement) | Continents entiers. |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> island                   | [MapElement](#mapelement) | Légendes des zones insulaires. |
| >>> medical                  | [MapElement](#mapelement) | Zones continentales utilisées à des fins médicales (par exemple, un campus hospitalier). |
| >>> military                 | [MapElement](#mapelement) | Bases militaires. |
| >>> nautical                 | [MapElement](#mapelement) | Zones continentales utilisées pour des activités nautiques. |
| >>> neighborhood             | [MapElement](#mapelement) | Légendes des zones définies en tant que quartiers. |
| >>> runway                   | [MapElement](#mapelement) | Zones continentales couvertes par une piste de décollage. |
| >>> sand                     | [MapElement](#mapelement) | Zones sablonneuses, telles que des plages. |
| >>> shoppingCenter           | [MapElement](#mapelement) | Surfaces au sol allouées à des galeries marchandes ou à d’autres centres commerciaux. |
| >>> stadium                  | [MapElement](#mapelement) | Stades. |
| >>> vegetation               | [MapElement](#mapelement) | Forêts, zones herbeuses, etc. |
| >>>> forest                  | [MapElement](#mapelement) | Terres forestières. |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | Espaces verts. |
| >>>> playingField            | [MapElement](#mapelement) | Terrains de sport, tels qu’un terrain de base-ball ou un court de tennis. |
| >>>> reserve                 | [MapElement](#mapelement) | Réserves naturelles. |
| >> point                     | [PointStyle](#pointstyle) | Ensemble des objets géographiques ponctuels qui sont affichés avec une icône quelconque. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  |
| >>>> peak                    | [PointStyle](#pointstyle) | Icônes représentant des pics montagneux. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) | Icônes représentant des sommets de volcan. |
| >>>> waterPoint              | [PointStyle](#pointstyle) | Icônes représentant des points d’eau, tels qu’une cascade. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) | Restaurants, hôpitaux, écoles, ports de plaisance, domaines skiables, etc. |
| >>>> business                | [PointStyle](#pointstyle) | Restaurants, hôpitaux, écoles, etc. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) | Restaurants, cafés, etc. |
| >>> populatedPlace           | [PointStyle](#pointstyle) | Icônes représentant la superficie d’une agglomération (par exemple, une ville ou un village). |
| >>>> capital                 | [PointStyle](#pointstyle) | Icônes représentant le chef-lieu d’une agglomération. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) | Icônes représentant le chef-lieu d’un département ou d’une province. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) | Icônes représentant la capitale d’un pays ou d’une région. |
| >>> roadShield               | [PointStyle](#pointstyle) | Caractères représentant le nom abrégé d’une route. (Par exemple: A6). Si vous définissez la propriété **ImageFamily** de l’entrée de paramètres sur la valeur *Palette*, utilisez uniquement des valeurs de palette. |
| >>> roadExit                 | [PointStyle](#pointstyle) | Icônes représentant des sorties, appartenant généralement à un réseau autoroutier. |
| >>> transit                  | [PointStyle](#pointstyle) | Icônes représentant des arrêts d’autobus, des gares, des aéroports, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) | Zones administratives telles que les pays, les régions et les départements. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) | Préfectures, départements, provinces, etc. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) | Circonscriptions, sous-préfectures, etc. |
| >> structure                 | [MapElement](#mapelement) | Bâtiments et autres constructions. |
| >>> building                 | [MapElement](#mapelement) |  |
| >>>> educationBuilding       | [MapElement](#mapelement) |  |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  |
| >>>> transitBuilding         | [MapElement](#mapelement) |  |
| >> transportation            | [TwoToneLineStyle](#twotonelinestyle) | Voies appartenant au réseau de transport (par exemple, routes, lignes de chemin de fer et lignes de ferry). |
| >>> road                     | [TwoToneLineStyle](#twotonelinestyle) | Voies représentant toutes les routes. |
| >>>> controlledAccessHighway | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> highSpeedRamp          | [TwoToneLineStyle](#twotonelinestyle) | Voies représentant les bretelles. Ces bretelles sont généralement situées aux abords du réseau autoroutier. |
| >>>> highway                 | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> majorRoad               | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> arterialRoad            | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> street                  | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> ramp                   | [TwoToneLineStyle](#twotonelinestyle) | Voies représentant l’entrée et la sortie d’une autoroute. |
| >>>>> unpavedStreet          | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> tollRoad                | [TwoToneLineStyle](#twotonelinestyle) | Routes à péage. |
| >>> railway                  | [TwoToneLineStyle](#twotonelinestyle) | Voies ferrées. |
| >>> trail                    | [TwoToneLineStyle](#twotonelinestyle) | Sentiers de promenade au sein des parcs ou sentiers de randonnée pédestre. |
| >>> waterRoute               | [TwoToneLineStyle](#twotonelinestyle) | Lignes de ferry. |
| >> water                     | [MapElement](#mapelement) | Tout type de plan d’eau. Comprend notamment les océans et les ruisseaux. |
| >>> river                    | [MapElement](#mapelement) | Rivières, ruisseaux ou autres passages d’eau.  Notez que cet élément peut prendre la forme d’une ligne ou d’un polygone et qu’il peut être relié à des plans d’eau autres que des rivières. |
| > routeMapElement            | [MapElement](#mapelement) | Entrée regroupant toutes les voies de circulation. |
| >> routeLine                 | [TwoToneLineStyle](#twotonelinestyle) | Style de l’ensemble des voies de circulation. |
| >>> walkingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>> drivingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| > userMapElement             | [MapElement](#mapelement) | Entrée regroupant toutes les entrées utilisateur. |
| >> userPoint                 | [PointStyle](#pointstyle) | Style des objets ponctuels utilisateur par défaut. |
| >> userLine                  | [MapElement](#mapelement) | Style des voies utilisateur par défaut. |

<span id="properties" />
## <a name="properties"></a>Propriétés

Cette section décrit les propriétés que vous pouvez utiliser pour chaque entrée.

<span id="version" />
### <a name="version-properties"></a>Propriétés de version

| Propriété                     | Type    | Description                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | Chaîne  | Version de feuille de style ciblée. Utilisée à des fins d’applicabilité. «1.0» par défaut, «1.*» pour les mises à jour de fonctionnalités mineures supplémentaires. |

<span id="settings" />
### <a name="settings-properties"></a>Propriétés de paramètres

| Propriété                     | Type    | Description                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | Booléen    | Indicateur précisant si l’atmosphère apparaît dans le contrôle3D.                                                                                                                                                     |
| fogColor                     | Couleur   | Valeur de couleur ARVB du brouillard de distance qui apparaît dans le contrôle3D.                                                                                                                                                    |
| glowColor                    | Couleur   | Valeur de couleur ARVB pouvant être appliquée à l’éclat des légendes et des icônes.                                                                                                                                                     |
| imageFamily                  | Chaîne  | Nom de l’ensemble d’images à utiliser pour ce style. Définissez cette propriété sur la valeur *Default* pour les signes qui utilisent des couleurs fixes basées sur le signe réel. Définissez cette propriété sur la valeur *Palette* pour les signes qui utilisent des couleurs de palette configurables. |
| landColor                    | Couleur   | Valeur de couleur ARVB d’une zone continentale avant qu’un élément quelconque soit dessiné sur cette dernière.                                                                                                                                                     |
| logosVisible                 | Booléen    | Indicateur précisant si les éléments dotés d’une propriété **Organization** doivent dessiner les logos appropriés ou utiliser une icône générique.                                                                                         |
| officialColorVisible         | Booléen    | Indicateur précisant si les éléments dotés d’une propriété de couleur officielle (telle que celle des lignes de transport en commun en Chine) doivent dessiner cette couleur. Par exemple, désactivez cette valeur dans le cas d’une carte en noir et blanc.                               |
| rasterRegionsVisible         | Booléen    | Indicateur précisant s’il convient ou non de dessiner les régions raster que nous ne restituons pas par des vecteurs (par exemple, Japon et Corée).                                                                                                |
| shadedReliefVisible          | Booléen    | Indicateur précisant s’il convient ou non de dessiner l’ombrage des hauteurs sur la carte.                                                                                                                                                  |
| shadedReliefDarkColor        | Couleur   | Couleur du côté foncé d’un relief par ombres portées.  Le canal alpha représente la valeur alpha maximale.                                                                                                                              |
| shadedReliefLightColor       | Couleur   | Couleur du côté clair d’un relief par ombres portées.  Le canal alpha représente la valeur alpha maximale.                                                                                                                             |
| spaceColor                   | Couleur   | Valeur de couleur ARVB de la zone entourant la carte.                                                                                                                                                                               |
| useDefaultImageColors        | Booléen    | Indicateur précisant s’il convient d’utiliser les couleurs d’origine du format SVG au lieu de rechercher les couleurs d’une image dans l’entrée de palette.                                                                                |

<span id="mapelement" />
### <a name="mapelement-properties"></a>Propriétés MapElement

| Propriété                     | Type    | Description                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| fillColor                    | Couleur   | Couleur utilisée pour le remplissage des polygones, pour l’arrière-plan des icônes d’objet ponctuel et pour le centre des lignes si elles sont fractionnées. |
| fontFamily                   | Chaîne  |                                                                                                                             |
| labelColor                   | Couleur   |                                                                                                                             |
| labelOutlineColor            | Couleur   |                                                                                                                             |
| labelScale                   | Flottant   | Valeur de mise à l’échelle des tailles de légende par défaut. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande.            |
| labelVisible                 | Booléen    |                                                                                                                             |
| strokeColor                  | Couleur   | Couleur à utiliser pour le contour des polygones, pour le contour des icônes d’objet ponctuel et pour la couleur des lignes.                   |
| strokeWidthScale             | Flottant   | Valeur de mise à l’échelle du trait des lignes. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande.            |
| visible                      | Booléen    |                                                                                                                             |

<span id="borderedmap" />
### <a name="borderedmapelement"></a>BorderedMapElement

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | Description                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | Couleur   | Couleur de ligne secondaire ou de contour de la bordure d’un polygone rempli. |
| borderStrokeColor            | Couleur   | Couleur de ligne principale de la bordure d’un polygone rempli.             |
| borderVisible                | Booléen    |                                                                       |
| borderWidthScale             | Flottant   |                                                                       |

<span id="pointstyle" />
### <a name="pointstyle-properties"></a>Propriétés PointStyle

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | Description                                                                                                        |
|------------------------------|---------|--------------------------------------------------------------------------------------------------------------------|
| iconColor                    | Couleur   | Couleur du glyphe affiché au milieu d’une icône d’objet ponctuel.                                                        |
| scale                        | Flottant   | Valeur de mise à l’échelle de la taille de l’objet ponctuel entier. Par exemple, utilisez la valeur *1* pour obtenir la taille par défaut et la valeur *2* pour obtenir une taille deux fois plus grande. |
| stemColor                    | Couleur   | Couleur de la ligne verticale sortant du bas de l’icône en mode3D.                                             |
| stemOutlineColor             | Couleur   | Couleur du contour de la ligne verticale sortant du bas de l’icône en mode3D.                          |

<span id="twotonelinestyle" />
### <a name="twotonelinestyle-properties"></a>Propriétés TwoToneLineStyle

Ce groupe de propriétés hérite du groupe de propriétés [MapElement](#mapelement).

| Propriété                     | Type    | Description                                                                                          |
|------------------------------|---------|------------------------------------------------------------------------------------------------------|
| overwriteColor               | Booléen    |  Fait en sorte que la valeur alpha de l’élément **FillColor** remplace l’élément **StrokeColor** au lieu de fusionner avec ce dernier. |
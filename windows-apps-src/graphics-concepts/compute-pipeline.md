---
title: Pipeline de calcul
description: "Le pipeline de calcul Direct 3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique."
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e6c2fdf148e582360a125c3cd98013dc6b424535
ms.lasthandoff: 02/07/2017

---

# <a name="compute-pipeline"></a>Pipeline de calcul


\[Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.\]


Le pipeline de calcul Direct3D est conçu pour traiter les calculs pouvant être effectués principalement en parallèle du pipeline graphique. Le pipeline de calcul comporte seulement quelques étapes, où des données sont transmises de l’entrée vers la sortie via l’étape du nuanceur de calcul programmable.

| | |
|-|-|
|Objectif|Comme les autres nuanceurs programmables, l'[étape Compute Shader (CS)](compute-shader-stage--cs-.md) est conçue et implémentée avec HLSL. Un nuanceur de calcul assure des opérations de calcul général à haut débit et tire parti du grand nombre de processeurs parallèles sur le processeur graphique (GPU). Le nuanceur de calcul fournit des fonctionnalités de partage de mémoire et de synchronisation de threads pour prendre en charge des méthodes de programmation parallèles plus efficaces.|
|Entrée|Contrairement à d’autres nuanceurs programmables, la définition d’entrée est abstraite. L’entrée peut, par nature, avoir une, deux ou trois dimensions, qui déterminent le nombre d’appels du nuanceur de calcul à exécuter. Il est possible de définir des données partagées pour un ensemble d’appels à lire.|
|Sortie|Les données de sortie du nuanceur de calcul, qui peuvent être extrêmement variées, peuvent être synchronisées avec le pipeline de rendu graphique lorsque les données calculées sont requises.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, [Compute Shader (CS) stage](#compute-shader-stage--cs-.md) is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphismes Direct3D](index.md)

 

 

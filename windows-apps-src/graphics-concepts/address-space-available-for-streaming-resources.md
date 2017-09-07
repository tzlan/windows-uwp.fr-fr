---
title: "Espace d’adresse disponible pour les ressources de diffusion en continu"
description: "Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu."
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords: "Espace d’adresse disponible pour les ressources de diffusion en continu"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: a63136f04570c4bf964c461f7296c930f5e168b5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="address-space-available-for-streaming-resources"></a>Espace d’adresse disponible pour les ressources de diffusion en continu


Cette section spécifie l’espace d’adresse virtuel disponible pour les ressources de diffusion en continu.

Les systèmes d’exploitation 64bits offrent au moins 40bits d’espace d’adresse virtuel (1téraoctet) disponible.

Pour les systèmes d’exploitation 32bits, l’espace d’adresse est de 32bits (4Go). Pour les systèmesARM 32bits, la création de ressources de diffusion en continu individuelles peut échouer si l’allocation utilise plus de 27bits d’espace d’adresse (128Mo). Ceci inclut tout remplissage masqué dans l’espace d’adresse que le matériel peut utiliser pour les mipmaps, le remplissage de vignettes compressées ainsi que, éventuellement, les dimensions de surfaces de remplissage à des puissances de 2.

Sur les systèmes graphiques comportant une table de page distinctes pour le processeur graphique (GPU), la majeure partie de cet espace d’adresse sera disponible aux ressources GPU utilisées par l’application, même si les allocations de GPU effectuées par le pilote d’affichage tiennent dans le même espace.

Sur les futurs systèmes où la table de pages sera partagée entre l'UC et le processeur graphique, l’espace d’adresse disponible sera partagée entre toutes les allocations UC et GPU d'un processus.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Paramètres de création de ressources de diffusion en continu](streaming-resource-creation-parameters.md)

 

 




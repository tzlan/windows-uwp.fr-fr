---
author: TerryWarwick
title: Configuration système requise du scanneur de codes-barres à caméra
description: Cet article répertorie les exigences d'utilisation d'un scanneur de codes-barres à caméra à partir d’une application UWP.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: f0dcbe28107bb8c6918e4e0ac63698f1f9692005
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833135"
---
# <a name="camera-barcode-scanner-system-requirements"></a>Configuration système requise du scanneur de codes-barres à caméra
À partir de Windows10, version1803, vous pouvez lire des codes-barres via un objectif de caméra standard à partir d’une application Windows universelle.

## <a name="supported-windows-editions"></a>Versions de Windows prises en charge
- Windows Mobile10 Professionnel en modeS
- Windows10Professionnel
- Windows10 Entreprise
- Windows10 IoT Core


## <a name="webcam-requirements"></a>Configuration système de la webcam
| Catégorie      | Recommandation           | Commentaires |
| ------------- | ------------------------ | -------- |
| Mise au point         | Mise au point automatique               | La mise au point fixe n’est pas recommandée |
| Résolution    | 1920 x 1440, ou supérieure    | Nous avons eu une expérience optimale avec des caméras capables d'une résolution de 1920 x 1440 ou plus.  Certaines caméras à résolution plus faible peuvent lire les codes-barres standards s'ils sont imprimés suffisamment grands. Les codes-barres comportant des éléments plus fins peuvent nécessiter des caméras d'une résolution supérieure. |
|

---
author: jnHs
Description: The Insights report in the Windows Dev Center dashboard
title: Rapport de perspectives
ms.author: wdg-dev-content
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, insight, tendance, anomalies, des anomalies, les modifications de données
ms.localizationpriority: medium
ms.openlocfilehash: be70dccbb7a12b65b9e7bbd07f27ae7ea3a578ff
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3370370"
---
# <a name="insights-report"></a>Rapport de perspectives


Le rapport de **perspectives** dans le tableau de bord du centre de développement Windows met en évidence des modifications importantes (augmentations ou baisses significatives dans une mesure spécifique) que nous avons détecté au cours des 30 derniers jours dans vos acquisitions, intégrité, et/ou des données d’utilisation. Cela vous permet d’obtenir un aperçu des modifications importantes potentiellement sans avoir à afficher tous les graphiques dans chacun de ces rapports.

> [!NOTE]
> Données de ce rapport couvrent au cours des 30 derniers jours. Vous ne pouvez pas sélectionner une autre période de temps pour ce rapport.

Le rapport trie les données en trois onglets: **Acquisitions**, **intégrité**et **utilisation**. Pour afficher des informations pour l’une de ces zones, sélectionnez son onglet.

Informations sont affichent lorsque nous détectons une évolution importante dans vos données. Pour chaque insight, nous allons montrer les éléments suivants:
- **Type de Insight**: la zone dans laquelle l’insight a été détectée.
- **Valeur**: métrique spécifique qui a changé considérablement (ou **tous les** si la modification s’applique à l’ensemble du **type d’informations**).
- **Date**: la date à laquelle nous avons identifié la modification. Cette date représente la fin de la semaine dans lequel nous avons détecté une augmentation significative ou une baisse par rapport à la semaine.
- **Incidence globale**: le pourcentage de la valeur augmentée ou a diminué sur votre base de clients. Elle vous permet de comprendre comment généralisée l’impact d’une modification particulière peut être, en particulier lors de la comparaison avec les informations de pourcentage affichées dans **Top contributors.**
- **Contributeurs principaux**: le cas échéant, le segment spécifique, package ou autre facteur identification pour aider à comprendre quels clients la modification est liée à. Par exemple, une modification peut être détectée principalement avec les clients à partir d’un marché spécifique ou sur un certain type d’appareil. Pour les données **d’intégrité** , cela peut inclure les hachages de défaillance spécifiques ou des versions de package. Le cas échéant, nous allons montrer également le pourcentage que la valeur augmentée ou réduite pour que le facteur.
- **Action**:
   - Sélectionnez **Afficher 14 jours tendance** pour afficher un graphique affichant la modification de la métrique 14 derniers jours entière conduisant à la date insight.
   - Sélectionnez **dites-nous si c’est le cas** pour nous faire part de vos commentaires et dites-nous si les informations que nous avons fourni semblent précises. Vos commentaires nous aideront à continuer à améliorer les données que nous fournissons ici. 

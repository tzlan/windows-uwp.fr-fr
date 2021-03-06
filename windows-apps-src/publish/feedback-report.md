---
description: Le rapport de commentaires de l’espace partenaires vous permet de voir les problèmes, les suggestions et les votes que vos clients Windows 10 ont soumis via le hub de commentaires.
title: Rapport sur les commentaires
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ad8cb47a2fe8df1ebbf1d1b67659a85b682d2b4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034792"
---
# <a name="feedback-report"></a>Rapport sur les commentaires

> [!WARNING]
> Désapprobation du rapport de commentaires le 15 avril 2020 ce rapport ne sera plus pris en charge après le 15 avril 2020. Les données de ce rapport ne seront pas actualisées après cette date et le rapport sera supprimé ultérieurement sans préavis. Vous pouvez continuer à afficher les commentaires reçus de vos clients directement dans le hub de commentaires.

Le **rapport de commentaires** de l’espace partenaires vous permet de voir les problèmes, les suggestions et les votes que vos clients Windows 10 ont soumis via le hub de commentaires. Vous pouvez afficher ces données dans l’espace partenaires ou exporter les données pour les afficher hors connexion.

> [!NOTE]
> Vous pouvez également [répondre aux commentaires](respond-to-customer-feedback.md) directement à partir de ce rapport pour informer vos clients que vous êtes à l’écoute.

Inciter vos clients à faire des commentaires sur votre application est un excellent moyen d’en savoir plus sur les problèmes et les fonctionnalités qui sont plus importantes pour eux. Quand vos clients savent qu’ils peuvent vous envoyer des commentaires directement, ils peuvent être moins susceptibles de faire part de ces commentaires en guise d’évaluation négative dans le magasin.

Vous pouvez utiliser l’API de commentaires dans le [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) pour permettre aux clients de [lancer directement le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md). N’oubliez pas que tout client ayant téléchargé votre application sur un appareil Windows 10 prenant en charge le Hub de commentaires a la possibilité de laisser des commentaires à son sujet à l’aide de cette application. Pour cette raison, vous pouvez voir les commentaires des clients dans ce rapport, même si vous n’avez pas demandé spécifiquement de commentaires à partir de votre application.

Les commentaires peuvent également être utiles lors de l’utilisation de la fonction de [vol de packages](package-flights.md), puisque le rapport de **Commentaires** vous indique le package spécifique que chaque client avait installé sur son appareil lorsqu’il a laissé les commentaires.

> [!TIP]
> Pour consulter rapidement les avis, les évaluations et les commentaires des utilisateurs au cours des 30 derniers jours, développez **engagement** dans le menu de navigation de gauche, puis sélectionnez **critiques et commentaires.** 


## <a name="apply-filters"></a>Appliquer des filtres

Près du haut de la page, vous pouvez sélectionner la période pour laquelle vous souhaitez afficher les données. La sélection par défaut est **durée de vie** , mais vous pouvez choisir d’afficher les données pendant 30 jours, 3 mois, 6 mois ou 12 mois.

Vous pouvez également développer des **filtres** pour filtrer toutes les données de cette page en suivant les options ci-dessous.

- **Type d’appareil** : le paramètre par défaut est **Tous** . Vous pouvez sélectionner **Problème** ou **Suggestions** pour n’afficher que ce type de commentaire.
- **Type d'appareil** : le paramètre par défaut est **Tous les appareils** . Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les commentaires laissés par les clients à l’aide de ce type d’appareil.
- **Version du package** : le paramètre par défaut est **Tous les packages** . Vous pouvez sélectionner l’un de vos packages pour afficher uniquement les commentaires laissés par les clients ayant utilisé ce package spécifique lorsqu’ils ont laissé leur commentaire.
- **Marché** : la valeur par défaut de ce filtre est **Tous les marchés** . Vous pouvez choisir un marché spécifique pour n’afficher que les commentaires des clients de ce marché.
- **Groupe** : le paramètre par défaut est **Tous** . Vous pouvez choisir d’afficher uniquement les commentaires soumis par les [Windows Insiders](https://insider.windows.com).

> [!TIP]
> Si vous ne voyez aucun commentaire sur la page, vérifiez que vos filtres n’ont pas exclu tous vos commentaires. Par exemple, si vous filtrez les commentaires en fonction d’un **type d’appareil** non pris en charge par votre application, aucun commentaire n’apparaîtra sur cette page.


## <a name="viewing-feedback-details"></a>Affichage des détails de vos commentaires

Dans ce rapport, vous verrez les Commentaires individuels laissés par vos clients. À gauche du texte de commentaires pour chaque élément, vous verrez le nombre de fois que les commentaires ont été émis par d’autres clients dans le hub de commentaires. Vous pouvez trier le commentaire de trois façons :

- **Votes pour** (par défaut) : affiche les commentaires pour lesquels les autres clients ont voté, en commençant par le commentaire ayant reçu le plus de votes.
- **Fréquents** : affiche les commentaires pour lesquels les autres clients ont voté au cours des sept derniers jours en commençant par le commentaire ayant fait l’objet de l’activité la plus récente.
- **Les plus récents** : montre tous les commentaires en commençant par le commentaire laissé le plus récemment.

La date à laquelle le commentaire a été laissé et le type de commentaire s’affiche en regard de chaque commentaire. Vous verrez également le marché du client, le package spécifique qui a été installé sur l’appareil qu’il utilisait lorsqu’il a quitté les commentaires, le type de cet appareil et **Windows Insider** si le client qui envoie les commentaires est membre du programme Windows Insider.

Vous verrez également une option ici pour [répondre aux commentaires](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Traduction des commentaires

Par défaut, les commentaires qui n’ont pas été écrits dans votre langue par défaut sont traduits pour vous. Si vous préférez, la traduction de commentaires peut être désactivée en désactivez la case à cocher **traduire les commentaires** en haut à droite, près des filtres de page.

Notez que les commentaires sont traduits par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Lancement du Hub de commentaires directement depuis votre application

Comme indiqué plus haut, nous vous recommandons d’intégrer un lien direct vers le Hub de commentaires directement dans votre application afin d’inciter les clients à envoyer leurs commentaires. Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md)

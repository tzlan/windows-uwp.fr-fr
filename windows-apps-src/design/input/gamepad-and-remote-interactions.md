---
author: mijacobs
Description: TODO
title: Interactions entre le boîtier de commande et la télécommande
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da1248937d8f7d1a5a1da27e376690cde2ac7ef6
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842849"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interactions entre le boîtier de commande et la télécommande

![Télécommande et bouton multidirectionnel](images/dpad-remote/dpad-remote.png)

Les applications de plateforme Windows universelle (UWP) prennent désormais en charge les entrées à l'aide du boîtier de commande et de la télécommande, qui sont les périphériques d’entrée principaux de la Xbox et de la télévision.

Les applications UWP doivent être optimisées pour ces types d’appareils d’entrée, comme c’est le cas pour les entrées via le clavier et la souris d’un ordinateur, et pour les entrées tactiles sur un téléphone ou une tablette.

Il est essentiel de s’assurer que votre application fonctionne correctement avec ces appareils d’entrée lorsque vous les optimisez pour une Xbox et un téléviseur.

> [!NOTE] 
> Vous pouvez maintenant brancher le boîtier de commande et l’utiliser avec des applications UWP sur un ordinateur, ce qui facilite la validation du travail.

Pour garantir une expérience utilisateur réussie et agréable avec l’application UWP lors de l’utilisation d’un boîtier de commande ou d’une télécommande, envisagez ce qui suit:

* [Boutons matériels](../devices/designing-for-tv.md#hardware-buttons)-Le boîtier de commande et la télécommande ont des configurations et des boutons très différents.

* [Navigation et interaction en mode focus XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction)-La navigation en mode focus XY permet à l’utilisateur de naviguer dans l’interface utilisateur de votre application.

* [Mode souris](../devices/designing-for-tv.md#mouse-mode)-Le mode souris permet à votre application d’émuler l’utilisation de la souris lorsque la navigation en mode focus XY n’est pas suffisante.
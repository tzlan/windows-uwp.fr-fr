---
title: Stick arcade
description: Utilisez les API de stick arcade Windows.Gaming.Input pour détecter les sticks arcade et lire leurs entrées.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, Arcade Stick, entrée
ms.localizationpriority: medium
ms.openlocfilehash: e9a9a2b3622169b99a1b3a0c3607329c85c5785f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159413"
---
# <a name="arcade-stick"></a>Stick arcade

Cet article explique les notions de base de la programmation pour les sticks arcade Xbox One avec l’API [Windows.Gaming.Input.ArcadeStick][arcadestick] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Obtenir une liste des sticks arcade connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un stick arcade
* Lire les entrées provenant d’un ou de plusieurs sticks arcade
* comportement des cartes d’arcade comme des appareils de navigation de l’interface utilisateur

## <a name="arcade-stick-overview"></a>Vue d’ensemble des sticks arcade

Les sticks arcade sont des périphériques d’entrée appréciés pour leur capacité à reproduire la sensation des machines d’arcade autonomes et pour leurs contrôles numériques de haute précision. Les sticks arcade constituent le périphérique d’entrée parfait pour les combats tête à tête ou d’autres jeux de type arcade. Ils conviennent à tous les jeux qui fonctionnent bien avec des contrôles entièrement numériques. Les sticks arcade sont pris en charge dans les applications UWP Windows 10 et Xbox One par l’espace de noms [Windows.Gaming.Input][].

Xbox One arcade sont équipés d’une manette de jeu numérique à 8 voies, de six boutons d' **action** (représentés par a1-a6 dans l’image ci-dessous) et de deux boutons **spéciaux** (représentés par S1 et S2). Il s’agit de périphériques d’entrée numériques qui ne prennent pas en charge les contrôles ou les vibrations analogiques. Xbox One arcade est également équipé de boutons de **visualisation** et de **menu** utilisés pour prendre en charge la navigation dans l’interface utilisateur, mais ils ne sont pas destinés à prendre en charge les commandes de jeu et ne peuvent pas être facilement accessibles en tant que boutons de manette de jeu.

![Un manche d’arcade avec manette à 4 directions, 6 boutons d’action (a1-a6) et 2 boutons spéciaux (S1 et S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>Navigation d’interface utilisateur

Pour faciliter la prise en charge de nombreux périphériques d’entrée différents pour la navigation dans l’interface utilisateur, et améliorer la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ jouent simultanément le rôle de périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

En tant que contrôleur de navigation de l’interface utilisateur, les bâtons d’arcade mappent l' [ensemble](ui-navigation-controller.md#required-set) des commandes de navigation requises sur les boutons de la manette et de l' **affichage**, du **menu**, de l' **action 1**et de l' **action 2** .

| Commande de navigation | Entrée stick arcade  |
| ------------------:| ------------------- |
|                 Haut | Stick vers le haut            |
|               Descendre | Stick vers le bas          |
|               Gauche | Stick vers la gauche          |
|              Right | Stick vers la droite         |
|               Affichage | Bouton Afficher         |
|               Menu | Bouton Menu         |
|             Acceptation | Bouton Action 1     |
|             Annuler | Bouton Action 2     |

Les sticks arcade ne mappent aucun [ensemble facultatif](ui-navigation-controller.md#optional-set) de commandes de navigation.

## <a name="detect-and-track-arcade-sticks"></a>Détecter et suivre des sticks arcade

La détection et le suivi des cartes d’arcade fonctionnent exactement de la même façon que pour les boîtiers de souformement, à l’exception de la classe [ArcadeStick][] au lieu de la classe de [boîtier](/uwp/api/Windows.Gaming.Input.Gamepad) . Pour plus d’informations, consultez [boîtier et vibration](gamepad-and-vibration.md) .

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>Lecture des entrées du stick arcade

Une fois que vous avez identifié le stick arcade qui vous intéresse, vous pouvez commencer à collecter les entrées de ce stick. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les sticks arcade ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces boîtiers de commande pour connaître leur état actuel.

### <a name="polling-the-arcade-stick"></a>Interroger le stick arcade

Le processus d’interrogation capture un instantané du stick arcade à un moment précis. Cette approche de la collecte des entrées est adaptée à la plupart des jeux, car leur logique s’exécute généralement dans une boucle déterministe plutôt que d’être pilotée par des événements. en général, il est généralement plus simple d’interpréter les commandes de jeu provenant d’entrées rassemblées en une seule fois par rapport à de nombreuses entrées collectées au fil du temps.

Vous interrogez un stick arcade en appelant la fonction [GetCurrentReading][]. Cette fonction renvoie un [ArcadeStickReading][] qui contient l’état du stick arcade.

L’exemple de code suivant interroge un stick arcade pour obtenir son état actuel.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

En plus de l’état du stick arcade, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-buttons"></a>Lecture des boutons

Chacun des boutons de la manette de l’arcade &mdash; se compose des quatre directions de la manette, de six boutons d' **action** et de deux boutons **spéciaux** &mdash; . il fournit une lecture numérique qui indique si elle est appuyée (vers le haut) ou commercialisée (haut). Pour plus d’efficacité, les lectures de bouton ne sont pas représentées sous forme de valeurs booléennes individuelles. au lieu de cela, ils sont tous regroupés dans un seul champ de champ qui est représenté par l’énumération [ArcadeStickButtons][] .

> [!NOTE]
> Les bâtons d’arcade sont équipés de boutons supplémentaires utilisés pour la navigation dans l’interface utilisateur, tels que les boutons d' **affichage** et de **menu** . Ces boutons ne figurent pas dans l’énumération `ArcadeStickButtons`. Leurs entrées sont lues uniquement quand le stick arcade est utilisé comme périphérique de navigation d’interface utilisateur. Pour plus d’informations, consultez [Périphérique de navigation d’interface utilisateur](ui-navigation-controller.md).

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [ArcadeStickReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est enfoncé (enfoncé) lorsque le bit correspondant est défini ; dans le cas contraire, elle est libérée (haut).

L’exemple suivant détermine si le bouton d' **action 1** est enfoncé.

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

L’exemple suivant détermine si le bouton d' **action 1** est relâché.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

Parfois, vous souhaiterez peut-être déterminer quand un bouton passe de enfoncé à relâché ou relâché, si plusieurs boutons sont enfoncés ou relâché, ou si un ensemble de boutons est organisé d’une façon particulière &mdash; , d’autres non. Pour plus d’informations sur la détection de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Exécuter l’exemple InputInterfacing

L’[exemple InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) montre comment utiliser les sticks arcade en tandem avec différents types de périphériques d’entrée. Il illustre aussi le comportement de ces périphériques d’entrée utilisés comme contrôleurs de navigation d’interface utilisateur.

## <a name="see-also"></a>Voir aussi

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)

[Windows. Gaming. Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[arcadestick]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadesticks]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickadded]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickremoved]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickreading]: /uwp/api/Windows.Gaming.Input.ArcadeStickReading
[arcadestickbuttons]: /uwp/api/Windows.Gaming.Input.ArcadeStickButtons
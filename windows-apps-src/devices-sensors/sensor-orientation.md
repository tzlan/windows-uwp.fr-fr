---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Orientation de capteur
description: Les données du capteur provenant des classes Accelerometer, Gyrometer, Compass, Inclinometer et OrientationSensor sont définies par leurs axes de référence. Ces axes sont définis par l’orientation paysage de l’appareil et pivotent avec celui-ci à mesure que l’utilisateur le fait tourner.
ms.date: 07/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8836753778b1dd5dcbc8856b0df5ec1f11d8e753
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159803"
---
# <a name="sensor-orientation"></a>Orientation de capteur

Les données de capteur des classes [**accéléromètre**](/uwp/api/Windows.Devices.Sensors.Accelerometer), [**gyromètre**](/uwp/api/Windows.Devices.Sensors.Gyrometer), [**Compass**](/uwp/api/Windows.Devices.Sensors.Compass), [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer)et [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) sont définies par leurs axes de référence. Ces axes sont définis par le frame de référence de l’appareil et pivotent avec l’appareil lorsque l’utilisateur l’active. Si votre application prend en charge la rotation automatique et se réoriente pour s’adapter à l’appareil à mesure que l’utilisateur le fait pivoter, vous devez ajuster vos données du capteur par rapport à la rotation avant de l’utiliser.

### <a name="important-apis"></a>API importantes

- [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
- [**Windows. Devices. Sensors. Custom**](/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>Orientation de l’affichage et orientation de l’appareil

Pour comprendre les axes de référence pour les capteurs, vous devez distinguer l’orientation de l’affichage de l’orientation de l’appareil. L’orientation de l’affichage correspond au sens dans lequel le texte et les images sont affichés à l’écran, alors que l’orientation de l’appareil correspond au positionnement physique de l’appareil.

> [!NOTE]
> L’axe z positif s’étend à partir de l’écran de l’appareil, comme indiqué dans l’image suivante.
> :::image type="content" source="images/sensor-orientation-zaxis-1-small.png" alt-text="Axe Z pour ordinateur portable":::

Dans les diagrammes suivants, l’orientation de l’appareil et de l’affichage est en [paysage](/uwp/api/Windows.Graphics.Display.DisplayOrientations) (les axes de capteur affichés sont spécifiques à l’orientation paysage).


Ce diagramme illustre l’affichage et l’orientation de l’appareil en [mode paysage](/uwp/api/Windows.Graphics.Display.DisplayOrientations).

:::image type="content" source="images/sensor-orientation-a-small.jpg" alt-text="Orientation de l’affichage et de l’appareil en mode Landscape":::

Le diagramme suivant montre l’orientation de l’affichage et de l’appareil dans [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations).

:::image type="content" source="images/sensor-orientation-b-small.jpg" alt-text="Orientation de l’affichage et de l’appareil en mode LandscapeFlipped":::

Ce diagramme final montre l’orientation d’affichage en paysage alors que l’orientation de l’appareil est [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations).

:::image type="content" source="images/sensor-orientation-c-small.jpg" alt-text="Orientation d’affichage en mode Landscape tandis que l’orientation de l’appareil est en mode LandscapeFlipped.":::

Vous pouvez effectuer une requête sur les valeurs d’orientation dans la classe [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation) à l’aide de la méthode [**GetForCurrentView**](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) avec la propriété [**CurrentOrientation**](/uwp/api/windows.graphics.display.displayinformation.currentorientation). Vous pouvez ensuite créer une logique en la comparant à l’énumération [**DisplayOrientations**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) . Souvenez-vous que, pour chaque orientation que vous prenez en charge, vous devez prendre en charge une conversion des axes de référence dans cette orientation.

## <a name="landscape-first-vs-portrait-first-devices"></a>Appareils à priorité Paysage ou à priorité Portrait

Les fabricants produisent des appareils à priorité Paysage ou Portrait. Le cadre de référence varie selon que les appareils sont à priorité Paysage (tels que les ordinateurs de bureau et ordinateurs portables) ou à priorité Portrait (comme les téléphones et certaines tablettes). Le tableau suivant montre les axes de capteur pour les appareils à priorité Paysage et Portrait.

| Orientation | Priorité Paysage | Priorité Portrait |
|-------------|-----------------|----------------|
| **Paysage** | :::image type="content" source="images/sensor-orientation-0-small.jpg" alt-text="Appareil à priorité Paysage en mode d’orientation Landscape"::: | :::image type="content" source="images/sensor-orientation-1-small.jpg" alt-text="Appareil à priorité Portrait en mode d’orientation Landscape"::: |
| **Portrait** | :::image type="content" source="images/sensor-orientation-2-small.jpg" alt-text="Appareil à priorité Paysage en mode d’orientation Portrait"::: | :::image type="content" source="images/sensor-orientation-3-small.jpg" alt-text="Appareil à priorité Portrait en mode d’orientation Portrait"::: |
| **LandscapeFlipped** | :::image type="content" source="images/sensor-orientation-4-small.jpg" alt-text="Appareil à priorité Paysage en mode d’orientation LandscapeFlipped"::: | :::image type="content" source="images/sensor-orientation-5-small.jpg" alt-text="Appareil à priorité Portrait en mode d’orientation LandscapeFlipped":::
| **PortraitFlipped** | :::image type="content" source="images/sensor-orientation-6-small.jpg" alt-text="Appareil à priorité Paysage en mode d’orientation PortraitFlipped"::: | :::image type="content" source="images/sensor-orientation-7-small.jpg" alt-text="Appareil à priorité Portrait en mode d’orientation PortraitFlipped"::: |

## <a name="devices-broadcasting-display-and-headless-devices"></a>Appareils diffusant leur affichage et appareils sans affichage

Certains appareils sont capables de diffuser leur affichage vers un autre appareil. Par exemple, vous pouvez avoir une tablette capable de diffuser son affichage vers un projecteur en orientation paysage. Dans cette situation, il est important de garder à l’esprit que l’orientation de l’appareil est basée sur celle de l’appareil d’origine et non sur celle de l’appareil qui affiche. Par conséquent, un accéléromètre indiquerait les données pour la tablette.

En outre, certains appareils n’ont pas d’affichage. L’orientation par défaut de ces appareils est le mode Portrait.

## <a name="display-orientation-and-compass-heading"></a>Orientation de l’affichage et orientation de la boussole

L’orientation de la boussole dépend des axes de référence. Elle change donc avec l’orientation de l’appareil. Vous compensez en vous appuyant sur le tableau suivant (en supposant que l’utilisateur est orienté au nord).

| Orientation de l’affichage | Axe de référence pour l’orientation de la boussole | Titre de l’API Compass lorsque le nord est orienté vers le Nord (paysage en premier) | En-tête API Compass en regard du Nord (en premier) |Compensation de l’en-tête de boussole (paysage-First) | Compensation de l’en-tête de boussole (portrait-premier) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Paysage           | -Z | 0   | 270 | Direction               | (Orientation + 90) % 360  |
| Portrait            |  O | 90  | 0   | (Orientation + 270) % 360 |  Direction              |
| LandscapeFlipped    |  Z | 180 | 90  | (Orientation + 180) % 360 | (Orientation + 270) % 360 |
| PortraitFlipped     |  O | 270 | 180 | (Orientation + 90) % 360  | (Orientation + 180) % 360 |

Modifiez le cap de la boussole tel qu’indiqué dans le tableau pour afficher correctement le cap. L’extrait de code suivant montre comment procéder.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>Orientation de l’affichage avec l’accéléromètre et le gyromètre

Le tableau suivant convertit les données de l’accéléromètre et du gyromètre pour obtenir l’orientation de l’affichage.

| Axes de référence        |  X |  O | Z |
|-----------------------|----|----|---|
| **Paysage**         |  X |  O | Z |
| **Portrait**          |  O | -X | Z |
| **LandscapeFlipped**  | -X | -y | Z |
| **PortraitFlipped**   | -y |  X | Z |

L’exemple de code suivant applique ces conversions au gyromètre.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>Orientation d’affichage et orientation d’appareil

Les données [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) doivent être modifiées d’une manière différente. Considérez ces différentes orientations comme des rotations dans le sens inverse des aiguilles d’une position vers l’axe z. nous devons donc inverser la rotation pour revenir à l’orientation de l’utilisateur. Pour les données de quaternion, nous pouvons utiliser la formule d’Euler pour définir une rotation avec un quaternion de référence. Nous pouvons également utiliser une matrice de rotation de référence.

:::image type="content" source="images/eulers-formula.png" alt-text="Formule de Euler":::

Pour obtenir l’orientation relative souhaitée, multipliez l’objet de référence par rapport à l’objet absolu. Notez que cette formule mathématique n’est pas commutative.

:::image type="content" source="images/orientation-formula.png" alt-text="Multiplier l’objet de référence par rapport à l’objet absolu":::

Dans l’expression précédente, l’objet absolu est retourné par les données de capteur.

| Orientation de l’affichage  | Rotation dans le sens inverse des aiguilles d’une montre autour de Z | Quaternion de référence (rotation inverse) | Matrice de rotation de référence (rotation inverse) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Paysage**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Portrait**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1 0 0<br/> 0 0 1]             |

## <a name="see-also"></a>Voir aussi

[Intégration de capteurs de mouvement et d’orientation](/windows-hardware/design/whitepapers/integrating-motion-and-orientation-sensors)
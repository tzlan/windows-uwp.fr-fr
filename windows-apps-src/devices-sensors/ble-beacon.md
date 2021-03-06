---
title: Annonces publicitaires Bluetooth
description: Cette section contient des articles expliquant comment intégrer des annonces Bluetooth Low Energy (LE) dans les applications de plateforme Windows universelle (UWP) par le biais de l’utilisation des API AdvertisementWatcher et AdvertisementPublisher.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
ms.localizationpriority: medium
ms.openlocfilehash: 319038f8922aac2686dab6836f94ead5018effdf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159743"
---
# <a name="bluetooth-le-advertisements"></a>Annonces publicitaires Bluetooth LE


**API importantes**

-   [**Windows.Devices.Bluetooth.Advertisement**](/uwp/api/windows.devices.bluetooth.advertisement)

Cet article fournit une vue d’ensemble des balises d’annonce Bluetooth Low Energy (LE) pour les applications de plateforme Windows universelle (UWP).  

> [!Important]
> Vous devez déclarer la fonctionnalité « Bluetooth » dans *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="overview"></a>Vue d’ensemble

Un développeur peut exécuter deux fonctions principales à l’aide des API d’annonce Bluetooth LE :

-   [Advertisement Watcher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) : écouter les balises proches et les filtrer en fonction de la charge utile ou de la proximité.  
-   [Advertisement Publisher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) : définir une charge utile pour Windows afin de créer des annonces au nom des développeurs.  

L’exemple de code complet est disponible dans l’[exemple d’annonce Bluetooth](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothAdvertisement) sur Github.

## <a name="basic-setup"></a>Configuration de base

Pour utiliser les fonctionnalités Bluetooth LE de base dans une application de plateforme Windows universelle, vous devez vérifier la fonctionnalité Bluetooth dans le fichier Package.appxmanifest.

1. Ouvrez Package.appxmanifest.
2. Accédez à l’onglet Capabilities.
3. Recherchez Bluetooth dans la liste de gauche et cochez la case située en regard.

## <a name="publishing-advertisements"></a>Publication d’annonces publicitaires

Les annonces Bluetooth LE permettent à votre appareil de baliser en permanence une charge utile spécifique, appelée annonce publicitaire. Cette annonce est visible par n’importe quel appareil compatible Bluetooth LE à proximité, s’il est configuré pour écouter cette annonce spécifiquement.

> **Remarque**: pour la confidentialité de l’utilisateur, la durée de vie de votre publication est liée à celle de votre application. Vous pouvez créer une annonce BluetoothLEAdvertisementPublisher et appeler Start dans une tâche en arrière-plan pour l’annonce en arrière-plan. Pour plus d’informations sur les tâches en arrière-plan, consultez [Lancement, reprise et tâches en arrière-plan](../launch-resume/index.md).

### <a name="basic-publishing"></a>Publication de base

Il existe plusieurs manières d’ajouter des données à une annonce. Cet exemple montre une façon courante de créer une annonce concernant une société. 

Tout d’abord, créez l’éditeur d’annonce qui vérifie si l’appareil balise ou non une annonce particulière.

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

Ensuite, créez une section de données personnalisée. Cet exemple utilise une valeur **CompanyId** non attribuée de *0xFFFE* et ajoute le texte *Hello World* dans l’annonce. 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

Maintenant que l’éditeur est créé et installé, vous pouvez appeler **Start** pour lancer l’annonce.

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>Visionnage des annonces

### <a name="basic-watching"></a>Visionnage de base

Le code suivant montre comment créer un observateur d’annonces Bluetooth LE, définir un rappel et commencer à visionner toutes les annonces LE.

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
``` 

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>Analyse active
Pour recevoir également des annonces en réponse à une analyse, définissez ce qui suit après avoir créé l’observateur. Notez que cette opération consomme davantage de puissance et n’est pas disponible dans les modes en arrière-plan.

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>Visionnage d’un modèle d’annonce spécifique

Parfois, vous voulez écouter une annonce spécifique. Dans ce cas, écoutez une annonce contenant une charge utile avec une société fictive (identifiée par 0xFFFE) et contenant la chaîne *Hello World*. Vous pouvez associer cette information à l’exemple Publication de base pour obtenir une machine Windows qui émet l’annonce et une autre qui l’affiche. N’oubliez pas de définir ce filtre d’annonce avant de lancer l’observateur !

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>Visionnage d’une annonce à proximité

Parfois, vous souhaitez ne déclencher votre observateur que si votre appareil est à portée de l’annonce. Vous pouvez définir votre propre portée. Notez simplement que les valeurs seront comprises entre 0 et -128. 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>Estimation de la distance

Lorsque le rappel de votre observateur Bluetooth LE se déclenche, les arguments eventArgs incluent une valeur RSSI indiquant la force du signal reçu (autrement dit, la qualité du signal Bluetooth).

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

Cette valeur peut être convertie en une distance, mais ne doit pas servir à mesurer des distances réelles, car chaque radio est différente. Différents facteurs environnementaux peuvent compliquer l’évaluation de la distance (comme les murs, le boîtier de la radio et même l’humidité ambiante).

Une autre solution permettant d’évaluer une distance consiste à définir des « compartiments ». Les radios ont tendance à émettre entre 0 et 50 dBm quand elles sont très proches, entre -50 et -90 lorsqu’elles sont à moyenne distance, et au-dessous de -90 lorsqu’elles sont éloignées. La méthode d’ajustement par tâtonnements est la meilleure pour déterminer les compartiments idéaux pour votre application.
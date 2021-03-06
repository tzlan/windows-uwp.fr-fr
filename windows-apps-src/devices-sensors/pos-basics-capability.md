---
title: Fonctionnalité d'appareil PointOfService
description: La fonctionnalité PointOfService est requise pour l'utilisation de l’espace de noms Windows.Devices.PointOfService.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: f120e093ab65224ca4c32b64640100b6abd1a36a
ms.sourcegitcommit: 0301f794f994e604ffc131de7e40ffcede3530c9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72036263"
---
# <a name="pointofservice-device-capability"></a>Fonctionnalité d'appareil PointOfService
Vous demandez l’accès aux API PointOfService en déclarant la fonctionnalité dans votre manifeste de package d’application. Vous pouvez déclarer la plupart des fonctionnalités à l’aide du concepteur de manifeste, dans Microsoft Visual Studio, ou vous pouvez les ajouter manuellement.  

> [!Important]
> Vous recevrez le message d’erreur **System. UnauthorizedAccessException** quand vous tenterez d’utiliser une API dans l’espace de noms Windows. Devices. PointOfService si vous ne déclarez pas la fonctionnalité **PointOfService** dans votre manifeste d’application. 

## <a name="declare-capability-using-manifest-designer"></a>Déclarez la fonctionnalité à l’aide du Concepteur de manifeste

1. Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2. Double-cliquez sur le fichier **Package.appxmanifest**.  
*Si le fichier manifeste est déjà ouvert en mode code XML, Visual Studio vous invite à le fermer.*
3. Cliquez sur l’onglet **Fonctionnalités**.
4. Cliquez sur la case à cocher en regard de **Point de service** dans la liste des fonctionnalités pour activer la fonctionnalité d'appareil de point de service


## <a name="declare-capability-manually"></a>Déclarer la fonctionnalité manuellement

1. Dans l’**Explorateur de solutions**, développez le nœud de projet de votre application UWP.
2. Cliquez avec le bouton droit sur le fichier **Package.appxmanifest** et sélectionnez **Afficher le code**
3. Ajoutez l’élément PointOfService DeviceCapability dans la section Fonctionnalités de votre manifeste d’application.  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```

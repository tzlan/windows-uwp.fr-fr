---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame.
title: Séquence de photos variables
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 376d5cb011ab4a72d7715a36a1522ab91303c1f9
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363712"
---
# <a name="variable-photo-sequence"></a>Séquence de photos variables



Cet article indique comment capturer une séquence de photos variables pour capturer plusieurs trames d’images en une succession rapide et configurer des paramètres de mise au point, de flash, de sensibilité ISO, d’exposition et de compensation de l’exposition différents pour chaque trame. Cette fonctionnalité permet par exemple de créer des images HDR (High Dynamic Range).

Si vous souhaitez capturer des images HDR, mais que vous ne voulez pas implémenter votre propre algorithme de traitement, vous pouvez utiliser l’API [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) pour utiliser les fonctionnalités HDR intégrées dans Windows. Pour plus d’informations, voir [Capture photo avec plage dynamique élevée (HDR)](high-dynamic-range-hdr-photo-capture.md).

> [!NOTE] 
> Cet article repose sur les concepts et le code décrits dans [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), qui décrit comment implémenter la capture photo et vidéo de base. Nous vous recommandons de vous familiariser avec le modèle de capture multimédia de base dans cet article avant de passer à des scénarios de capture plus avancés. Le code de cet article part du principe que votre application possède déjà une instance de MediaCapture initialisée correctement.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>Configurer votre application pour utiliser une capture de séquence de photos variables

Outre les espaces de noms requis pour la capture multimédia de base, l’implémentation d’une capture de séquence de photos variables requiert les espaces de noms suivants.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVPSUsing":::

Déclarez une variable membre pour stocker l’objet [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) , qui est utilisé pour initier la capture de la séquence de photos. Déclarez un tableau d’objets [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) pour stocker chaque image capturée dans la séquence. Déclarez également un tableau pour stocker l’objet [**CapturedFrameControlValues**](/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) pour chaque trame. Ce dernier peut être utilisé par votre algorithme de traitement d’image afin de déterminer les paramètres utilisés pour la capture de chaque trame. Enfin, déclarez un index qui sera utilisé pour suivre l’image en cours de capture dans la séquence.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetVPSMemberVariables":::

## <a name="prepare-the-variable-photo-sequence-capture"></a>Préparer la capture de la séquence de photos variables

Une fois que vous avez initialisé votre [MediaCapture](./index.md), assurez-vous que les séquences de photos variables sont prises en charge sur l’appareil actuel en obtenant une instance de l’élément [**VariablePhotoSequenceController**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController)à partir de l’objet [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) de la capture multimédia et en activant la propriété [**Supported**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsVPSSupported":::

Obtenez un objet [**FrameControlCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) à partir du contrôleur de séquence de photos variables. Cet objet possède une propriété pour chaque paramètre pouvant être configuré par trame d’une séquence de photos. notamment :

-   [**Vulnérabilité**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**Clignote**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**Priorité**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

Cet exemple définit une valeur de compensation d’exposition différente pour chaque trame. Pour vérifier que la compensation d’exposition est prise en charge pour les séquences de photos sur l’appareil actuel, vérifiez la propriété [**Supported**](/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) de l’objet [**FrameExposureCompensationCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) accessible par le biais de la propriété **ExposureCompensation**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetIsExposureCompensationSupported":::

Créez un objet [**FrameController**](/uwp/api/Windows.Media.Devices.Core.FrameController) pour chaque trame que vous souhaitez capturer. Cet exemple capture trois trames. Définissez les valeurs des contrôles que vous souhaitez faire varier pour chaque trame. Ensuite, supprimez la collection [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) de l’objet **VariablePhotoSequenceController** et ajoutez chaque contrôleur de trame à la collection.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitFrameControllers":::

Créez un objet [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) pour définir l’encodage que vous souhaitez utiliser pour les images capturées. Appelez la méthode statique [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync) en indiquant les propriétés de codage. Cette méthode renvoie un objet [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture). Enfin, enregistrez les gestionnaires d’événements pour les événements [**PhotoCapture**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) et [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPrepareVPS":::

## <a name="start-the-variable-photo-sequence-capture"></a>Démarrer la capture de la séquence de photos variables

Pour démarrer la capture de la séquence de photos variables, appelez [**VariablePhotoSequenceCapture.StartAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync). Veillez à initialiser les tableaux pour stocker les images capturées et les valeurs de contrôle de trame et à définir l’index actuel sur la valeur 0. Définissez la variable d’état d’enregistrement de votre application et mettez à jour votre interface utilisateur pour désactiver le démarrage d’une autre capture pendant que cette capture est en cours.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetStartVPSCapture":::

## <a name="receive-the-captured-frames"></a>Recevoir les trames capturées

L’événement [**PhotoCapture**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) est déclenché pour chaque frame capturé. Enregistrez les valeurs de contrôle de trame et l’image capturée pour la trame, puis augmentez l’index de trame actuel. Cet exemple indique comment obtenir une représentation [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) de chaque trame. Pour plus d’informations sur l’utilisation de **SoftwareBitmap**, voir [Acquisition d’images](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOnPhotoCaptured":::

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>Achever la capture de la séquence de photos variables

L’événement [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) est déclenché lorsque tous les frames de la séquence ont été capturés. Mettez à jour l’état d’enregistrement de votre application et votre interface utilisateur pour permettre à l’utilisateur de créer de nouvelles captures. À ce stade, vous pouvez transférer les images capturées et les valeurs de contrôle de trame dans votre code de traitement d’image.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetOnStopped":::

## <a name="update-frame-controllers"></a>Mettre à jour des contrôleurs de trame

Si vous souhaitez effectuer une autre capture de séquence de photos variables avec différents paramètres de trame, vous n’avez pas besoin de réinitialiser complètement l’élément **VariablePhotoSequenceCapture**. Vous pouvez supprimer la collection [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) et ajouter de nouveaux contrôleurs de trame ou bien modifier les valeurs du contrôleur de trame existant. L’exemple suivant vérifie l’objet [**FrameFlashCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) pour vérifier que l’appareil actuel prend en charge la puissance de Flash et de Flash pour les images de séquence de photos variables. Si c’est le cas, chaque trame est mise à jour pour activer le flash à 100 %. Les valeurs de compensation d’exposition précédemment définies pour chaque trame sont toujours actives.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUpdateFrameControllers":::

## <a name="clean-up-the-variable-photo-sequence-capture"></a>Nettoyer la capture de la séquence de photos variables

Lorsque vous avez terminé de capturer des séquences de photos de photos ou si votre application est en pause, nettoyez l’objet de séquence de photos variable en appelant [**FinishAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync). Annulez l’inscription des gestionnaires d’événements de l’objet et définissez-les sur la valeur Null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCleanUpVPS":::

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 

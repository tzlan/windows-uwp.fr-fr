---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: Cet article vous explique comment valoriser les fonctionnalités spécifiques d’interface utilisateur d’appareil photo présentes de manière exclusive sur les appareils mobiles.
title: Fonctionnalités d’interface utilisateur d’appareil photo pour les appareils mobiles
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e1076c0632299ff79a8ca2fc226865d0ff3ebef
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362892"
---
# <a name="camera-ui-features-for-mobile-devices"></a>Fonctionnalités d’interface utilisateur d’appareil photo pour les appareils mobiles

Cet article vous explique comment valoriser les fonctionnalités spécifiques d’interface utilisateur d’appareil photo présentes de manière exclusive sur les appareils mobiles. 

## <a name="add-the-mobile-extension-to-your-project"></a>Ajouter l’extension mobile à votre projet 

Pour utiliser ces fonctionnalités, vous devez ajouter à votre projet une référence au kit de développement logiciel (SDK) Microsoft Mobile Extension pour la plateforme d’application universelle.

**Pour ajouter une référence au kit de développement logiciel (SDK) de l’extension mobile pour la prise en charge du bouton de l’appareil photo, procédez comme suit :**

1.  Dans **Explorateur de solutions**, cliquez avec le bouton droit sur **références** , puis sélectionnez **Ajouter une référence**.

2.  Développez le nœud **Universelle Windows** et sélectionnez**Extensions**.

3.  Sélectionnez la case **Kit de développement logiciel Microsoft Mobile Extension pour la plateforme d’application universelle**.

## <a name="hide-the-status-bar"></a>Masquer la barre d’état

Les périphériques mobiles ont un contrôle [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) qui fournit à l’utilisateur des informations d’État sur l’appareil. Ce contrôle occupe de l’espace sur l’écran , ce qui peut interférer avec l’interface utilisateur de capture multimédia. Vous pouvez masquer la barre d’état en appelant [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync). Toutefois cet appel doit être effectué depuis un bloc conditionnel où vous utilisez la méthode [**ApiInformation.IsTypePresent**](/uwp/api/windows.foundation.metadata.apiinformation.istypepresent) pour déterminer si l’API est disponible. Cette méthode renvoie uniquement la valeur true sur les appareils mobiles qui prennent en charge la barre d’état. Vous devez masquer la barre d’état au lancement de votre application ou lorsque vous commencez à afficher un aperçu à partir de l’appareil photo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetHideStatusBar":::

Lorsque votre application s’arrête ou lorsque l’utilisateur quitte la page de capture multimédia de votre application, vous pouvez rendre le contrôle de nouveau visible.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetShowStatusBar":::

## <a name="use-the-hardware-camera-button"></a>Utiliser le bouton matériel de l’appareil photo

Certains appareils mobiles disposent d’un bouton matériel dédié à l’appareil photo que certains utilisateurs préfèrent à une commande tactile. Pour être averti de l’utilisation du bouton matériel de l’appareil photo, enregistrez un gestionnaire pour l’événement [**HardwareButtons.CameraPressed**](/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed). Cette API est disponible sur les appareils mobiles, par conséquent, vous devez utiliser de nouveau l’élément **IsTypePresent** pour vous assurer que l’API est prise en charge sur l’appareil actuel avant d’essayer d’y accéder.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetPhoneUsing":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetRegisterCameraButtonHandler":::

Dans le gestionnaire de l’événement **CameraPressed**, vous pouvez lancer une capture de photos.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetCameraPressed":::

Lorsque votre application s’arrête ou que l’utilisateur quitte la page de capture multimédia de votre application, annulez l’enregistrement du gestionnaire de boutons matériels.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetUnregisterCameraButtonHandler":::

## <a name="related-topics"></a>Rubriques connexes

* [Appareil photo](camera.md)
* [Capture photo, vidéo et audio de base à l’aide de MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)

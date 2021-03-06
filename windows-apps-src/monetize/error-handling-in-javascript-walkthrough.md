---
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Découvrez comment intercepter et gérer les erreurs d’un classe AdControl dans une application JavaScript et HTML5 en suivant cette procédure pas à pas.
title: Gestion des erreurs dans la procédure pas à pas pour JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, gestion des erreurs, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: f6567432714fb68618510923e49f2467daefdc54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167683"
---
# <a name="error-handling-in-javascript-walkthrough"></a>Gestion des erreurs dans la procédure pas à pas pour JavaScript

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette procédure pas à pas montre comment intercepter des erreurs liées à Active Directory dans votre application JavaScript. Cette procédure pas à pas utilise un [classe AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pour afficher une bannière publicitaire, mais les concepts généraux qu’il contient s’appliquent également aux publicités interstitielles et aux publicités natives.

Ces exemples supposent que vous disposez d’une application JavaScript qui contient un **classe AdControl**. Pour obtenir des instructions pas à pas qui montrent comment ajouter un **AdControl** à votre application, voir [AdControl en HTML 5 JavaScript](adcontrol-in-html-5-and-javascript.md). Pour un exemple de projet complet illustrant l’ajout de bannières publicitaires à une application HTML/JavaScript, voir [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

1.  Dans le fichier default.html, ajoutez une valeur pour l’événement **onErrorOccurred** où vous définissez les options **data-win-options** dans l’élément **div** du contrôle **AdControl**. Recherchez le code ci-dessous dans le fichier default.html.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    Après l’attribut **adUnitId**, ajoutez la valeur de l’événement **onErrorOccurred**.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  Créez un élément **div** qui affiche le texte afin de pouvoir consulter les messages générés. Pour ce faire, ajoutez le code suivant après l’élément **div** de **myAd**.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Créez un **AdControl** pour déclencher un événement d’erreur. Il ne peut exister qu’un ID d’application pour tous les objets **AdControl** d’une application. Par conséquent, la création d’une application supplémentaire avec un autre ID d’application déclenche une erreur au moment de l’exécution. Pour ce faire, après les sections **div** précédentes que vous avez ajoutées, ajoutez le code suivant au corps de la page default.html.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  Dans le fichier default.js du projet, après la fonction d’initialisation par défaut, vous allez ajouter le gestionnaire d’événements pour **errorLogger**. Faites défiler le fichier jusqu’en bas, puis après le dernier point-virgule, insérez le code suivant.
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  Générez et exécutez le fichier. Vous pouvez voir la publicité d’origine de l’exemple d’application généré précédemment ainsi que le texte figurant sous cette publicité qui décrit l’erreur. Vous ne voyez pas la publicité dotée de l’ID de publicité **liveAd**.

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
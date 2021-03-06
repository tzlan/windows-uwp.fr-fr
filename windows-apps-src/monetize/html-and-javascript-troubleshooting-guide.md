---
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: Découvrez les solutions aux problèmes de développement courants liés aux bibliothèques de publicités Microsoft dans les applications JavaScript/HTML.
title: Guide de résolution des problèmes pour HTML et JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, publicité, classe AdControl, dépannage, HTML, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 53c2d18c56626d4a71b4326b1ab7e292a2267dca
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174983"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>Guide de résolution des problèmes pour HTML et JavaScript

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cette rubrique contient des solutions aux problèmes de développement courants liés aux bibliothèques de publicités Microsoft dans les applications JavaScript/HTML.

* [HTML](#html)
  * [AdControl invisible](#html-notappearing)
  * [Une boîte noire clignote et disparaît](#html-blackboxblinksdisappears)
  * [Non-actualisation des publicités](#html-adsnotrefreshing)

* [JavaScript](#js)
  * [AdControl invisible](#js-adcontrolnotappearing)
  * [Une boîte noire clignote et disparaît](#js-blackboxblinksdisappears)
  * [Non-actualisation des publicités](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl invisible

1.  Assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée dans le fichier Package.appxmanifest.

2.  Vérifiez la présence des informations de référence JavaScript. Sans la référence ad.js dans la section &lt;head&gt; (après la référence default.js), le contrôle **AdControl** n’est pas en mesure de s’afficher, et une erreur se produit lors de la génération.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  Vérifiez l’ID de l’application et l’ID d’unité publicitaire. Ces ID doivent correspondre à l’ID d’application et à l’ID d’unité Active Directory que vous avez obtenus dans l’espace partenaires. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  Vérifiez les propriétés **height** et **width**. Celles-ci doivent être définies sur l’une des [tailles de publicités prises en charge pour les bannières publicitaires](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  Vérifiez la position des éléments. Le contrôle [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) doit se situer à l’intérieur de la zone d’affichage.

6.  Vérifiez la propriété **Visibility** . Cette propriété ne doit pas être définie sur collapsed ou hidden. Cette propriété peut être incluse (comme illustré ci-dessous) ou définie dans une feuille de style externe.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  Vérifiez la propriété **position**. La propriété position doit être définie sur une valeur appropriée en fonction des autres propriétés de l’élément (par exemple, les marges dans l’élément parent et l’index z). Cette propriété peut être incluse (comme illustré ci-dessous) ou définie dans une feuille de style externe.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  Vérifiez la propriété **z-index**. La propriété **z-index** doit être définie sur une valeur suffisamment élevée pour que le contrôle **AdControl** apparaisse toujours au-dessus des autres éléments. Cette propriété peut être incluse (comme illustré ci-dessous) ou définie dans une feuille de style externe.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  Vérifiez les feuilles de style externes. Si les propriétés sont définies dans l’élément **AdControl** par le biais d’une feuille de style externe, assurez-vous que toutes les propriétés mentionnées ci-dessus sont correctement définies.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. Vérifiez le parent du **AdControl**. Si le **AdControl** réside dans un élément parent, ce dernier doit être actif et visible.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. Vérifiez que le **AdControl** n’est pas masqué dans la fenêtre d’affichage. Le **AdControl** doit être visible afin que les publicités s’affichent correctement.

12. Les valeurs dynamiques des paramètres [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) et [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) ne doivent pas être testées dans l’émulateur. Pour vous assurer que le **classe AdControl** fonctionne comme prévu, utilisez les [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour **ApplicationID** et **AdUnitId**.

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Une boîte noire clignote et disparaît

1.  Vérifiez toutes les étapes indiquées dans la section précédente [AdControl invisible](#html-notappearing).

2.  Gérez l’événement **onErrorOccurred**, puis utilisez le message transmis au gestionnaire d’événements pour déterminer si une erreur s’est produite et identifier le type d’erreur levée. Pour plus d’informations, voir [Gestion des erreurs dans la procédure pas à pas pour JavaScript](error-handling-in-javascript-walkthrough.md).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    L’erreur la plus courante provoquant une boîte noire est la suivante : « Aucune publicité disponible ». Cette erreur signifie qu’aucune publicité n’est disponible pour être retourné à partir de la demande.

3.  Le contrôle **AdControl** se comporte normalement. Par défaut, le **AdControl** est réduit s’il ne peut pas afficher de publicité. Si d’autres éléments sont des enfants du même parent, ils peuvent être déplacés pour combler le vide du contrôle **AdControl** réduit, et développés à la prochaine demande.

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Non-actualisation des publicités

1.  Vérifiez la propriété **isAutoRefreshEnabled**. Par défaut, cette propriété facultative est définie sur true. Lorsque la valeur est false, la méthode **Refresh** doit être utilisée pour récupérer une autre publicité.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  Vérifiez les appels à la méthode **refresh**. Lors de l’utilisation de l’actualisation automatique, l' **actualisation** ne peut pas être utilisée pour récupérer une autre publicité. Lors de l’utilisation d’une actualisation manuelle, l' **actualisation** ne doit être appelée qu’après un minimum de 30 à 60 secondes, en fonction de la connexion de données actuelle de l’appareil.

    Cet exemple montre comment utiliser la méthode **refresh**. Le code HTML suivant montre un exemple d’instanciation du contrôle **AdControl** avec la propriété **isAutoRefreshEnabled** définie sur false.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    Cet exemple montre comment utiliser la fonction **refresh**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl invisible

1.  Assurez-vous que la fonctionnalité **Internet (client)** est sélectionnée dans le fichier Package.appxmanifest.

2.  Vérifiez que le contrôle **AdControl** est instancié. Si le **classe AdControl** n’est pas instancié. c’est qu’il n’est pas disponible.

    Les extraits de code suivants illustrent un exemple d’instanciation du contrôle **AdControl**. Le code HTML suivant montre un exemple de configuration de l’interface utilisateur pour le contrôle **AdControl**

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    Le code JavaScript suivant illustre un exemple d’instanciation du contrôle **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  Vérifiez l’élément parent. La ** &lt; balise &gt; div** parente doit être correctement assignée, active et visible.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  Vérifiez l’ID de l’application et l’ID d’unité publicitaire. Ces ID doivent correspondre à l’ID d’application et à l’ID d’unité Active Directory que vous avez obtenus dans l’espace partenaires. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  Vérifiez l’élément parent du **AdControl**. Le parent doit être actif et visible.

6.  Les valeurs dynamiques des paramètres **ApplicationId** et **AdUnitId** ne doivent pas être testées dans l’émulateur. Pour vous assurer que le **classe AdControl** fonctionne comme prévu, utilisez les [valeurs de test](set-up-ad-units-in-your-app.md#test-ad-units) pour **ApplicationID** et **AdUnitId**.

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>Une boîte noire clignote et disparaît

1.  Vérifiez toutes les étapes indiquées dans la section [AdControl invisible](#js-adcontrolnotappearing).

2.  Gérez l’événement **onErrorOccurred**, puis utilisez le message transmis au gestionnaire d’événements pour déterminer si une erreur s’est produite et identifier le type d’erreur levée. Pour plus d’informations, voir [Gestion des erreurs dans la procédure pas à pas pour JavaScript](error-handling-in-javascript-walkthrough.md).

    Cet exemple montre comment implémenter un gestionnaire d’erreurs signalant des messages d’erreur. Cet extrait de code HTML fournit un exemple de configuration de l’interface utilisateur pour afficher les messages d’erreur.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    Cet exemple montre comment instancier le contrôle **AdControl**. Cette fonction est insérée dans le fichier app.onactivated.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    Cet exemple illustre le signalement des erreurs. Cette fonction est insérée sous la fonction exécutée automatiquement dans le fichier default.js.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    L’erreur la plus courante provoquant une boîte noire est la suivante : « Aucune publicité disponible ». Cette erreur signifie qu’aucune publicité n’est disponible pour être retourné à partir de la demande.

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Non-actualisation des publicités

1.  Vérifiez si la propriété [IsAutoRefreshEnabled](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) de votre **AdControl** est définie sur false. Par défaut, cette propriété facultative a la valeur **true**. Lorsque la valeur est **false**, la méthode **Refresh** doit être utilisée pour récupérer une autre publicité.

2.  Vérifiez les appels à la méthode [Refresh](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) . Si vous utilisez l’actualisation automatique (**IsAutoRefreshEnabled** est définie sur **true**), la méthode **Refresh** ne permet pas de récupérer une autre publicité. Si vous utilisez l’actualisation manuelle (**IsAutoRefreshEnabled** est définie sur **false**), la méthode **Refresh** doit être appelée uniquement après un minimum de 30 à 60 secondes en fonction de la connexion de données actuelle de l’appareil.

    Cet exemple montre comment créer l’élément **div** pour le contrôle **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    Cet exemple montre comment utiliser la fonction **Refresh**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  Le contrôle **AdControl** se comporte normalement. Parfois, une même publicité s’affiche plusieurs fois dans une ligne, ce qui donne l’impression que les publicités ne sont pas actualisées.

 

 
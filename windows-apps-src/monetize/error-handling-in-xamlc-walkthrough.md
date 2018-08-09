---
author: mcleanbyron
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: Découvrez comment intercepter les erreurs AdControl dans votre application.
title: Gestion des erreurs dans la procédure pas à pas pour XAML/C#
ms.author: mcleans
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, annonces publicitaires, publicité, gestion des erreurs, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 5bb793e5ca9bb44e888581f1d5dd3142d0b09ee8
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880790"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>Gestion des erreurs dans la procédure pas à pas pour XAML/C#

Cette procédure pas à pas montre comment intercepter les erreurs liées à aux publicités dans votre application. Cette procédure pas à pas utilise un [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) pour afficher une bannière, mais les concepts généraux s’appliquent également aux spots publicitaires et aux publicités natives.

Ces exemples partent du principe que vous disposez d’une application XAML/C# qui contient un **AdControl**. Pour obtenir des instructions pas à pas qui montrent comment ajouter un **AdControl** à votre application, voir [AdControl en XAML et .NET](adcontrol-in-xaml-and--net.md). 

1.  Dans votre fichier MainPage.xaml, recherchez la définition du contrôle **AdControl**. Ce code se présente ainsi:
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   Après la propriété **Width**, mais avant la balise de fermeture, affectez le nom d’un gestionnaire d’événements d’erreur à l’événement [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx). Dans cette procédure pas à pas, le nom du gestionnaire d’événements d’erreur est **OnAdError**.
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  Pour générer une erreur lors de l’exécution, créez un second contrôle **AdControl** avec un ID d’application différent. Comme tous les objets **AdControl** d’une application doivent utiliser le même ID d’application, la création d’un objet **AdControl** supplémentaire doté d’un autre ID d’application lève une erreur.

    Définissez un second objet **AdControl** dans le fichier MainPage.xaml juste après le premier **AdControl**, puis définissez la propriété [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) sur zéro («0»).
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  Dans MainPage.xaml.cs, ajoutez le gestionnaire d’événements **OnAdError** suivant à la classe **MainPage**. Ce gestionnaire d’événements écrit les informations dans la fenêtre **Sortie** de VisualStudio.
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  Générez et exécutez le projet. Après l’exécution de l’application, un message semblable à celui qui suit s’affiche dans la fenêtre **Sortie** de VisualStudio.
    ```
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>Rubriques connexes

* [Exemples de publicité sur GitHub](http://aka.ms/githubads)
---
author: mcleblanc
description: Derrière votre interface utilisateur se trouvent les couches métier et les couches de données.
title: Portage des couches métier et des couches de données de Silverlight pour Windows Phone vers UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 860b42ca05c95768ca694d13971da278e2129142
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.locfileid: "229996"
---
#  <a name="porting-windows-phone-silverlight-business-and-data-layers-to-uwp"></a>Portage des couches métier et des couches de données de Silverlight pour Windows Phone vers UWP

\[ Article mis à jour pour les applications UWP sur Windows10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Rubrique précédente : [Portage pour le modèle d’E/S, d’appareil et d’application](wpsl-to-uwp-input-and-sensors.md).

Derrière votre interface utilisateur se trouvent les couches métier et les couches de données. Le code de ces couches appelle le système d’exploitation et les API .NET Framework (par exemple, pour le traitement en arrière-plan, l’emplacement, l’appareil photo, le système de fichiers, le réseau et l’accès à d’autres données). La grande majorité de ces éléments sont [disponibles pour une application de plateforme Windows universelle (UWP)](https://msdn.microsoft.com/library/windows/apps/br211369). Vous pouvez donc espérer être en mesure de porter la plus grande partie de ce code sans avoir à le modifier.

## <a name="asynchronous-methods"></a>Méthodes asynchrones

L’une des priorités de la plateforme Windows universelle (UWP) consiste à vous permettre de générer des applications qui réagissent efficacement et de manière cohérente. Les animations sont toujours fluides, et les interactions tactiles (telles que le balayage et les mouvements panoramiques) sont instantanées, sans retard, ce qui donne le sentiment que l’interface utilisateur ne fait qu’un avec votre doigt. Pour ce faire, toute API UWP qui ne peut pas garantir qu’elle s’exécutera en 50 millisecondes maximum est rendue asynchrone ; son nom comprend le suffixe suivant : **Async**. Votre thread d’interface utilisateur réapparaît immédiatement suite à l’appel d’une méthode **Async** ; le travail est alors effectué sur un autre thread. L’utilisation d’une méthode **Async** a été facilitée via l’optimisation de la syntaxe, au moyen de l’opérateur C# **await**, des objets de promesse JavaScript et des continuations C++. Pour en savoir plus, voir [Programmation asynchrone](https://msdn.microsoft.com/library/windows/apps/mt187335).

## <a name="background-processing"></a>Traitement en arrière-plan

Une application Silverlight pour Windows Phone peut utiliser un objet **ScheduledTaskAgent** managé pour effectuer une tâche lorsque l’application n’est pas affichée au premier plan. Une application UWP utilise la classe [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) pour créer et enregistrer une tâche en arrière-plan, de la même manière. Vous définissez une classe qui implémente le travail effectué par votre tâche en arrière-plan. Le système exécute régulièrement votre tâche en arrière-plan, en appelant la méthode [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) de votre classe pour exécuter le travail. Dans une application UWP, n’oubliez pas de définir la déclaration **Tâches en arrière-plan** dans le manifeste du package d’application. Pour plus d’informations, voir [Définir des tâches en arrière-plan pour les besoins de votre application](https://msdn.microsoft.com/library/windows/apps/mt299103).

Pour transférer des fichiers de données volumineux en arrière-plan, une application Silverlight pour Windows Phone utilise la classe **BackgroundTransferService**. Pour effectuer cette opération, une application UWP utilise des API de l’espace de noms [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242). Les fonctions utilisent un modèle semblable pour lancer des transferts, mais la nouvelle API présente des performances et fonctionnalités optimisées. Pour en savoir plus, voir [Transfert de données en arrière-plan](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975).

Une application Silverlight pour Windows Phone utilise les classes managées de l’espace de noms **Microsoft.Phone.BackgroundAudio** pour lire les données audio alors que l’application n’est pas au premier plan. UWP utilise le modèle d’application Windows Phone Store. Voir [Contenu audio en arrière-plan](https://msdn.microsoft.com/library/windows/apps/mt282140) et l’exemple [Contenu audio en arrière-plan](http://go.microsoft.com/fwlink/p/?linkid=619997).

## <a name="cloud-services-networking-and-databases"></a>Services cloud, mise en réseau et bases de données

L’hébergement de services de données et d’application dans le cloud est possible par le biais de la plateforme Azure. Voir [Prise en main de Mobile Services](http://go.microsoft.com/fwlink/p/?LinkID=403138). Pour les solutions qui nécessitent à la fois des données en ligne et des données hors connexion, voir [Utilisation de la synchronisation des données hors connexion dans Mobile Services](http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

UWP offre une prise en charge partielle de la classe **System.Net.HttpWebRequest**, mais ne prend pas en charge la classe **System.Net.WebClient**. L’alternative prospective recommandée est la classe [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (ou [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx) si vous avez besoin que votre code soit portable vers d’autres plateformes prenant en charge .NET). Ces API utilisent [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) pour représenter une requête HTTP.

À l’heure actuelle, les applications UWP ne prennent pas en charge les scénarios qui impliquent une utilisation intensive des données, notamment les scénarios métier. Toutefois, vous pouvez utiliser SQLite pour gérer les services de base de données transactionnelle en local. Pour plus d’informations, voir [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Transmettez les URI absolus, et non relatifs, aux types Windows Runtime. Voir [Transmission d’un URI au Windows Runtime](https://msdn.microsoft.com/library/hh763341.aspx).

## <a name="launchers-and-choosers"></a>Lanceurs et sélecteurs

Grâce aux lanceurs et aux sélecteurs (qui figurent dans l’espace de noms **Microsoft.Phone.Tasks**), une application Silverlight pour Windows Phone peut interagir avec le système d’exploitation pour effectuer des opérations courantes (composition d’un e-mail, sélection d’une photo ou partage de différents types de données avec une autre application, par exemple). Recherchez l’élément **Microsoft.Phone.Tasks** dans la rubrique [Mappages des espaces de noms et des classes de Silverlight pour Windows Phone vers Windows 10](wpsl-to-uwp-namespace-and-class-mappings.md) pour trouver le type UWP équivalent. Ces éléments vont des mécanismes du même ordre (lanceurs et sélecteurs) à l’implémentation d’un contrat de partage de données entre applications.

Une application Silverlight pour Windows Phone peut être placée à l’état dormant, voire désactivée, même si vous utilisez la tâche de sélecteur de photos, par exemple. Une application UWP reste active et en cours d’exécution lors de l’utilisation de la classe [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Monétisation (mode d’évaluation et achats dans l’application)

Une application Silverlight pour Windows Phone peut utiliser la classe UWP [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) pour la plupart de ses fonctionnalités d’achat dans l’application et de mode d’évaluation. Ainsi, le code n’a pas besoin d’être porté. Toutefois, une application Silverlight pour Windows Phone appelle l’élément **MarketplaceDetailTask.Show** pour proposer l’application à l’achat :

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Portez le code suivant pour appeler la méthode UWP [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) :

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Si vous disposez de code qui simule les fonctionnalités d’achat dans l’application et d’achat d’application à des fins de test, vous pouvez porter ce code afin qu’il utilise plutôt la classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

## <a name="notifications-for-tile-or-toast-updates"></a>Notifications relatives aux mises à jour de vignette ou toast

Les notifications sont une extension du modèle de notifications Push pour les applications Silverlight pour Windows Phone. Lorsque vous recevez une notification provenant des services de notifications Push Windows (WNS), vous pouvez faire apparaître les informations sur l’interface utilisateur, avec une mise à jour de vignette ou avec un toast. Pour le portage du côté de l’interface utilisateur dédié à vos fonctionnalités de notification, voir [Vignettes et toasts](w8x-to-uwp-porting-xaml-and-ui.md).

Pour plus d’informations sur l’utilisation des notifications dans une application UWP, voir [Envoi de notifications toast](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266).

Pour obtenir des informations et des didacticiels sur l’utilisation des vignettes, des toasts, des badges, des bannières et des notifications dans une application Windows Runtime utilisant C++, C# ou Visual Basic, voir [Utilisation de vignettes, de badges et de notifications toast](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

## <a name="storage-file-access"></a>Stockage (accès aux fichiers)

Le code Silverlight pour Windows Phone qui stocke des paramètres d’application sous forme de paires clé-valeur dans le stockage isolé est porté facilement. Voici un exemple de type avant/après. Tout d’abord, la version Silverlight pour Windows Phone :

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

Et son équivalent UWP:

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

Bien qu’un sous-ensemble de l’espace de noms **Windows.Storage** soit mis à leur disposition, de nombreuses applications Silverlight pour Windows Phone effectuent les E/S de fichiers avec la classe **IsolatedStorageFile**, car elle est prise en charge depuis plus longtemps. Si l’on part du principe que l’élément **IsolatedStorageFile** est utilisé, voici un exemple d’écriture et de lecture d’un fichier (avant et après l’opération), dans la version Silverlight pour Windows Phone pour commencer :

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

Et son équivalent pour UWP:

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Une application Silverlight pour Windows Phone dispose d’un accès en lecture seule à la carte mémoire Secure Digital en option. Une application UWP présente quant à elle un accès en lecture et en écriture à la carte mémoire Secure Digital. Pour plus d’informations, voir [Accéder à la carte SD](https://msdn.microsoft.com/library/windows/apps/mt188699).

Pour plus d’informations sur l’accès aux fichiers photo, musique et vidéo dans une application UWP, voir [Fichiers et dossiers dans les bibliothèques de musique, d’images et de vidéos](https://msdn.microsoft.com/library/windows/apps/mt188703).

Pour plus d’informations, voir [Fichiers, dossiers et bibliothèques](https://msdn.microsoft.com/library/windows/apps/mt185399).

Rubrique suivante : [Portage pour différents facteurs de forme et expériences utilisateur](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Rubriques connexes

* [Mappages des espaces de noms et des classes](wpsl-to-uwp-namespace-and-class-mappings.md)
 

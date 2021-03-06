---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: Cet article vous décrit la procédure d’importation de fichiers multimédias à partir d’un appareil, notamment la recherche de sources de médias disponibles, l’importation de photos et de fichiers sidecar et la suppression des fichiers importés de l’appareil source.
title: Importer des fichiers multimédias
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 626a80b1c3962f5bf12d7a906a61f2f600da5eed
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362532"
---
# <a name="import-media-from-a-device"></a>Importer des fichiers multimédias à partir d’un appareil

Cet article vous décrit la procédure d’importation de fichiers multimédias à partir d’un appareil, notamment la recherche de sources de médias disponibles, l’importation de photos et de fichiers sidecar et la suppression des fichiers importés de l’appareil source.

> [!NOTE] 
> Le code de cet article a été adapté à partir de l' [**exemple d’application UWP MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) . Vous pouvez cloner ou Télécharger cet exemple à partir du [**référentiel git d’exemples d’applications Windows universelles**](https://github.com/Microsoft/Windows-universal-samples) pour afficher le code en contexte ou l’utiliser comme point de départ pour votre propre application.

## <a name="create-a-simple-media-import-ui"></a>Créer une interface simple d’importation de médias
L’exemple de cet article utilise une interface utilisateur épurée prenant en charge les scénarios principaux d’importation de médias. Pour savoir comment créer une interface utilisateur plus robuste adaptée à une application d’importation de médias, consultez l’[**exemple MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Le code XAML suivant crée un panneau d’empilement avec les contrôles suivants :
* Un objet [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) permettant de lancer la recherche des sources à partir desquelles les médias peuvent être importés.
* [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) à répertorier et à sélectionner à partir des sources d’importation de média trouvées.
* Contrôle [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) à afficher et à sélectionner à partir des éléments multimédias de la source d’importation sélectionnée.
* Un élément **Button** permettant de lancer l’importation des éléments multimédias à partir de la source sélectionnée.
* Un élément **Button** permettant de lancer la suppression des éléments importés de la source sélectionnée.
* Un élément **Button** permettant d’annuler une opération d’importation asynchrone d’éléments multimédias.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml" id="SnippetImportXAML":::

## <a name="set-up-your-code-behind-file"></a>Configurer votre fichier code-behind
Ajoutez des directives *using* afin d’inclure les espaces de noms utilisés par cet exemple qui ne sont pas encore inclus dans le modèle de projet par défaut.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetUsing":::

## <a name="set-up-task-cancellation-for-media-import-operations"></a>Configurer l’annulation des tâches associées aux opérations d’importation des fichiers multimédias

Étant donné que les opérations d’importation de média peuvent prendre beaucoup de temps, elles sont exécutées de façon asynchrone à l’aide de [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_). Déclarez une variable de membre de classe de type [**CancellationTokenSource**](/dotnet/api/system.threading.cancellationtokensource) qui sera utilisée pour annuler une opération en cours si l’utilisateur clique sur le bouton Annuler.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareCts":::

Implémentez un gestionnaire pour le bouton d’annulation. L’exemple figurant plus loin dans cet article est dédié à l’initialisation de l’instance **CancellationTokenSource** au démarrage d’une opération. La valeur de cette dernière est définie sur null à l’issue. Dans le gestionnaire du bouton d’annulation, vérifiez si le jeton est défini sur null et si ce n’est pas le cas, appelez [**Cancel**](/dotnet/api/system.threading.cancellationtokensource.cancel#System_Threading_CancellationTokenSource_Cancel) afin d’annuler l’opération.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetOnCancel":::

## <a name="data-binding-helper-classes"></a>Classes d’assistance de liaison de données

Dans un scénario d’importation standard d’éléments multimédias, vous présentez à l’utilisateur une liste des éléments multimédias disponibles à l’importation. Le nombre de fichiers pouvant s’avérer conséquent, vous pouvez avoir intérêt à présenter une miniature de chaque élément multimédia. Pour cette raison, cet exemple utilise trois classes d’assistance permettant de charger de manière incrémentielle des entrées dans le contrôle ListView, à mesure que l’utilisateur fait défiler la liste.

* Classe **IncrementalLoadingBase** : implémente les objets [**IList**](/dotnet/api/system.collections.ilist), [**ISupportIncrementalLoading**](/uwp/api/windows.ui.xaml.data.isupportincrementalloading) et [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) afin de communiquer le comportement de base de chargement incrémentiel.
* Classe **GeneratorIncrementalLoadingClass** : fournit une implémentation de la classe de base de chargement incrémentiel.
* Classe **ImportableItemWrapper** : un wrapper fin autour de la classe [**PhotoImportItem**](/uwp/api/Windows.Media.Import.PhotoImportItem) afin d’ajouter une propriété [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) pouvant être liée pour chacune des images miniatures associées aux éléments importés.

Ces classes sont fournies dans l' [**exemple MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) et peuvent être ajoutées à votre projet sans modification. Une fois les classes d’assistance ajoutées à votre projet, déclarez une variable de membre de classe de type **GeneratorIncrementalLoadingClass**, qui sera utilisé plus loin dans cet exemple.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetGeneratorIncrementalLoadingClass":::


## <a name="find-available-sources-from-which-media-can-be-imported"></a>Rechercher les sources disponibles à partir desquelles importer les fichiers multimédias

Dans le gestionnaire de clic du bouton Rechercher des sources, appelez la méthode statique [**PhotoImportManager. FindAllSourcesAsync**](/uwp/api/windows.media.import.photoimportmanager.findallsourcesasync) pour démarrer le système de recherche des appareils à partir desquels les médias peuvent être importés. Après avoir attendu l’exécution de l’opération, parcourez chaque objet [**PhotoImportSource**](/uwp/api/Windows.Media.Import.PhotoImportSource) de la liste renvoyée et ajoutez une entrée à l’instance **ComboBox**, en définissant la propriété **Tag** sur l’objet source, afin qu’il puisse être facilement récupéré lorsque l’utilisateur effectue une sélection.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetFindSourcesClick":::

Déclarez une variable de membre de classe dédiée au stockage de la source d’importation sélectionnée de l’utilisateur.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareImportSource":::

Dans le gestionnaire [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) de la source d’importation **ComboBox**, définissez la variable de membre de classe sur la source sélectionnée, puis appelez la méthode d’assistance **FindItems**, décrite plus loin dans cet article. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetSourcesSelectionChanged":::

## <a name="find-items-to-import"></a>Rechercher des éléments à importer

Ajoutez des variables de membre de classe de type [**PhotoImportSession**](/uwp/api/Windows.Media.Import.PhotoImportSession) et [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) à utiliser dans les étapes suivantes.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareImport":::

Dans la méthode **FindItems** , initialisez la variable **CancellationTokenSource** afin qu’elle puisse être utilisée pour annuler l’opération de recherche si nécessaire. Dans un bloc **try**, créez une nouvelle session d’importation en appelant [**CreateImportSession**](/uwp/api/windows.media.import.photoimportsource.createimportsession) sur l’objet [**PhotoImportSource**](/uwp/api/Windows.Media.Import.PhotoImportSource) sélectionné par l’utilisateur. Créez un nouvel objet [**Progress**](/dotnet/api/system.progress-1) afin de fournir un rappel prenant en charge l’affichage de l’avancement de l’opération de recherche. Ensuite, appelez **[FindItemsAsync](/uwp/api/windows.media.import.photoimportsession.finditemsasync)** pour démarrer l’opération de recherche. Fournissez une valeur [**PhotoImportContentTypeFilter**](/uwp/api/Windows.Media.Import.PhotoImportContentTypeFilter) pour spécifier si des photos, des vidéos ou les deux doivent être retournés. Fournissez une valeur [**PhotoImportItemSelectionMode**](/uwp/api/Windows.Media.Import.PhotoImportItemSelectionMode) pour spécifier si tous, aucun, ou seuls les nouveaux éléments multimédias sont retournés avec leur propriété [**IsSelected**](/uwp/api/windows.media.import.photoimportitem.isselected) définie sur true. Cette propriété est liée à une case à cocher pour chaque élément multimédia de notre modèle d’élément ListBox.

**FindItemsAsync** renvoie une valeur [**IAsyncOperationWithProgress**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_). La méthode d’extension [**AsTask**](/dotnet/api/system) est utilisée pour créer une tâche pouvant être attendue ou pouvant être annulée avec le jeton d’annulation. Elle fait état de l’avancement à l’aide de l’objet **Progress** fourni.

En regard de la classe d’assistance de liaison des données, la classe **GeneratorIncrementalLoadingClass** est initialisée. **FindItemsAsync**, lorsqu’il revient d’avoir été attendu, renvoie un objet [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult). Cet objet comporte les informations de statut sur l’opération de recherche, notamment sur sa réussite et le nombre des différents types d’éléments multimédias identifiés. La propriété [**FoundItems**](/uwp/api/windows.media.import.photoimportfinditemsresult.founditems) contient une liste des objets [**PhotoImportItem**](/uwp/api/Windows.Media.Import.PhotoImportItem) représentant les éléments multimédias identifiés. Le constructeur **GeneratorIncrementalLoadingClass** prend en tant qu’arguments le nombre total d’éléments à charger de manière incrémentielle et une fonction qui génère de nouveaux éléments à charger au besoin. Dans ce cas, l’expression lambda fournie génère une nouvelle instance de l’objet **ImportableItemWrapper**, qui enveloppe **PhotoImportItem** et inclut une miniature pour chaque élément. Une fois que la classe de chargement incrémentielle a été initialisée, définissez-la sur la propriété [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) du contrôle **ListView** de l’interface utilisateur. À présent, les éléments multimédias identifiés seront chargés de manière incrémentielle et chargés dans la liste.

Ensuite, les informations de statut de l’opération de recherche sont affichées. Une application standard affiche ces informations à l’utilisateur dans l’interface, mais cet exemple transmet simplement les informations à la console de débogage. Enfin, définissez le jeton d’annulation sur null, dans la mesure où l’opération est terminée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetFindItems":::

## <a name="import-media-items"></a>Importer des éléments multimédias

Avant d’implémenter l’opération d’importation, déclarez un objet [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) dédié au stockage des résultats de l’opération d’importation. Il sera utilisé plus tard pour supprimer les éléments multimédias importés à partir de la source.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareImportResult":::

Avant de démarrer l’opération d’importation d’éléments multimédias, initialisez la variable **CancellationTokenSource** en définissant la valeur du contrôle [**ProgressBar**](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) sur 0.

Si le contrôle **ListView** ne présente aucun élément sélectionné, l’importation ne peut pas avoir lieu. Sinon, initialisez un objet de [**progression**](/dotnet/api/system.progress-1) pour fournir un rappel de progression qui met à jour la valeur du contrôle de barre de progression. Inscrire un gestionnaire pour l’événement [**ItemImported**](/uwp/api/windows.media.import.photoimportfinditemsresult.itemimported) du [**PhotoImportFindItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportFindItemsResult) retourné par l’opération de recherche. Cet événement est déclenché chaque fois qu’un élément est importé et, dans cet exemple, génère le nom de chaque fichier importé sur la console de débogage.

Appelez [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) pour commencer l’opération d’importation. Tout comme pour l’opération de recherche, la méthode d’extension [**AsTask**](/dotnet/api/system) est utilisée pour convertir l’opération retournée en une tâche qui peut être attendue, signale la progression et peut être annulée.

Une fois l’opération d’importation terminée, l’état de l’opération peut être obtenu à partir de l’objet [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) retourné par [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync). Cet exemple affiche les informations de statut sur la console de débogage puis, en définitive, définit le jeton d’annulation sur null.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetImportClick":::

## <a name="delete-imported-items"></a>Supprimer des éléments importés
Pour supprimer les éléments importés de la source à partir de laquelle ils ont été importés, initialisez le jeton d’annulation, de manière à ce que l’opération de suppression puisse être annulée et définissez la valeur de la barre d’avancement sur 0. Assurez-vous que le [**PhotoImportImportItemsResult**](/uwp/api/Windows.Media.Import.PhotoImportImportItemsResult) retourné par [**ImportItemsAsync**](/uwp/api/windows.media.import.photoimportfinditemsresult.importitemsasync) n’est pas null. Si ce n’est pas le cas, créez de nouveau un objet [**Progress**](/dotnet/api/system.progress-1) afin de fournir un rappel de progression associé à l’opération de suppression. Appelez [**DeleteImportedItemsFromSourceAsync**](/uwp/api/windows.media.import.photoimportimportitemsresult.deleteimporteditemsfromsourceasync) pour commencer à supprimer les éléments importés. Utilisez **AsTask** pour convertir le résultat en une tâche pouvant être attendue à l’aide de fonctionnalités de progression et d’annulation. Après l’attente, l’objet [**PhotoImportDeleteImportedItemsFromSourceResult**](/uwp/api/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) retourné peut être utilisé pour obtenir et afficher des informations d’État sur l’opération de suppression.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/PhotoImport_Win10/cs/MainPage.xaml.cs" id="SnippetDeleteClick":::









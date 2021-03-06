---
description: Résolvez les problèmes que vous pouvez rencontrer lors du Portage Windows Phone Silverlight vers UWP.
title: Résolution des problèmes de portage de Windows Phone Silverlight vers UWP
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b1e73dcb74ae95729cb5c07db469a1ef1a77e35
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133002"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>Résolution des problèmes de portage de Windows Phone Silverlight vers UWP


Rubrique précédente : [Portage du projet](wpsl-to-uwp-porting-to-a-uwp-project.md).

Nous vous recommandons vivement de lire ce guide de portage jusqu’à la fin, mais nous comprenons également que vous soyez impatient d’avancer et de passer à l’étape de développement et d’exécution de votre projet. À cette fin, vous pouvez avancer provisoirement en commentant ou en remplaçant du code non essentiel, pour revenir ensuite afin de combler cette lacune ultérieurement. Le tableau de résolution des problèmes et des solutions de cette rubrique peuvent vous être utiles à ce stade, même s’il ne se substitue pas à la lecture des rubriques suivantes. Vous pouvez toujours revenir au tableau lorsque vous avancez dans les rubriques ultérieures.

## <a name="tracking-down-issues"></a>Suivi des problèmes

Les exceptions d’analyse XAML peuvent être difficiles à diagnostiquer, en particulier si l’exception ne présente aucun message d’erreur explicite. Assurez-vous que le débogueur est configuré pour intercepter les exceptions de première chance (pour essayer d’intercepter l’exception d’analyse le plus tôt possible). Vous pourrez peut-être inspecter la variable d’exception dans le débogueur pour déterminer si la valeur HRESULT ou le message comportent des informations utiles. Vérifiez également la fenêtre de sortie de Visual Studio pour voir si elle contient des messages d’erreur de l’analyseur XAML.

Si votre application s’arrête et que tout ce vous savez, c’est qu’une exception non gérée a été levée pendant l’analyse du balisage XAML, ce peut être le résultat d’une référence à une ressource manquante (c’est-à-dire à une ressource dont la clé existe pour les applications Silverlight pour Windows Phone, mais non pour les applications Windows 10, par exemple certaines clés de style **TextBlock** système). Il peut s’agir d’une exception levée à l’intérieur d’un **UserControl**, d’un contrôle personnalisé ou d’un panneau de disposition personnalisé.

En dernier recours, vous pouvez effectuer un fractionnement binaire. Supprimez environ la moitié du balisage d’une page et réexécutez l’application. Vous saurez ensuite si l’erreur se situe à l’intérieur de la moitié que vous avez supprimée (que vous devez maintenant restaurer dans tous les cas) ou dans la moitié que vous n’avez *pas* supprimée. Répétez ce processus en fractionnant la moitié qui contient l’erreur et ainsi de suite jusqu’à ce que vous ayez ciblé le problème.

## <a name="targetplatformversion"></a>TargetPlatformVersion

Cette section explique comment procéder si, lorsque vous ouvrez un projet Windows 10 dans Visual Studio, vous voyez apparaître le message suivant : « Mise à jour de Visual Studio requise. Un ou plusieurs projets nécessitent un Kit de développement de plate-forme &lt;version&gt; qui n’est pas installé ou qui est inclus comme élément d’une prochaine mise à jour de Visual Studio. »

-   Commencez par déterminer le numéro de version du Kit de développement logiciel (SDK) pour Windows 10 que vous avez installé. Accédez à **C : \\ Program Files (x86) les \\ Kits \\ Windows 10 \\ incluent \\ &lt; versionfoldername &gt; ** et indiquent * &lt; Versionfoldername &gt; *, qui sera en notation quadruple, "major. minor. Build. Revision".
-   Ouvrez le fichier de votre projet à des fins d’édition, puis recherchez les éléments `TargetPlatformVersion` et `TargetPlatformMinVersion`. Modifiez-les pour qu’il ressemble à ce qui suit, en remplaçant * &lt; versionfoldername &gt; * par le numéro de version à quatre notations que vous avez trouvé sur le disque :

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Résolution des problèmes et solutions

Les informations sur les solutions contenues dans le tableau sont destinées à vous donner suffisamment d’informations pour débloquer votre situation. Vous trouverez d’autres détails sur chacun de ces problèmes en parcourant les rubriques suivantes.

| Symptôme | Solution |
|---------|--------|
| L’analyseur ou le compilateur XAML indique l’erreur « _Le nom "&lt;nom de type&gt;" n’existe pas dans l’espace de noms [...]._ ». | Si &lt;nom de type&gt; est un type personnalisé, dans vos déclarations de préfixe d’espace de noms dans le balisage XAML, remplacez « clr-namespace » par « using » et supprimez tous les jetons d’assembly. Pour les types de plateforme, cela signifie que le type ne s’applique pas à la plateforme Windows universelle (UWP). En conséquence, recherchez l’équivalent et mettez à jour votre balisage. Les exemples que vous pouvez rencontrer immédiatement sont `phone:PhoneApplicationPage` et `shell:SystemTray.IsVisible`. | 
| L’analyseur ou le compilateur XAML indique l’erreur « _Le membre &lt;nom de membre&gt; n’est pas reconnu ou n’est pas accessible._» ou « _La propriété &lt;nom de propriété&gt; est introuvable dans le type [...]._». | Ces erreurs commencent à s’afficher une fois que vous avez porté certains noms de type, par exemple l’élément **Page** racine. La propriété ou le membre ne s’appliquent pas à UWP. Recherchez donc l’équivalent et mettez à jour votre balisage. Les exemples que vous pouvez rencontrer immédiatement sont `SupportedOrientations` et `Orientation`. |
| L’analyseur ou le compilateur XAML indique l’erreur « _La propriété pouvant être attachée [...] est introuvable._  » ou « _Membre pouvant être attaché inconnu [...]. _». | Cette erreur est probablement provoquée par le type plutôt que par la propriété jointe ; dans ce cas, vous avez déjà reçu une erreur pour le type, qui une fois résolue fait disparaître cette erreur. Les exemples que vous pouvez rencontrer immédiatement sont `phone:PhoneApplicationPage.Resources` et `phone:PhoneApplicationPage.DataContext`. | 
|L’analyseur ou le compilateur XAML (ou une exception runtime) indique l’erreur suivante : « _Impossible de résoudre la ressource "&lt;clé_ressource&gt;"._ ». | La clé de ressource ne s’applique pas aux applications de plateforme Windows universelle (UWP). Recherchez la ressource équivalente appropriée et mettez votre balisage à jour. Les exemples que vous pouvez rencontrer immédiatement sont des clés de style **TextBlock** système, comme `PhoneTextNormalStyle`. |
| Le compilateur C# indique l’erreur « _Le nom de type ou d’espace de noms "&lt;nom&gt;" est introuvable [...]._ » ou « _Le nom de type ou d’espace de noms "&lt;nom&gt;" n’existe pas dans l’espace de noms [...]._ » ou « _Le nom de type ou de l’espace de noms "&lt;nom&gt;" n’existe pas dans le contexte actuel._ ». | Cela signifie probablement que le compilateur ne connaît pas encore l’espace de noms UWP correct pour un type. Utilisez la commande **Résoudre** de Visual Studio pour résoudre ce problème. <br/>Si l’API ne figure pas dans l’ensemble d’API désigné sous le terme de famille d’appareils universelle (en d’autres termes, l’API est implémentée dans un SDK d’extension), utilisez les [SDK d’extension](wpsl-to-uwp-porting-to-a-uwp-project.md).<br/>Il existe peut-être d’autres cas dans lesquels le portage se révèle moins simple. Les exemples que vous pouvez rencontrer immédiatement sont `DesignerProperties` et `BitmapImage`. | 
|Lorsqu’elle est exécutée sur l’appareil, l’application se termine ou, une fois lancée à partir de Visual Studio, vous voyez l’erreur «Impossible d’activer Windows Runtime 8. x application [...]. La demande d’activation a échoué avec l’erreur « Windows n’a pas pu communiquer avec l’application cible. Cela indique généralement que le processus de l’application cible a été abandonné. […]”. | Le problème peut être lié au code impératif en cours d’exécution dans vos propres pages ou dans les propriétés de liaison (ou d’autres types) lors de l’initialisation. Il peut également se produire lors de l’analyse du fichier XAML sur le point d’être affiché lorsque l’application est arrêtée (en cas de lancement dans Visual Studio, qui sera la page de démarrage). Recherchez les clés de ressource non valides et/ou essayez certaines des instructions de la section « [suivi des problèmes de suivi](#tracking-down-issues) » dans cette rubrique.|
| _XamlCompiler Error WMC0055 : impossible d’assigner la valeur texte' &lt; Your Stream Geometry &gt; 'à la propriété’clip’de type’RectangleGeometry'_ | Dans UWP, le type de l’application UWP [Microsoft DirectX](/windows/desktop/directx) et XAML en C++. |
| _Erreur XamlCompiler WMC0001 : type inconnu « RadialGradientBrush » dans l’espace de noms XML [...]._ | UWP ne comporte pas le type **RadialGradientBrush**. Supprimez le type **RadialGradientBrush** dans le balisage et utilisez un autre type d’application UWP [Microsoft DirectX](/windows/desktop/directx) et XAML en C++. |
| _XamlCompiler Error WMC0011 : membre inconnu’OpacityMask’sur l’élément' &lt; UIElement type &gt; '_ | L’application UWP [Microsoft DirectX](/windows/desktop/directx) et XAML en C++. |
| _Une exception de première chance de type’System. Runtime. InteropServices. COMException’s’est produite dans SYSTEM.NI.DLL. Informations supplémentaires : l’application a appelé une interface qui a été maintenue en ordre pour un thread différent. (Exception de HRESULT : 0x8001010E (RPC_E_WRONG_THREAD))._ | Le travail que vous effectuez doit être exécuté sur le thread d’interface utilisateur. Appelez la méthode [**CoreWindow.GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread). |
| Une animation est en cours d’exécution, mais elle est sans effet sur sa propriété cible. | Transformez l’animation en animation indépendante ou définissez le paramètre `EnableDependentAnimation="True"` dans sa syntaxe. Voir [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| En ouvrant un projet Windows 10 dans Visual Studio, vous voyez apparaître le message suivant : « Mise à jour de Visual Studio requise. Un ou plusieurs projets nécessitent un Kit de développement de plate-forme &lt;version&gt; qui n’est pas installé ou qui est inclus comme élément d’une prochaine mise à jour de Visual Studio. » | Voir la section [TargetPlatformVersion](#targetplatformversion) dans cette rubrique. |
| Une exception System.InvalidCastException est levée lorsque le paramètre InitializeComponent est appelé dans un fichier xaml.cs. | Cela peut se produire lorsque vous disposez de plusieurs fichiers xaml (dont l’un est qualifié pour MRT, au minimum) qui partagent le même fichier xaml.cs et que les éléments présentent des attributs x: Name qui ne correspondent pas d’un fichier xaml à l’autre. Essayez d’ajouter le même nom à ces éléments identiques dans les deux fichiers xaml, ou omettez-les complètement. | 

Rubrique suivante : [Portage du balisage XAML et de la couche interface utilisateur](wpsl-to-uwp-porting-xaml-and-ui.md).
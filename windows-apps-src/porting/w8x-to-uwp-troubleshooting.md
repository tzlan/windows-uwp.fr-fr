---
description: Résoudre les problèmes que vous pouvez rencontrer lors du Portage de Windows Runtime 8. x vers UWP
title: Résolution des problèmes de portage de Windows Runtime 8.x vers UWP
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0e915aec7f37600ce821f1a097a8aa5ac7ca76b
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133102"
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>Résolution des problèmes de portage de Windows Runtime 8.x vers UWP


Rubrique précédente : [Portage du projet](w8x-to-uwp-porting-to-a-uwp-project.md).

Nous vous recommandons vivement de lire ce guide de portage jusqu’à la fin, mais nous comprenons également que vous soyez impatient d’avancer et de passer à l’étape de développement et d’exécution de votre projet. À cette fin, vous pouvez avancer provisoirement en commentant ou en remplaçant du code non essentiel, pour revenir ensuite afin de combler cette lacune ultérieurement. Le tableau de résolution des problèmes et des solutions de cette rubrique peuvent vous être utiles à ce stade, même s’il ne se substitue pas à la lecture des rubriques suivantes. Vous pouvez toujours revenir au tableau lorsque vous avancez dans les rubriques ultérieures.

## <a name="tracking-down-issues"></a>Suivi des problèmes

Les exceptions d’analyse XAML peuvent être difficiles à diagnostiquer, en particulier si l’exception ne présente aucun message d’erreur explicite. Assurez-vous que le débogueur est configuré pour intercepter les exceptions de première chance (pour essayer d’intercepter l’exception d’analyse le plus tôt possible). Vous pourrez peut-être inspecter la variable d’exception dans le débogueur pour déterminer si la valeur HRESULT ou le message comportent des informations utiles. Vérifiez également la fenêtre de sortie de Visual Studio pour voir si elle contient des messages d’erreur de l’analyseur XAML.

Si votre application s’arrête et que tout ce vous savez, c’est qu’une exception non gérée a été levée pendant l’analyse du balisage XAML, ce peut être le résultat d’une référence à une ressource manquante (c’est-à-dire à une ressource dont la clé existe pour les applications 8.1 universelles, mais non pour les applications Windows 10, par exemple certaines clés système de style **TextBlock**). Ou il peut s’agir d’une exception levée à l’intérieur d’un **UserControl**, d’un contrôle personnalisé ou d’un panneau de disposition personnalisé.

En dernier recours, vous pouvez effectuer un fractionnement binaire. Supprimez environ la moitié du balisage d’une page et réexécutez l’application. Vous saurez ensuite si l’erreur se situe à l’intérieur de la moitié que vous avez supprimée (que vous devez maintenant restaurer dans tous les cas) ou dans la moitié que vous n’avez *pas* supprimée. Répétez ce processus en fractionnant la moitié qui contient l’erreur et ainsi de suite jusqu’à ce que vous ayez ciblé le problème.

## <a name="targetplatformversion"></a>TargetPlatformVersion

Cette section explique comment procéder si, lorsque vous ouvrez un projet Windows 10 dans Visual Studio, vous voyez apparaître le message suivant : « Mise à jour de Visual Studio requise. Un ou plusieurs projets nécessitent un Kit de développement de plate-forme \<version\> qui n’est pas installé ou qui est inclus comme élément d’une prochaine mise à jour de Visual Studio. »

-   Commencez par déterminer le numéro de version du Kit de développement logiciel (SDK) pour Windows 10 que vous avez installé. Accédez à **C : \\ Program Files (x86) les \\ Kits \\ Windows 10 \\ incluent \\ \<versionfoldername\> ** et notez *\<versionfoldername\>* , qui sera en notation quadruple, « major. minor. Build. revision ».
-   Ouvrez le fichier de votre projet à des fins d’édition, puis recherchez les éléments `TargetPlatformVersion` et `TargetPlatformMinVersion`. Modifiez-les pour qu’ils ressemblent à ce qui suit, en remplaçant *\<versionfoldername\>* par le numéro de version à quatre notations que vous avez trouvé sur le disque :

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Résolution des problèmes et solutions

Les informations sur les solutions contenues dans le tableau sont destinées à vous donner suffisamment d’informations pour débloquer votre situation. Vous trouverez d’autres détails sur chacun de ces problèmes en parcourant les rubriques suivantes.

| Symptôme | Solution |
|---------|--------|
| En ouvrant un projet Windows 10 dans Visual Studio, vous voyez apparaître le message suivant : « Mise à jour de Visual Studio requise. Un ou plusieurs projets nécessitent un Kit de développement de plate-forme &lt;version&gt; qui n’est pas installé ou qui est inclus comme élément d’une prochaine mise à jour de Visual Studio. » | Voir la section [TargetPlatformVersion](#targetplatformversion) dans cette rubrique. |
| Une exception System.InvalidCastException est levée lorsque le paramètre InitializeComponent est appelé dans un fichier xaml.cs.| Cela peut se produire lorsque vous disposez de plusieurs fichiers xaml (dont l’un est qualifié pour MRT, au minimum) qui partagent le même fichier xaml.cs et que les éléments présentent des attributs x: Name qui ne correspondent pas d’un fichier xaml à l’autre. Essayez d’ajouter le même nom à ces éléments identiques dans les deux fichiers xaml, ou omettez-les complètement. |
| Lorsqu’elle est exécutée sur l’appareil, l’application se termine ou, une fois lancée à partir de Visual Studio, vous voyez l’erreur «Impossible d’activer Windows Runtime application 8. x... \[ \] . La demande d’activation a échoué avec l’erreur « Windows n’a pas pu communiquer avec l’application cible. Cela indique généralement que le processus de l’application cible a été abandonné. \[…\]”. | Le problème peut être lié au code impératif en cours d’exécution dans vos propres pages ou dans les propriétés de liaison (ou d’autres types) lors de l’initialisation. Il peut également se produire lors de l’analyse du fichier XAML sur le point d’être affiché lorsque l’application est arrêtée (en cas de lancement dans Visual Studio, il s’agira de la page de démarrage). Recherchez les clés de ressources non valides ou essayez de suivre certaines des recommandations de la section « Suivi des problèmes » dans cette rubrique.|
| L’analyseur ou le compilateur XAML (ou une exception runtime) indiquent l’erreur suivante : « *Impossible de résoudre la ressource « \<resourcekey\> ».* ». | La clé de ressource ne s’applique pas aux applications de plateforme Windows universelle (UWP) (c’est le cas pour certaines ressources Windows Phone, par exemple). Recherchez la ressource équivalente appropriée et mettez votre balisage à jour. Les exemples que vous pouvez rencontrer immédiatement correspondent à des clés système, comme `PhoneAccentBrush`. |
| Le compilateur C# génère l’erreur «*le type ou le nom d’espace de noms' \<name\> 'est introuvable \[ . \] .*. » ou «*le nom du type ou de l’espace de noms' \<name\> 'n’existe pas dans l’espace de noms \[ ... \] *» ou «*le nom du type ou de l’espace de noms' \<name\> 'n’existe pas dans le contexte actuel*». | Cela signifie probablement que le type est implémenté dans un SDK d’extension (même si dans certains cas, la solution n’est pas aussi simple). Utilisez le contenu de référence des [API Windows](/uwp/) pour déterminer le SDK d’extension qui implémente l’API, puis la commande **Ajouter** > **Référence** de Visual Studio pour ajouter une référence à ce SDK dans votre projet. Si votre application cible le jeu d’API connu sous le nom de famille de périphériques universels, il est vital que vous utilisiez la classe [**ApiInformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) pour tester la présence du kit de développement logiciel (SDK) d’extension avant de les appeler (c’est ce que l’on appelle un code adaptatif). S’il existe une API universelle, elle sera toujours préférable à une API figurant dans un SDK d’extension. Pour plus d’informations, voir [Kits de développement logiciel (SDK) d’extension](w8x-to-uwp-porting-to-a-uwp-project.md). |

Rubrique suivante : [Portage du balisage XAML et de la couche interface utilisateur](w8x-to-uwp-porting-xaml-and-ui.md).
---
description: Cet article explique comment créer un composant Windows Runtime implémentant l’interface IBasicVideoEffect pour créer des effets personnalisés de flux vidéo.
MS-HAID: dev\_audio\_vid\_camera.custom\_video\_effects
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Effets vidéo personnalisés
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 40a6bd32-a756-400f-ba34-2c5f507262c0
ms.localizationpriority: medium
ms.openlocfilehash: 119d444f073c4f668dcdc63fc118ed408eca8e32
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030492"
---
# <a name="custom-video-effects"></a>Effets vidéo personnalisés




Cet article explique comment créer un composant Windows Runtime implémentant l’interface [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) pour créer des effets personnalisés de flux vidéo. Vous pouvez utiliser les effets personnalisés avec plusieurs API Windows Runtime différentes, notamment [MediaCapture](/uwp/api/Windows.Media.Capture.MediaCapture), qui fournit un accès à la caméra d’un appareil, et [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition), qui vous permet de créer des compositions complexes à partir de clips multimédias.

## <a name="add-a-custom-effect-to-your-app"></a>Ajouter un effet personnalisé à votre application


Un effet vidéo personnalisé est défini dans une classe qui implémente l’interface [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect). Cette classe ne peut pas être incluse directement dans le projet de votre application. À la place, vous devez utiliser un composant Windows Runtime pour héberger votre classe d’effet vidéo.

**Ajouter un composant Windows Runtime pour votre effet vidéo**

1.  Dans Microsoft Visual Studio, quand votre solution est ouverte, accédez au menu **Fichier** , sélectionnez **Ajouter-&gt;Nouveau projet.**
2.  Sélectionnez le type de projet **Composant Windows Runtime (Windows universel)** .
3.  Pour cet exemple, nommez le projet *VideoEffectComponent* . Ce nom sera référencé dans le code ultérieurement.
4.  Cliquez sur **OK** .
5.  Le modèle de projet crée une classe appelée Class1.cs. Dans **l’Explorateur de solutions** , cliquez avec le bouton droit sur l’icône de Class1.cs et sélectionnez **Renommer** .
6.  Renommez le fichier *ExampleVideoEffect.cs* . Visual Studio affiche une invite vous demandant si vous voulez mettre à jour toutes les références sous le nouveau nom. Cliquez sur **Oui** .
7.  Ouvrez **ExampleVideoEffect.cs** et mettez à jour la définition de classe pour implémenter l’interface [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetImplementIBasicVideoEffect":::


Vous devez inclure les espaces de noms suivants dans votre fichier de classe effet afin d’accéder à tous les types utilisés dans les exemples de cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetEffectUsing":::


## <a name="implement-the-ibasicvideoeffect-interface-using-software-processing"></a>Implémentez l’interface IBasicVideoEffect à l’aide du traitement logiciel


Votre effet vidéo doit implémenter toutes les méthodes et propriétés de l’interface [**IBasicVideoEffect**](/uwp/api/Windows.Media.Effects.IBasicVideoEffect) . Cette section vous explique la procédure d’implémentation simple de cette interface à l’aide d’un traitement logiciel.

### <a name="close-method"></a>Close, méthode

Le système appelle la méthode [**Fermer**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) sur votre classe lorsque l’effet doit être arrêté. Vous devez utiliser cette méthode pour supprimer les ressources que vous avez créées. L’argument de la méthode est un [**MediaEffectClosedReason**](/uwp/api/Windows.Media.Effects.MediaEffectClosedReason) qui vous permet de savoir si l’effet a été fermé normalement, si une erreur s’est produite ou si l’effet ne prend pas en charge le format d’encodage requis.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetClose":::


### <a name="discardqueuedframes-method"></a>Méthode DiscardQueuedFrames

La méthode [**DiscardQueuedFrames**](/uwp/api/windows.media.effects.ibasicvideoeffect.discardqueuedframes) est appelée lorsque votre effet doit être réinitialisé. Dans ce cas, le scénario courant est que l’effet stocke les trames précédemment traitées pour les utiliser dans le traitement de la trame active. Quand cette méthode est appelée, vous devez supprimer l’ensemble des trames précédentes que vous avez enregistrées. Cette méthode peut être utilisée pour réinitialiser un état associé aux trames précédentes, pas seulement les trames vidéo cumulées.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetDiscardQueuedFrames":::



### <a name="isreadonly-property"></a>IsReadOnly, propriété

La propriété [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) permet au système de savoir si votre effet écrit dans la sortie de l’effet. Si votre application ne modifie pas les trames vidéo (par exemple, un effet qui effectue seulement une analyse des trames vidéo), vous devez définir cette propriété sur true. Ainsi, le système copie efficacement à votre place l’entrée de trame dans la sortie de trame.

> [!TIP]
> Quand la propriété [**IsReadOnly**](/uwp/api/windows.media.effects.ibasicvideoeffect.isreadonly) est définie sur true, le système copie la trame d’entrée sur la trame de sortie avant l’appel de [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe). Le fait de définir la propriété **IsReadOnly** sur true ne vous empêche pas d’écrire sur les trames de sortie de l’effet dans **ProcessFrame** .


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetIsReadOnly":::

### <a name="setencodingproperties-method"></a>Méthode SetEncodingProperties

Le système appelle [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) à votre effet pour vous indiquer les propriétés d’encodage du flux vidéo sur lequel l’effet s’applique. Cette méthode fournit également une référence à l’appareil Direct3D utilisé pour le rendu matériel. L’utilisation de cet appareil est expliquée dans l’exemple de traitement matériel plus loin dans cet article.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetEncodingProperties":::


### <a name="supportedencodingproperties-property"></a>Propriété SupportedEncodingProperties

Le système vérifie la propriété [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties) pour déterminer quelles propriétés d’encodage sont prises en charge par votre effet. Notez que si le consommateur de votre effet ne peut pas encoder une vidéo en utilisant les propriétés que vous spécifiez, il appelle [**Close**](/uwp/api/windows.media.effects.ibasicvideoeffect.close) sur votre effet et supprime votre effet du pipeline vidéo.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedEncodingProperties":::


> [!NOTE] 
> Si vous renvoyez une liste vide d’objets [**VideoEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingProperties) à partir de **SupportedEncodingProperties** , le système utilise par défaut le codage ARGB32.

 

### <a name="supportedmemorytypes-property"></a>Propriété SupportedMemoryTypes

Le système vérifie que la propriété [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes) pour déterminer si l’effet va accéder aux trames vidéo dans la mémoire du logiciel ou dans la mémoire du matériel (GPU). Si vous retournez [**MediaMemoryTypes. CPU**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes), les trames d’entrée et de sortie qui contiennent des données d’image dans les objets [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) sont passées. Si vous renvoyez **MediaMemoryTypes.Gpu** , l’effet sera transmis aux trames en entrée et en sortie qui contiennent des données d’image dans les objets [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSupportedMemoryTypes":::


> [!NOTE]
> Si vous spécifiez [**MediaMemoryTypes.GpuAndCpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes), le système utilise la mémoire du GPU ou du système, selon laquelle est la plus performante pour le pipeline. Quand vous utilisez cette valeur, vous devez vérifier la méthode [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) pour voir si le [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) ou le [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) transmis dans la méthode contient des données, puis traiter la trame en fonction.

 

### <a name="timeindependent-property"></a>Propriété TimeIndependent

La propriété [**TimeIndependent**](/uwp/api/windows.media.effects.ibasicvideoeffect.timeindependent) permet au système de savoir si votre effet n’exige pas un minutage uniforme. Lorsqu’elle est définie sur true, le système peut utiliser les optimisations qui améliorent la performance de l’effet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetTimeIndependent":::

### <a name="setproperties-method"></a>Méthode SetProperties

La méthode [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) permet à l’application qui utilise votre effet d’ajuster les paramètres d’effet. Les propriétés sont transmises sous la forme d’une carte de [**IPropertySet**](/uwp/api/Windows.Foundation.Collections.IPropertySet) de noms et de valeurs de propriétés.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetSetProperties":::


Cet exemple simple estompe les pixels dans chaque trame vidéo en fonction d’une valeur spécifiée. Une propriété est déclarée et TryGetValue est utilisé pour obtenir la valeur définie par l’application d’appel. Si aucune valeur n’a été définie, la valeur par défaut .5 est utilisée.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetFadeValue":::


### <a name="processframe-method"></a>Méthode ProcessFrame

C’est dans la méthode [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) que l’effet modifie les données d’image de la vidéo. La méthode est appelée une fois par trame et un objet [**ProcessVideoFrameContext**](/uwp/api/Windows.Media.Effects.ProcessVideoFrameContext) lui est transmis. Cet objet contient un objet [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) en entrée qui contient la trame entrante à traiter et un objet **VideoFrame** en sortie sur lequel vous écrivez des données d’image qui seront transmises dans le reste du pipeline vidéo. Chacun de ces objets **VideoFrame** comporte une propriété [**SoftwareBitmap**](/uwp/api/windows.media.videoframe.softwarebitmap) et une propriété [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface), mais lesquels d’entre eux peuvent être utilisés est déterminé par la valeur renvoyée à partir de la propriété [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes).

Cet exemple montre une implémentation simple de la méthode **ProcessFrame** à l’aide du traitement logiciel. Pour plus d’informations sur l’utilisation des objets [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap), voir [Acquisition d’images](imaging.md). Un exemple d’implémentation de **ProcessFrame** à l’aide du traitement logiciel est illustré plus loin dans cet article.

L’accès à la mémoire tampon de données d’un **SoftwareBitmap** nécessite l’interopérabilité COM. Vous devez donc inclure l’espace de noms **System.Runtime.InteropServices** dans votre fichier de classe effet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMUsing":::


Ajoutez le code suivant à l’intérieur de l’espace de noms de l’effet pour importer l’interface et accéder à la mémoire tampon d’image.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetCOMImport":::


> [!NOTE]
> Dans la mesure où cette technique accède à une mémoire tampon d’image native non gérée, vous devez configurer votre projet pour autoriser du code unsafe.
> 1.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet VideoEffectComponent et sélectionnez **Propriétés** .
> 2.  Sélectionnez l’onglet **Build** .
> 3.  Activez la case à cocher **autoriser le code unsafe** .

 

Vous pouvez maintenant ajouter l’implémentation de la méthode **ProcessFrame** . Tout d’abord, cette méthode obtient un objet [**BitmapBuffer**](/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) à partir des bitmaps logiciels en entrée et en sortie. Notez que la trame en sortie est ouverte pour l’écriture, et que la trame en entrée l’est pour la lecture. Ensuite, un [**IMemoryBufferReference**](/uwp/api/Windows.Foundation.IMemoryBufferReference) est obtenu pour chaque tampon en appelant [**CreateReference**](/uwp/api/windows.graphics.imaging.bitmapbuffer.createreference). Ensuite, le tampon de données réel est obtenu en transtypant les objets **IMemoryBufferReference** en tant qu’interface d’interopérabilité COM définie ci-dessus, **IMemoryByteAccess** , puis en appelant **GetBuffer** .

Maintenant que les tampons de données ont été obtenus, vous pouvez lire à partir du tampon en entrée et écrire sur le tampon en sortie. La disposition de la mémoire tampon est obtenue en appelant [**GetPlaneDescription**](/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription), qui fournit des informations sur la largeur, la Stride et le décalage initial de la mémoire tampon. Les bits par pixel sont déterminés par les propriétés d’encodage définies précédemment à l’aide de la méthode [**SetEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.setencodingproperties) . Les informations sur le format du tampon sont utilisées pour trouver l’index dans le tampon pour chaque pixel. La valeur du pixel du tampon source est copiée dans le tampon cible. Les valeurs de couleur sont multipliées par la propriété FadeValue définie pour cet effet afin de les diminuer du montant spécifié.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffect.cs" id="SnippetProcessFrameSoftwareBitmap":::


## <a name="implement-the-ibasicvideoeffect-interface-using-hardware-processing"></a>Implémenter l’interface IBasicVideoEffect à l’aide du traitement matériel


La création d’un effet vidéo personnalisé à l’aide du traitement matériel (GPU) est presque identique à l’utilisation du traitement logiciel tel que décrit ci-dessus. Cette section vous explique les différences dans un effet qui utilise le traitement matériel. Cet exemple utilise l’API Windows Runtime Win2D. Pour plus d’informations sur l’utilisation de Win2D, voir la [documentation Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm).

Utilisez les étapes suivantes pour ajouter le package NuGet Win2D au projet que vous avez créé comme décrit dans la section **Ajouter un effet personnalisé à votre application** au début de cet article.

**Pour ajouter le package NuGet Win2D à votre projet d’effet**

1.  Dans l’ **Explorateur de solutions** , cliquez avec le bouton droit sur le projet **VideoEffectComponent** et sélectionnez **Gérer les packages NuGet** .
2.  En haut de la fenêtre, sélectionnez l’onglet **Explorer** .
3.  Dans la zone de recherche, entrez **Win2D** .
4.  Sélectionnez **Win2D.uwp** , puis **Installer** dans le volet droit.
5.  La boîte de dialogue **Examiner les modifications** vous indique le package à installer. Cliquez sur **OK** .
6.  Acceptez la licence de package.

Outre les espaces de noms inclus dans l’installation de base du projet, vous devez inclure les espaces de noms suivants fournis par Win2D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetUsingWin2D":::


Dans la mesure où cet effet utilisera la mémoire GPU pour les opérations sur les données d’image, vous devez renvoyer [**MediaMemoryTypes.Gpu**](/uwp/api/Windows.Media.Effects.MediaMemoryTypes) à partir de la propriété [**SupportedMemoryTypes**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedmemorytypes).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedMemoryTypesWin2D":::


Définissez les propriétés de codage que votre effet prend en charge avec la propriété [**SupportedEncodingProperties**](/uwp/api/windows.media.effects.ibasicvideoeffect.supportedencodingproperties). Quand vous utilisez Win2D, vous devez utiliser le codage ARGB32.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSupportedEncodingPropertiesWin2D":::


Utilisez la méthode [**SetEncodingProperties**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) pour créer un nouvel objet **CanvasDevice** Win2D à partir du [**IDirect3DDevice**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DDevice) transmis dans la méthode.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetEncodingPropertiesWin2D":::


L’implémentation de [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties) est identique à l’exemple de traitement logiciel précédent. Cet exemple utilise une propriété **BlurAmount** pour configurer un effet de flou Win2D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetSetPropertiesWin2D":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetBlurAmountWin2D":::


La dernière étape consiste à implémenter la méthode [**ProcessFrame**](/uwp/api/windows.media.effects.ibasicvideoeffect.processframe) qui traite réellement les données de l’image.

À l’aide d’API Win2D, un **CanvasBitmap** est créé à partir de la propriété [**Direct3DSurface**](/uwp/api/windows.media.videoframe.direct3dsurface) de la trame en entrée. Un **CanvasRenderTarget** est créé à partir du **Direct3DSurface** de la trame en sortie et un **CanvasDrawingSession** est créé à partir de cette cible de rendu. Un nouveau **GaussianBlurEffect** Win2D est initialisé à l’aide de la propriété **BlurAmount** que l’effet expose via [**SetProperties**](/uwp/api/windows.media.imediaextension.setproperties). Enfin, la méthode **CanvasDrawingSession.DrawImage** est appelée pour dessiner le bitmap en entrée sur la cible de rendu à l’aide de l’effet de flou.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffectComponent/ExampleVideoEffectWin2D.cs" id="SnippetProcessFrameWin2D":::


## <a name="adding-your-custom-effect-to-your-app"></a>Ajout d’un effet personnalisé à votre application


Pour utiliser votre effet vidéo dans votre application, vous devez ajouter une référence au projet d’effet à votre application.

1.  Dans l’Explorateur de solutions, sous votre projet d’application, cliquez avec le bouton droit sur **Références** , puis sélectionnez **Ajouter une référence** .
2.  Développez l’onglet **Projets** , sélectionnez **Solution** , puis cochez la case du nom de votre projet d’effet. Dans cet exemple, le nom est *VideoEffectComponent* .
3.  Cliquez sur **OK** .

### <a name="add-your-custom-effect-to-a-camera-video-stream"></a>Ajouter un effet personnalisé à un flux vidéo de caméra

Vous pouvez configurer un flux d’aperçu simple à partir de la caméra en suivant les étapes décrites dans l’article [Accès à l’aperçu simple de l’appareil photo](simple-camera-preview-access.md). Suivez ces étapes pour obtenir un objet [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) initialisé qui est utilisé pour accéder au flux vidéo de l’appareil photo.

Pour ajouter votre effet vidéo personnalisé à un flux de caméra, créez d’abord un objet [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition), en transmettant l’espace de noms et le nom de classe de votre effet. Appelez ensuite la méthode [**AddVideoEffect**](/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) de l’objet **MediaCapture** pour ajouter l’effet au flux spécifié. Cet exemple utilise la valeur [**MediaStreamType.VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) pour indiquer que l’effet doit être ajouté au flux d’aperçu. Si votre application prend en charge la capture vidéo, vous pouvez également utiliser **MediaStreamType.VideoRecord** pour ajouter l’effet au flux de capture. **AddVideoEffect** renvoie un objet [**IMediaExtension**](/uwp/api/Windows.Media.IMediaExtension) représentant votre effet personnalisé. Vous pouvez utiliser la méthode SetProperties pour définir la configuration de votre effet.

Une fois l’effet ajouté, [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync) est appelé pour démarrer le flux d’aperçu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddVideoEffectAsync":::



### <a name="add-your-custom-effect-to-a-clip-in-a-mediacomposition"></a>Ajouter un effet personnalisé à un clip dans une composition multimédia

Pour obtenir des instructions générales sur la création des compositions multimédias à partir de clips vidéo, voir [Compositions multimédias et modification](media-compositions-and-editing.md). L’extrait de code suivant illustre la création d’une composition multimédia simple qui utilise un effet vidéo personnalisé. Un objet [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) est créé en appelant [**CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync), en transmettant un fichier vidéo sélectionné par l’utilisateur avec un [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) et le clip est ajouté à un nouveau [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition). Ensuite, un objet [**VideoEffectDefinition**](/uwp/api/Windows.Media.Effects.VideoEffectDefinition) est créé, en transmettant l’espace de noms et le nom de classe de votre effet au constructeur. Enfin, la définition de l’effet est ajoutée à la collection [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) de l’objet **MediaClip** .


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/VideoEffect_Win10/cs/VideoEffect_Win10/MainPage.xaml.cs" id="SnippetAddEffectToComposition":::


## <a name="related-topics"></a>Rubriques connexes
* [Accès à l’aperçu simple de l’appareil photo](simple-camera-preview-access.md)
* [Compositions multimédias et modification](media-compositions-and-editing.md)
* [Documentation Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm)
* [Lecture de contenu multimédia](media-playback.md)

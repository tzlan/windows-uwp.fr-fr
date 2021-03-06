---
title: Comment désactiver le mode souris
description: Découvrez comment désactiver le mode de souris par défaut dans les applications HTML et XAML/C# plateforme Windows universelle (UWP).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 16b2df2d84c0064c2549c75d867123d90e663314
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094596"
---
# <a name="how-to-disable-mouse-mode"></a>Comment désactiver le mode souris
Le mode souris est activé par défaut sur toutes les applications. Toutes les applications qui ne le refusent pas reçoivent un pointeur de souris (semblable à celui du navigateur Microsoft Edge de la console). Nous vous recommandons vivement de désactiver cette fonctionnalité et d’optimiser la navigation par commande directionnelle.   
   
## <a name="html"></a>HTML   
Pour activer la navigation par commande directionnelle dans une application de plateforme Windows universelle (UWP) JavaScript, utilisez la bibliothèque JavaScript de [navigation directionnelle TVHelpers](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation). Incluez le fichier JavaScript de navigation directionnelle dans le package de votre application, et ajoutez une référence à celui-ci dans toutes les pages HTML nécessitant une navigation par commande directionnelle :

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
Pour plus d’informations, voir le [wiki de navigation directionnelle](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation).

Si vous voulez plutôt désactiver le mode souris et utiliser directement les API de boîtier de commande DOM ou WinRT, exécutez la commande suivante pour chaque page qui l’exige : 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   Par défaut, cette propriété a la valeur `mouse` qui active le mode souris. Si vous la définissez sur `keyboard`, le mode souris est désactivé et, à la place, l’entrée de boîtier de commande génère des événements de clavier DOM. Si vous la définissez sur `gamepad`, le mode souris est désactivé. Cela ne génère pas d’événements de clavier DOM, et vous pouvez seulement utiliser les API de boîtier de commande DOM ou WinRT.

## <a name="xaml"></a>XAML    
Pour désactiver le mode souris, ajoutez le code suivant au constructeur de votre application :   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
Si vous écrivez une application C++/DirectX, vous n’avez rien à faire. Le mode souris s’applique uniquement aux applications HTML et XAML.

## <a name="see-also"></a>Voir aussi
- [Bonnes pratiques pour Xbox](tailoring-for-xbox.md)
- [UWP sur Xbox One](index.md)


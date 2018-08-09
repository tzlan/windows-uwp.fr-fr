---
author: msatranjr
title: Composants Windows Runtime
description: Les composants Windows Runtime sont des objets autonomes que vous pouvez instancier et utiliser dans n’importe quel langage, notamment C#, Visual Basic, JavaScript et C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc6d955ac77409f37b7701bef8c1fa4d93c97e3c
ms.sourcegitcommit: 54c2cd58fde08af889093a0c85e7297e33e6a0eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2018
ms.locfileid: "1664932"
---
# <a name="windows-runtime-components"></a>Composants Windows Runtime



Les composants Windows Runtime sont des objets autonomes que vous pouvez instancier et utiliser dans n’importe quel langage, notamment C#, Visual Basic, JavaScript et C++.

Vous pouvez utiliser Visual Studio et C#, Visual Basic ou C++ pour créer des composants Windows Runtime utilisables dans les applications de plateforme Windows universelle (UWP).

| Rubrique | Description |
|-------|-------------|
| [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) | Cet article explique comment utiliser C++ pour créer un composant Windows Runtime, qui est une DLL qui peut être appelée depuis une application UWP générée à l’aide de JavaScript, ou encore de C#, Visual Basic ou C++. |
| [Procédure pas à pas: création d’un composant WindowsRuntime de base en C++ et appel de ce composant à partir de JavaScript ou C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | Cette procédure pas à pas montre comment créer une DLL de composant Windows Runtime de base qui peut être appelée à partir de JavaScript, C# ou Visual Basic. Avant d’entreprendre cette procédure pas à pas, vous devez maîtriser des concepts tels que l’interface binaire abstraite (ABI), les classes ref et les extensions des composants Visual C++ qui facilitent l’utilisation des classes ref. Pour plus d’informations, consultez les articles [Création de composants Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) et [Informations de référence sur le langage Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx). |
| [Création de composants Windows Runtime en C# et Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Depuis le .NET Framework 4.5, vous pouvez utiliser du code managé pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime. Vous pouvez utiliser votre composant dans les applications de plateforme Windows universelle (UWP) avec C++, JavaScript, Visual Basic ou C#. Cet article présente les règles de création d’un composant et décrit quelques aspects de la prise en charge de .NET Framework pour Windows Runtime. En règle générale, cette prise en charge est conçue pour être transparente pour les programmeurs .NET Framework. Toutefois, lorsque vous créez un composant à utiliser avec JavaScript ou C++, vous devez tenir compte des différences de prise en charge de Windows Runtime par ces langages. |
| [Procédure pas à pas: création d’un composant Windows Runtime simple et appel de ce composant à partir de JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | Cette procédure pas à pas montre comment utiliser .NET Framework avec Visual Basic ou C# pour créer vos propres types Windows Runtime, empaquetés dans un composant Windows Runtime, et comment appeler le composant à partir de votre application Windows universelle générée pour Windows à l’aide de JavaScript. |
| [Déclenchement d’événements dans les composants Windows Runtime](raising-events-in-windows-runtime-components.md) | Si votre composant Windows Runtime déclenche un événement d’un type délégué défini par l’utilisateur sur un thread d’arrière-plan (thread de travail) et vous souhaitez que JavaScript puisse recevoir l’événement, vous pouvez l’implémenter ou le déclencher de plusieurs manières: | 
| [Composants WindowsRuntime du service Broker pour les applications installées hors UWP](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | Cette rubrique présente une fonctionnalité de la mise à jour Windows10 et versions supérieures, destinée aux entreprises, qui permet aux applications .NET tactiles d’utiliser le code responsable des opérations d’entreprise stratégiques. |
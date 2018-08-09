---
author: stevewhims
description: C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête.
title: C++/WinRT
ms.author: stwhi
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, standard, c++, cpp, winrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: 515ac1f9a079e3791be8835f1a33c16198e27362
ms.sourcegitcommit: 929fa4b3273862dcdc76b083bf6c3b2c872dd590
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1935635"
---
# [<a name="cwinrt"></a>C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
C++/WinRT est une projection de langage C++17 moderne entièrement standard pour les API Windows Runtime (WinRT), implémentée en tant que bibliothèque basée sur un fichier d'en-tête et conçue pour vous fournir un accès de première classe à l’API Windows moderne. Avec C++/WinRT, vous pouvez créer et utiliser des API Windows Runtime en utilisant n’importe quel compilateur C++17 conforme aux normes. Le SDK Windows inclut C++/WinRT. Il a été introduit dans la version10.0.17134.0 (Windows10, version1803).

C++/WinRT est destiné à tous les développeurs intéressés pour écrire du code sublime et rapide pour Windows. Voici pourquoi.

## <a name="the-case-for-cwinrt"></a>Les arguments en faveur de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

Le langage de programmation C++ est utilisé à la fois dans les segments des entreprises *et* des éditeurs de logiciels indépendants (ISV) pour les applications où des niveaux élevés d’exactitude, de qualité et de performances sont importants. Par exemple: programmation de systèmes, systèmes mobiles et embarqués avec restriction de ressources, jeux et graphiques, pilotes de périphériques et applications médicales, scientifiques et industrielles, pour n’en citer que quelques-uns.

D’un point de vue du langage, C++ a toujours été destiné à la création et l’utilisation d’abstractions qui sont à la fois riches en types et légères. Mais le langage a changé considérablement depuis les pointeurs bruts, les boucles brutes et l’allocation et la libération laborieuses de mémoire de C++98. Le C++ moderne (à partir de C++11) s’attache à l’expression claire des idées, à la simplicité, à la lisibilité et à limiter l'introduction de bogues.

Pour la création et l’utilisation d’API Windows Runtime avec C++, il existe C++/WinRT. Il s’agit du remplacement recommandé par Microsoft pour la [(bibliothèque de modèles C++ WindowsRuntime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) et [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live).

Vous utilisez des types de données, des algorithmes et des mots-clés C++ standard lorsque vous utilisez C++/WinRT. La projection a ses propres types de données personnalisés, mais dans la plupart des cas vous n’avez pas besoin de les apprendre car ils fournissent des conversions appropriées vers et depuis les types standard. De cette façon, vous pouvez continuer à utiliser les fonctionnalités de langage C++ standard auxquelles vous êtes habitué, et le code source que vous avez déjà. C++/WinRT facilite beaucoup l’appel des API Windows Runtime dans n’importe quelle application C++, de Win32 à UWP.

C++/WinRT offre de meilleures performances et génère des fichiers binaires plus petits que toute autre option de langage pour Windows Runtime. Il surpasse même le code manuscrit en utilisant directement les interfaces ABI. Cela est dû au fait que les abstractions utilisent des idiomes C++ modernes que le compilateur Visual C++ est conçu pour optimiser. Cela inclut magic statics, les classes de base vide, l'élision **strlen**, ainsi que de nombreuses optimisations plus récentes de la dernière version de Visual C++ destinée spécifiquement à améliorer les performances de C++/WinRT.

| Rubrique | Description |
| - | - |
| [Présentation de C++/WinRT](intro-to-using-cpp-with-winrt.md) | Présentation de C++/WinRT &mdash;une projection de langage C++ standard pour les API Windows Runtime. |
| [Prise en main de C++/WinRT](get-started.md) | Pour vous aider à utiliser rapidement C++/WinRT, cette rubrique présente un exemple simple de code. |
| [Forum aux questions](faq.md) | Réponses aux questions que vous êtes susceptibles de vous poser sur la création et l’utilisation d’API Windows Runtime avec C++/WinRT. |
| [Résolution des problèmes](troubleshooting.md) | Le tableau de résolution des problèmes et des solutions de cette rubrique peut vous être utile, que vous coupiez un nouveau code ou portiez une application existante. |
| [Exemple d’application C++/WinRT Photo Editor](photo-editor-sample.md) | Photo Editor est un exemple d’application UWP qui illustre le développement à l'aide de la projection de langage C++/WinRT. L’exemple d’application vous permet de récupérer des photos à partir de la bibliothèque **Images**, puis de modifier l’image sélectionnée avec des effets de photo assortis. | 
| [Gestion des chaînes](strings.md) | Avec C++/WinRT, vous pouvez appeler des API Windows Runtime à l’aide de types de chaînes étendues C++ standard, ou vous pouvez utiliser le type [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). |
| [Types de données C++ standard et C++/WinRT](std-cpp-data-types.md) | Avec C++/WinRT, vous pouvez appeler les API Windows Runtime à l’aide de types de données C++ standard. |
| [Conversions boxing et unboxing de valeurs scalaires vers IInspectable](boxing.md) | Une valeur scalaire doit être encapsulée dans un objet de classe de référence avant d’être transmise à une fonction qui attend **IInspectable**. Ce processus d’encapsulation est appelé «*conversion boxing*» de la valeur. |
| [Utiliser des API avec C++/WinRT](consume-apis.md) | Cette rubrique montre comment utiliser des API C++/WinRT, qu’elles soient implémentées par Windows, un fournisseur de composants tiers ou par vous-même. |
| [Créer des API avec C++/WinRT](author-apis.md) | Cette rubrique montre comment créer des API C++/WinRT à l’aide de la structure de base **winrt::implements**, directement ou indirectement. |
| [Gestion des erreurs avec C++/WinRT](error-handling.md) | Cette rubrique décrit les stratégies de gestion des erreurs lors de la programmation avec C++/WinRT. |
| [Gérer des événements en utilisant des délégués](handle-events.md) | Cette rubrique montre comment inscrire et révoquer des délégués de gestion d’événements à l’aide de C++/WinRT. |
| [Créer des événements](author-events.md) | Cette rubrique montre comment créer un composant Windows Runtime contenant une classe runtime qui déclenche des événements. Elle montre également une application qui utilise le composant et gère les événements. |
| [Opérations concurrentes et asynchrones](concurrency.md) | Cette rubrique présente les manières dont vous pouvez à la fois créer et utiliser des objets asynchrones Windows Runtime avec C++/WinRT. |
| [Contrôles XAML; liaison à une propriété C++/WinRT](binding-property.md) | Une propriété qui peut être efficacement liée à un contrôle XAML est appelée «propriété *observable*». Cette rubrique montre comment implémenter et utiliser une propriété observable, et comment y lier un contrôle XAML. |
| [Contrôles d’éléments XAML; liaison à une collection C++/WinRT](binding-collection.md) | Une collection qui peut être efficacement liée à un contrôle d’éléments XAML est appelée «collection *observable*». Cette rubrique montre comment implémenter et utiliser une collection observable, et comment y lier un contrôle d’éléments XAML. |
| [Interopérabilité entre C++/WinRT et C++/CX](interop-winrt-cx.md) | Cette rubrique montre deux fonctions d’assistance qui peuvent être utilisées pour effectuer des conversions entre des objets [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) et C++/WinRT. |
| [Passer de C++/CX à C++/WinRT](move-to-winrt-from-cx.md) | Cette rubrique montre comment porter du code C++/CX vers son équivalent en C++/WinRT. |
| [Interopérabilité entre C++/WinRT et ABI](interop-winrt-abi.md) | Cette rubrique montre comment effectuer des conversions entre des objets de l’interface binaire d’application (ABI) et C++/WinRT. |
| [Passer de WRL à C++/WinRT](move-to-winrt-from-wrl.md) | Cette rubrique montre comment porter du code [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) vers son équivalent en C++/WinRT. |
| [Références faibles](weak-references.md) | La prise en charge des références faibles C++/WinRT est de type «payer pour lire», dans la mesure où elle ne vous coûte rien, sauf si votre objet est interrogé pour [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609). |
| [Objets agiles](agile-objects.md) | Un objet agile est un objet qui est accessible à partir de n’importe quel thread. Vos types C++/WinRT sont agiles par défaut, mais vous pouvez le refuser. |

## <a name="important-apis"></a>API importantes
* [Espace de noms winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Rubriquesassociées
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Bibliothèque de modèles C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
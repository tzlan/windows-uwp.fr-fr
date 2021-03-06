---
description: Nous décrivons le concept de programmation des événements dans une application Windows Runtime, quand vous utilisez C#, Visual Basic ou les extensions de composants Visual C++ (C++/CX) comme langage de programmation et le langage XAML pour la définition de votre interface utilisateur.
title: Vue d’ensemble des événements et des événements routés
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67ee1f31e1480e0aa805e161433976d5e789d00c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155093"
---
# <a name="events-and-routed-events-overview"></a>Vue d’ensemble des événements et des événements routés

**API importantes**
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Nous décrivons le concept de programmation des événements dans une application Windows Runtime, quand vous utilisez C#, Visual Basic ou les extensions de composants Visual C++ (C++/CX) comme langage de programmation et le langage XAML pour la définition de votre interface utilisateur. Vous pouvez assigner des gestionnaires pour les événements dans le cadre des déclarations des éléments d’interface utilisateur en XAML, ou vous pouvez ajouter les gestionnaires dans le code. Windows Runtime prend en charge les *événements routés* : certains événements d’entrée et événements de données peuvent être gérés par des objets autres que l’objet ayant déclenché l’événement. Les événements routés s’avèrent utiles quand vous définissez des modèles de contrôles ou utilisez des pages ou conteneurs de disposition.

## <a name="events-as-a-programming-concept"></a>Événements en tant que concept de programmation

En règle générale, les concepts d’événement dans le cadre de la programmation d’une application Windows Runtime s’apparentent au modèle d’événement dans les langages de programmation les plus répandus. Le fait de savoir déjà utiliser des événements Microsoft .NET ou C++ procure un certain avantage, mais vous n’avez pas besoin d’en savoir énormément sur les concepts de modèle d’événement pour effectuer certaines tâches de base, comme l’attachement des gestionnaires.

Quand vous utilisez C#, Visual Basic ou C++/CX comme langage de programmation, l’interface utilisateur est définie dans le balisage (XAML). Dans la syntaxe du balisage XAML, certains des principes de connexion des événements entre les éléments de balisage et les entités de code d’exécution sont similaires à d’autres technologies Web, comme ASP.NET ou HTML5.

**Remarque**    Le code qui fournit la logique Runtime pour une interface utilisateur définie en XAML est souvent appelé *code-behind* ou fichier code-behind. Dans les affichages de solutions Microsoft Visual Studio, cette relation est représentée graphiquement, avec le fichier code-behind en tant que fichier dépendant et incorporé par rapport à la page XAML auquel il se réfère.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click : présentation des événements et de XAML

L’une des tâches de programmation courante pour une application Windows Runtime consiste à capturer une entrée utilisateur dans l’interface utilisateur. Par exemple, votre interface utilisateur peut comporter un bouton sur lequel l’utilisateur doit cliquer pour envoyer des informations ou modifier un état.

Vous définissez l’interface utilisateur de votre application Windows Runtime en générant du code XAML. Ce code XAML constitue habituellement la sortie d’une aire de conception dans Visual Studio. Vous pouvez également écrire le code XAML dans un éditeur de texte brut ou un éditeur XAML tiers. Au moment de la génération de ce code XAML, vous pouvez connecter des gestionnaires d’événements pour un élément d’interface utilisateur individuel en même temps que vous définissez tous les autres attributs XAML qui établissent les valeurs de propriétés de cet élément d’interface utilisateur.

Pour connecter les événements en XAML, vous spécifiez le nom sous forme de chaîne de la méthode de gestionnaire que vous avez déjà définie ou que vous définirez plus tard dans votre code-behind. Par exemple, ce code XAML définit un objet [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) avec d’autres propriétés ([attribut x:Name](x-name-attribute.md), [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) assignées en tant qu’attributs, puis connecte un gestionnaire pour l’événement [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) du bouton en référençant une méthode nommée `ShowUpdatesButton_Click` :

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Conseil** La   *connexion d’événements* est un terme de programmation. Il fait référence au processus ou au code par lequel vous indiquez que les occurrences d’un événement doivent invoquer une méthode de gestionnaire nommé. Dans la plupart des modèles de code procédural, la connexion d’événements est le code « AddHandler » implicite ou explicite qui nomme l’événement et la méthode, et qui implique généralement une instance d’objet cible. En XAML, le code « AddHandler » est implicite. La connexion d’événements consiste exclusivement à nommer l’événement en tant que nom d’attribut d’un élément d’objet, et à nommer le gestionnaire en tant que valeur de cet attribut.

Vous écrivez le gestionnaire réel dans le langage de programmation que vous utilisez pour tout le code et le code-behind de votre application. Avec l’attribut `Click="ShowUpdatesButton_Click"`, vous avez créé un contrat qui veut que lorsque le balisage XAML est compilé et analysé, l’étape de compilation du balisage XAML dans l’action de génération de votre IDE et l’analyse XAML finale lors du chargement de l’application peuvent trouver une méthode nommée `ShowUpdatesButton_Click` dans le code de l’application. `ShowUpdatesButton_Click` doit être une méthode qui implémente une signature de méthode compatible (basée sur un délégué) pour n’importe quel gestionnaire de l’événement [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) . Par exemple, ce code définit le gestionnaire `ShowUpdatesButton_Click`.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

Dans cet exemple, la méthode `ShowUpdatesButton_Click` est basée sur le délégué [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). Vous savez qu’il s’agit du délégué à utiliser, car vous verrez ce délégué nommé dans la syntaxe de la méthode [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) .

**Conseil**    Visual Studio offre un moyen pratique de nommer le gestionnaire d’événements et de définir la méthode de gestionnaire pendant que vous modifiez le code XAML. Quand vous fournissez le nom d’attribut de l’événement dans l’éditeur de texte XAML, patientez jusqu’à ce qu’une liste Microsoft IntelliSense s’affiche. Si vous cliquez sur ** &lt; nouveau gestionnaire &gt; d’événements** dans la liste, Microsoft Visual Studio suggère un nom de méthode basé sur le **x :Name** de l’élément (ou le nom de type), le nom de l’événement et un suffixe numérique. Vous pouvez ensuite cliquer avec le bouton droit sur le nom du gestionnaire d’événements sélectionné et cliquer sur **Naviguer vers le gestionnaire d’événements**. Vous accéderez alors directement à la définition de gestionnaire d’événements nouvellement insérée, comme illustré dans l’affichage de l’éditeur de code de votre fichier code-behind pour la page XAML. Le gestionnaire d’événements a déjà la signature correcte, y compris le paramètre *sender* et la classe de données d’événements utilisée par l’événement. En outre, si une méthode de gestionnaire avec la signature correcte existe déjà dans votre code-behind, le nom de cette méthode apparaît dans la liste déroulante de saisie semi-automatique avec la nouvelle option de ** &lt; gestionnaire &gt; d’événements** . Vous pouvez aussi appuyer sur la touche Tab, pour aller plus vite, au lieu de cliquer sur les éléments de liste IntelliSense.

## <a name="defining-an-event-handler"></a>Définition d’un gestionnaire d’événements

Pour les objets qui sont des éléments d’interface utilisateur déclarés en XAML, le code de gestionnaire d’événements est défini dans la classe partielle qui sert de code-behind pour une page XAML. Les gestionnaires d’événements sont des méthodes que vous écrivez dans le cadre de la classe partielle associée à votre code XAML. Ces gestionnaires d’événements sont basés sur les délégués utilisés par un événement particulier. Vos méthodes de gestionnaires d’événements peuvent être publiques ou privées. L’accès privé fonctionne car le gestionnaire et l’instance créés par le code XAML sont finalement associés par la génération de code. En général, il est recommandé de rendre privées vos méthodes de gestionnaires d’événements dans la classe.

**Remarque**    Les gestionnaires d’événements pour C++ ne sont pas définis dans les classes partielles, ils sont déclarés dans l’en-tête en tant que membre de classe privée. Les actions de génération pour un projet C++ se chargent de générer le code qui prend en charge le système de types XAML et le modèle code-behind pour C++.

### <a name="the-sender-parameter-and-event-data"></a>Paramètre *sender* et données d’événement

Le gestionnaire que vous écrivez pour l’événement peut accéder à deux valeurs disponibles en tant qu’entrée chaque fois que votre gestionnaire est invoqué. La première valeur est *sender*, laquelle est une référence à l’objet auquel le gestionnaire est attaché. Le paramètre *sender* est typé en tant que type **Object** de base. Une technique courante consiste à effectuer un transtypage de *sender* vers un type plus précis. Cette technique s’avère utile si vous prévoyez de vérifier ou modifier l’état sur l’objet *sender* lui-même. En fonction de la conception de votre propre application, vous connaissez généralement un type vers lequel il est sûr d’effectuer un transtypage de *sender*, selon l’élément auquel le gestionnaire est attaché ou d’autres caractéristiques de conception.

La seconde valeur correspond aux données d’événement, qui apparaissent généralement dans les définitions de syntaxe sous la forme du paramètre *e*. Vous pouvez découvrir quelles propriétés sont disponibles pour les données d’événement en consultant le paramètre *e* du délégué qui est assigné pour l’événement spécifique que vous gérez, puis en utilisant IntelliSense ou l’Explorateur d’objets dans Visual Studio. Ou vous pouvez utiliser la documentation de référence Windows Runtime.

Pour certains événements, les valeurs de propriétés spécifiques des données d’événements sont aussi importantes que le fait de savoir que l’événement s’est produit. Ceci est particulièrement vrai pour les événements d’entrée. Pour les événements de pointeur, la position du pointeur lorsque l’événement s’est produit peut s’avérer importante. Pour les événements de clavier, toutes les activations de touches possibles déclenchent un événement [**Keyverse**](/uwp/api/windows.ui.xaml.uielement.keydown) et [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) . Pour déterminer la clé sur laquelle un utilisateur a appuyé, vous devez accéder au [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) qui est disponible pour le gestionnaire d’événements. Pour plus d’informations sur la gestion des événements d’entrée, voir [Interactions avec le clavier](../design/input/keyboard-interactions.md) et [Gérer les entrées du pointeur](../design/input/handle-pointer-input.md). Les événements et scénarios d’entrée induisent souvent d’autres considérations qui ne sont pas décrites dans cette rubrique, comme la capture du pointeur pour les événements de pointeur, ainsi que les touches de modification et les codes de touche de plateforme pour les événements de clavier.

### <a name="event-handlers-that-use-the-async-pattern"></a>Gestionnaires d’événements qui utilisent le modèle **async**

Dans certains cas, vous pouvez utiliser des API qui emploient un modèle **async** dans un gestionnaire d’événements. Par exemple, vous pouvez utiliser un [**bouton**](/uwp/api/Windows.UI.Xaml.Controls.Button) dans un [**appbar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) pour afficher un sélecteur de fichier et interagir avec lui. Toutefois, un grand nombre des API de sélecteur de fichiers sont asynchrones. Elles doivent être appelées dans une étendue **async**/awaitable, et le compilateur se charge de l’opération. Ainsi, vous pouvez ajouter le mot clé **async** à votre gestionnaire d’événements pour que le gestionnaire soit désormais **async** **void**. Votre gestionnaire d’événements est à présent autorisé à effectuer des appels **async**/awaitable.

Pour obtenir un exemple de gestion des événements d’interaction utilisateur à l’aide du modèle **async**, consultez [Accès aux fichiers et sélecteurs](/previous-versions/windows/apps/jj655411(v=win.10)) (partie de la série de didacticiels [Créer votre première application Windows Runtime en C# ou Visual Basic](/previous-versions/windows/apps/hh974581(v=win.10))). Voir aussi [Appeler des API asynchrones en C].

## <a name="adding-event-handlers-in-code"></a>Ajout de gestionnaires d’événements dans le code

Le langage XAML ne constitue pas la seule manière d’assigner un gestionnaire d’événements à un objet. Pour ajouter des gestionnaires d’événements à un objet donné dans du code, notamment un objet inutilisable en XAML, vous pouvez utiliser la syntaxe propre au langage.

En C#, la syntaxe doit utiliser l’opérateur `+=`. Pour inscrire le gestionnaire, vous référencez le nom de la méthode du gestionnaire d’événements à droite de l’opérateur.

Si vous utilisez du code pour ajouter des gestionnaires d’événements à des objets qui apparaissent dans l’interface utilisateur au moment de l’exécution, une pratique courante consiste à les ajouter en réponse à un rappel ou événement de durée de vie de l’objet, comme [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) ou [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), afin que les gestionnaires d’événements de l’objet concerné soient prêts pour les événements initialisés par l’utilisateur au moment de l’exécution. Cet exemple montre un plan XAML de la structure de la page, puis fournit la syntaxe en langage C# permettant d’ajouter un gestionnaire d’événements à un objet.

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**Remarque**    Il existe une syntaxe plus détaillée. En 2005, C# a ajouté une fonctionnalité appelée inférence de délégué, qui permet à un compilateur d’inférer la nouvelle instance de délégué et permet d’activer la syntaxe précédente plus simple. La syntaxe détaillée est identique d’un point de vue fonctionnel à l’exemple précédent, mais elle permet de créer explicitement une instance de délégué avant de l’inscrire, en tirant ainsi parti de l’inférence de délégué. Cette syntaxe explicite est moins courante, mais il se peut que vous la rencontriez dans certains exemples de code.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Il existe deux possibilités pour la syntaxe Visual Basic. L’une consiste à trouver un équivalent à la syntaxe C# et à attacher des gestionnaires directement aux instances. Cette possibilité requiert le mot-clé **AddHandler** et l’opérateur **AddressOf** qui déréférence le nom de la méthode du gestionnaire.

L’autre possibilité pour la syntaxe Visual Basic consiste à utiliser le mot clé **Handles** sur les gestionnaires d’événements. Cette technique convient aux cas dans lesquels les gestionnaires d’événements sont censés exister sur les objets au moment du chargement et persister pendant toute la durée de vie des objets. L’utilisation du mot clé **Handles** sur un objet qui est défini en XAML nécessite que vous fournissiez un **Name** / **x:Name**. Ce nom devient le qualificateur d’instance requis pour la partie *Instance.Event* de la syntaxe **Handles**. Dans ce cas, vous n’avez pas besoin d’un gestionnaire d’événements basé sur la durée de vie des objets pour initialiser l’attachement des autres gestionnaires d’événements ; les connexions **Handles** sont créées lorsque vous compilez la page XAML.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Remarque**    Visual Studio et son aire de conception XAML favorisent généralement la technique de gestion d’instance au lieu du mot clé **Handles** . Cela est dû au fait que l’établissement de la connexion des gestionnaires d’événements en XAML fait partie du flux de travail concepteur-développeur habituel et que la technique du mot-clé **Handles** est incompatible avec cette connexion des gestionnaires d’événements en XAML.

En C++/CX, vous utilisez également la **+=** syntaxe, mais il existe des différences par rapport au formulaire C# de base :

- Il n’existe aucune inférence de délégué, donc vous devez utiliser **ref new** pour l’instance de délégué.
- Le constructeur du délégué possède deux paramètres et requiert que l’objet cible soit le premier d’entre eux. En général, vous spécifiez **this**.
- Le constructeur délégué exige que l’adresse de la méthode soit le second paramètre, de sorte que l' **&** opérateur de référence précède le nom de la méthode.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Suppression de gestionnaires d’événements dans le code

Il n’est généralement pas nécessaire de supprimer les gestionnaires d’événements du code, même si vous les avez ajoutés dans le code. Le comportement de la durée de vie des objets pour la plupart des objets Windows Runtime tels que les pages et les contrôles détruira les objets lorsqu’ils seront déconnectés de la [**fenêtre**](/uwp/api/Windows.UI.Xaml.Window) principale et de son arborescence d’éléments visuels, et que toutes les références de délégué sont également détruites. .NET effectue cette opération via un nettoyage de la mémoire et Windows Runtime avec C++/CX utilise des références faibles par défaut.

Il existe de rares cas où vous voulez supprimer explicitement des gestionnaires d’événements. notamment :

- les gestionnaires que vous avez ajoutés pour des événements statiques, lesquels ne peuvent pas être nettoyés de la mémoire de manière conventionnelle. Les événements des classes [**CompositionTarget**](/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) et [**Clipboard**](/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) sont des exemples d’événements statiques dans l’API Windows Runtime.
- le code de test pour lequel vous voulez une suppression immédiate des gestionnaires, ou le code pour lequel vous voulez permuter les anciens et les nouveaux gestionnaires d’événements pour un événement au moment de l’exécution ;
- l’implémentation d’un accesseur **remove** personnalisé ;
- les événements statiques personnalisés.
- Gestionnaires des navigations au sein des pages.

[**FrameworkElement. uncharged**](/uwp/api/windows.ui.xaml.frameworkelement.unloaded) ou [**page. NavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) sont des déclencheurs d’événements possibles qui ont des positions appropriées dans la gestion d’État et la durée de vie des objets, ce qui vous permet de les utiliser pour supprimer des gestionnaires pour d’autres événements.

Par exemple, vous pouvez supprimer un gestionnaire d’événements nommé **textBlock1 \_ PointerEntered** de l’objet cible **textBlock1** à l’aide de ce code.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Vous pouvez également supprimer des gestionnaires dans les cas où l’événement a été ajouté via un attribut XAML, ce qui signifie que le gestionnaire a été ajouté dans du code généré. Cette opération est plus simple si vous avez fourni une valeur**Name** pour l’élément auquel le gestionnaire était attaché, car cela fournit une référence d’objet pour le code ultérieurement ; toutefois, vous pouvez également parcourir l’arborescence d’objets afin de trouver la référence d’objet nécessaire dans les cas où l’objet n’a pas de valeur **Name**.

Si vous devez supprimer un gestionnaire d’événements en C++/CX, vous avez besoin d’un jeton d’inscription, que vous devez avoir reçu de la valeur de retour de l’inscription du gestionnaire d’événements `+=`. En effet, la valeur que vous utilisez pour le côté droit de l’annulation de l’inscription `-=` dans la syntaxe C++/CX est le jeton, et non le nom de la méthode. Pour C++/CX, vous ne pouvez pas supprimer des gestionnaires qui ont été ajoutés en tant qu’attribut XAML, car le code généré C++/CX n’enregistre pas de jeton.

## <a name="routed-events"></a>Événements routés

Windows Runtime avec C#, Microsoft Visual Basic ou C++/CX prend en charge le concept d’événement routé pour un ensemble d’événements présents sur la plupart des éléments d’interface utilisateur. Ces événements sont destinés à des scénarios d’entrée et d’interaction de l’utilisateur, et sont implémentés sur la classe de base [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement). Voici une liste des événements d’entrée qui sont des événements routés :

- [**BringIntoViewRequested**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Déplacez**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**Événementiel**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)

Un événement routé est un événement potentiellement transmis (*routé*) depuis un objet enfant vers chacun de ses objets parents successifs dans une arborescence d’objets. La structure XAML de votre interface utilisateur ressemble à peu près à cette arborescence, la racine de l’arborescence étant l’élément racine en XAML. La vraie arborescence d’objets peut varier un peu par rapport à l’imbrication des éléments XAML car elle n’inclut pas certaines fonctionnalités du langage XAML telles que les balises d’élément de propriété. Vous pouvez considérer l’événement routé comme une *propagation* depuis tout élément enfant de l’élément de l’objet XAML qui déclenche l’événement vers l’élément de l’objet parent qui le contient. L’événement et ses données d’événement peuvent être gérés sur plusieurs objets tout au long de l’itinéraire de l’événement. Si aucun élément n’a de gestionnaires, l’itinéraire se poursuit jusqu’à ce que l’élément racine soit atteint.

Si vous connaissez des technologies Web telles que Dynamic HTML (DHTML) ou HTML5, le concept d’événement de *propagation* ne vous est peut-être pas étranger.

Quand un événement routé se propage le long de son itinéraire d’événement, tout gestionnaire d’événement attaché accède à une instance partagée des données d’événements. Par conséquent, si l’une des données d’événement est accessible en écriture par un gestionnaire, toute modification apportée aux données d’événement est transmise au gestionnaire suivant et risque de ne plus représenter les données d’événement d’origine de l’événement. Quand un événement a un comportement d’événement routé, la documentation de référence inclut des remarques ou d’autres indications sur le comportement routé.

### <a name="the-originalsource-property-of-routedeventargs"></a>Propriété **OriginalSource** de **RoutedEventArgs**

Lorsqu’un événement se propage sur un itinéraire d’événement, *sender* n’est plus le même objet que celui qui a déclenché l’événement. En effet, *sender* est plutôt l’objet auquel le gestionnaire qui est invoqué est attaché.

Dans certains cas, *sender* n’est pas intéressant et ce que vous voulez savoir, c’est plutôt sur quel objet enfant le pointeur se trouve quand un événement de pointeur se déclenche ou quel objet dans une interface utilisateur plus grande a le focus quand l’utilisateur appuie sur une touche du clavier. Dans ce cas, vous pouvez utiliser la valeur de la propriété [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) . À tous les stades de l’itinéraire, **OriginalSource** signale l’objet d’origine qui a déclenché l’événement, plutôt que l’objet auquel le gestionnaire est attaché. Toutefois, pour les événements d’entrée [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) , cet objet d’origine est souvent un objet qui n’est pas immédiatement visible dans le XAML de définition d’interface utilisateur au niveau de la page. Au lieu de cela, cet objet source d’origine peut être une partie basée sur un modèle d’un contrôle. Par exemple, si l’utilisateur passe le pointeur au-dessus du bord d’un objet [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button), pour la plupart des événements de pointeur, l’objet **OriginalSource** est une partie de modèle [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) dans la propriété [**Template**](/uwp/api/windows.ui.xaml.controls.control.template), et non l’objet **Button** lui-même.

**Conseil**    La propagation des événements d’entrée est particulièrement utile si vous créez un contrôle basé sur un modèle. Tout contrôle qui possède un modèle peut se voir appliquer un nouveau modèle par son consommateur. Le consommateur qui essaie de recréer un modèle de travail peut involontairement éliminer une gestion des événements déclarée dans le modèle par défaut. Vous pouvez toujours fournir la gestion des événements au niveau du contrôle en attachant des gestionnaires dans le cadre de la substitution [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) dans la définition de classe. Vous pouvez ensuite intercepter les événements d’entrée qui se propagent à la racine du contrôle lors de l’instanciation.

### <a name="the-handled-property"></a>Propriété **Handled**

Plusieurs classes de données d’événement pour des événements routés spécifiques contiennent une propriété nommée **Handled**. Pour obtenir des exemples, consultez [**PointerRoutedEventArgs. Handled**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs. Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled), [**DragEventArgs. Handled**](/uwp/api/windows.ui.xaml.drageventargs.handled). Dans tous les cas, **Handled** est une propriété booléenne définissable.

L’affectation à la propriété **Handled** de la valeur **true** influence le comportement du système d’événements. Quand **Handled** a la valeur **true**, le routage s’arrête pour la plupart des gestionnaires d’événements ; l’événement ne poursuit pas l’itinéraire pour informer les autres gestionnaires attachés de ce cas d’événement particulier. Il vous appartient de déterminer ce que signifie la propriété « handled » dans le contexte de l’événement et comment votre application y répond. En fait, **Handled** est un protocole simple qui permet au code d’application d’indiquer qu’une occurrence d’un événement ne doit pas être propagée vers des conteneurs. La logique de votre application a effectué les opérations requises. À l’inverse, vous devez veiller à ne pas gérer les événements susceptibles de se propager afin que les comportements système ou de contrôle intégrés puissent agir. Par exemple, la gestion des événements de bas niveau dans les parties ou les éléments d’un contrôle de sélection peut être nuisible. Le contrôle de sélection peut être à la recherche d’événements d’entrée pour savoir que la sélection doit changer.

Les événements routés ne peuvent pas tous annuler un itinéraire de cette façon et vous pouvez le savoir car ils n’ont pas de propriété **Handled**. Par exemple, [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) et [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) se propagent, mais toujours jusqu’à la racine et leurs classes de données d’événements n’ont pas de propriété **Handled** capable d’influencer ce comportement.

##  <a name="input-event-handlers-in-controls"></a>Gestionnaires d’événements d’entrée dans les contrôles

Parfois, des contrôles Windows Runtime spécifiques utilisent le concept **Handled** pour les événements d’entrée en interne. Cela peut donner l’impression qu’un événement d’entrée ne s’est jamais produit, car votre code utilisateur ne peut pas le gérer. Par exemple, la classe [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) comprend une logique qui gère délibérément l’événement d’entrée général [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed). Il en est ainsi car les boutons déclenchent un événement [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) qui est initialisé par une entrée par appui du pointeur, ainsi que par d’autres modes d’entrée tels que la gestion de touches comme la touche Entrée capables d’appeler un bouton lorsqu’il a le focus. À des fins de conception de la classe de **Button**, l’événement d’entrée brute est géré de manière conceptuelle et les consommateurs de la classe tels que votre code utilisateur peuvent plutôt interagir avec l’événement **Click** propre au contrôle. Les rubriques sur les classes de contrôles spécifiques dans la documentation de référence d’API Windows Runtime indiquent souvent le comportement de gestion des événements implémenté par la classe. Dans certains cas, vous pouvez changer le comportement en substituant les méthodes **On**_Event_. Par exemple, vous pouvez modifier la manière dont votre classe dérivée [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) réagit à la saisie au clavier en substituant [**Control.OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown).

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Inscription de gestionnaires d’événements pour des événements routés déjà gérés

Nous avons indiqué précédemment que l’affectation à **Handled** de la valeur **true** permettait d’empêcher l’appel de la plupart des gestionnaires d’événements. Toutefois, la méthode [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) propose une technique qui permet d’attacher un gestionnaire qui est toujours invoqué pour l’itinéraire, même si un autre gestionnaire antérieur dans l’itinéraire a affecté à **Handled** la valeur **true** dans les données d’événement partagées. Cette technique s’avère utile si un contrôle que vous utilisez a géré l’événement dans sa composition interne ou pour une logique propre à un contrôle, mais que vous avez quand même besoin d’y répondre sur une instance de contrôle, ou l’interface utilisateur de votre application. Vous devez toutefois l’utiliser avec précaution, car elle peut entrer en contradiction avec l’objectif de **Handled** et éventuellement interrompre les interactions prévues d’un contrôle.

Seuls les événements routés qui ont un identificateur d’événement routé correspondant peuvent utiliser la technique de gestion des événements [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler), car l’identificateur est une entrée requise de la méthode **AddHandler**. Consultez la documentation de référence pour [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) pour obtenir la liste des événements qui ont des identificateurs d’événements routés disponibles. Il s’agit, en grande partie, de la même liste d’événements routés que celle présentée précédemment. La différence réside dans les deux dernières lignes de la liste, [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) et [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus), qui ne possèdent pas d’identificateur d’événement routé. De ce fait, vous ne pouvez pas utiliser **AddHandler** pour ces événements.

## <a name="routed-events-outside-the-visual-tree"></a>Événements routés en dehors de l’arborescence visuelle

Certains objets participent à une relation avec l’arborescence visuelle principale, ce qui, de façon conceptuelle, revient à avoir une superposition des principaux visuels. Ces objets ne font pas partie des relations parent-enfant habituelles qui relient tous les éléments de l’arborescence à la racine visuelle. C’est le cas pour toutes les classes [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) ou [**ToolTip**](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) affichées. Si vous voulez gérer des événements routés à partir d’une classe **Popup** ou **ToolTip**, placez les gestionnaires sur des éléments d’interface utilisateur spécifiques qui se trouvent au sein des classes **Popup** ou **ToolTip** et non sur les éléments **Popup** ou **ToolTip** eux-mêmes. Ne comptez pas sur le routage dans une composition effectuée pour du contenu **Popup** ou **ToolTip**, car le routage des événements routés fonctionne uniquement le long de l’arborescence visuelle principale. Une classe **Popup** ou **ToolTip** n’est pas considérée comme parent des éléments d’interface utilisateur secondaires et ne reçoit jamais l’événement routé, même si elle essaie d’utiliser par exemple l’arrière-plan par défaut de la classe **Popup** en tant que zone de capture des événements d’entrée.

## <a name="hit-testing-and-input-events"></a>Test de positionnement et événements d’entrée

Le *test de positionnement* est l’action qui consiste à déterminer si et où un élément est visible dans l’interface utilisateur durant l’entrée tactile, l’entrée à l’aide de la souris ou l’entrée à l’aide du stylet. Pour les actions tactiles et pour les événements de manipulation ou spécifiques à l’interaction qui sont des conséquences d’une action tactile, un élément doit être visible au test de positionnement pour pouvoir être la source d’événement et déclencher l’événement associé à l’action. Sinon, l’action passe à travers l’élément et atteint tout élément sous-jacent ou élément parent dans l’arborescence visuelle pouvant interagir avec cette entrée. Il existe plusieurs facteurs qui affectent le test de positionnement, mais vous pouvez déterminer si un élément donné peut déclencher des événements d’entrée en vérifiant sa propriété [**IsHitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) . Cette propriété renvoie la valeur **true** uniquement si l’élément remplit les critères suivants :

- La valeur de la propriété [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) de l’élément est [**visible**](/uwp/api/Windows.UI.Xaml.Visibility).
- La valeur de propriété **Background** ou **Fill** de l’élément n’est pas **null**. Une valeur de [**pinceau**](/uwp/api/Windows.UI.Xaml.Media.Brush) **null** entraîne la transparence et l’invisibilité du test de positionnement. (Pour rendre un élément transparent mais disponible pour le test de positionnement, utilisez un pinceau [**Transparent**](/uwp/api/windows.ui.colors.transparent) plutôt que **null**.)

**Remarque**  **Background** et **Fill** ne sont pas définis par [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement), mais plutôt par différentes classes dérivées telles que [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) et [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape). Mais les implications des pinceaux que vous utilisez pour les propriétés de premier plan et d’arrière-plan sont les mêmes pour le test de positionnement et les événements d’entrée, quelle que soit la sous-classe qui implémente les propriétés.

- Si l’élément est un contrôle, sa propriété [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) doit avoir la valeur **true**.
- L’élément doit avoir des dimensions réelles dans la disposition. Un élément dont la propriété [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) ou [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) a la valeur 0 ne déclenche pas d’événement d’entrée.

Des règles de test de positionnement spéciales s’appliquent à certains contrôles. Par exemple, [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) n’a pas de propriété **Background** mais peut quand même faire l’objet d’un test de positionnement dans la région entière de ses dimensions. Les contrôles [**image**](/uwp/api/Windows.UI.Xaml.Controls.Image) et [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) sont testés par test sur leurs dimensions rectangle définies, quel que soit le contenu transparent, tel que le canal alpha dans le fichier source du média affiché. Les contrôles [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) ont un comportement de test de positionnement spécial, car l’entrée peut être gérée par le code html hébergé et déclencher des événements de script.

La plupart des classes [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) et la classe [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) ne peuvent pas être soumises à un test de positionnement dans leur propre arrière-plan, mais peuvent quand même gérer les événements d’entrée utilisateur routés à partir des éléments qu’elles contiennent.

Vous pouvez déterminer les éléments qui se trouvent à la même position qu’un événement d’entrée utilisateur, que les éléments soient ou non soumis à des tests de positionnement. Pour cela, appelez la méthode [**FindElementsInHostCoordinates**](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates). Comme son nom l’indique, cette méthode recherche les éléments à un emplacement relatif à un élément hôte spécifié. Néanmoins, les transformations appliquées et les modifications de disposition peuvent ajuster le système de coordonnées relatives d’un élément et par conséquent avoir un impact sur les éléments qui sont trouvés à un emplacement donné.

## <a name="commanding"></a>Commandes

Un petit nombre d’éléments d’interface utilisateur prend en charge les *commandes*. Les commandes utilisent les événements routés associés à une entrée dans leur implémentation sous-jacente et permettent de traiter l’entrée d’interface utilisateur associée (une certaine action du pointeur, une touche d’accès rapide spécifique) en invoquant un seul gestionnaire de commandes. Si les commandes sont disponibles pour un élément d’interface utilisateur, envisagez d’utiliser ses API de commandes plutôt que les événements d’entrée discrets. Vous utilisez généralement une référence **Binding** dans les propriétés d’une classe qui définit le modèle d’affichage pour les données. Les propriétés contiennent des commandes nommées qui implémentent le modèle de commandes **ICommand** spécifique au langage. Pour plus d’informations, voir [**ButtonBase.Command**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Événements personnalisés dans Windows Runtime

Dans le cadre de la définition d’événements personnalisés, la façon dont vous ajoutez l’événement et ce qu’il signifie pour votre conception de classe dépendent beaucoup du langage de programmation utilisé.

- Pour C# et Visual Basic, vous définissez un événement CLR. Vous pouvez utiliser le modèle d’événement .NET standard, tant que vous n’utilisez pas d’accesseurs personnalisés (**Add** / **Remove**). Conseils supplémentaires :
    - Pour le gestionnaire d’événements, il est conseillé d’utiliser [**System.EventHandler<TEventArgs>**](/dotnet/api/system.eventhandler-1), car il possède une traduction intégrée vers le délégué d’événement générique Windows Runtime [**EventHandler<T>**](/uwp/api/windows.foundation.eventhandler).
    - Ne basez pas votre classe de données d’événements sur la classe [**System.EventArgs**](/dotnet/api/system.eventargs), car elle n’effectue pas de traduction vers Windows Runtime. Utilisez une classe de données d’événements existante ou aucune classe de base.
    - Si vous utilisez des accesseurs personnalisés, consultez [événements personnalisés et accesseurs d’événement dans Windows Runtime composants](/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Si vous ne savez pas exactement quel est le modèle d’événement .NET standard, voir la rubrique relative à la [Définition d’événements pour les classes Silverlight personnalisées](/previous-versions/windows/). Elle est rédigée pour Microsoft Silverlight, mais elle constitue néanmoins un bon résumé du code et des concepts relatifs au modèle d’événement .NET standard.
- Pour C++/CX, voir [Événements (C++/CX)](/cpp/cppcx/events-c-cx).
    - Utilisez des références nommées même pour vos propres utilisations d’événements personnalisés. N’utilisez pas d’expression lambda pour les événements personnalisés, car cela peut créer une référence circulaire.

Vous ne pouvez pas déclarer un événement routé personnalisé pour Windows Runtime ; les événements routés se limitent à l’ensemble fourni par Windows Runtime.

La définition d’un événement personnalisé s’effectue généralement dans le cadre de l’exercice de définition d’un contrôle personnalisé. Il est courant d’avoir une propriété de dépendance ayant un rappel de propriété modifiée, et de définir également un événement personnalisé qui est déclenché par ce rappel de propriété modifiée dans certains ou tous les cas. Les consommateurs de votre contrôle n’ont pas accès au rappel de propriété modifiée défini, mais avoir un événement de notification disponible est la prochaine meilleure mesure à prendre. Pour plus d’informations, voir [Propriétés de dépendance personnalisées](custom-dependency-properties.md).

## <a name="related-topics"></a>Rubriques connexes

* [Vue d’ensemble du langage XAML](xaml-overview.md)
* [Démarrage rapide : entrée tactile](/previous-versions/windows/apps/hh465387(v=win.10))
* [Interactions avec le clavier](../design/input/keyboard-interactions.md)
* [Événements et délégués .NET](/previous-versions/17sde2xt(v=vs.110))
* [Création de composants Windows Runtime](/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler)
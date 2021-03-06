---
description: Décrit le concept des homologues pour UI Automation, et la manière dont vous pouvez fournir l’automatisation pour votre classe d’interface utilisateur.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Homologues d’automation personnalisés
label: Custom automation peers
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 21f583cc529092b28aa8bc3efde9de6247a4373a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032512"
---
# <a name="custom-automation-peers"></a>Homologues d’automation personnalisés  

Décrit le concept des homologues pour UI Automation, et la manière dont vous pouvez fournir l’automatisation pour votre classe d’interface utilisateur.

UI Automation procure une infrastructure utilisable par les clients Automation pour examiner ou utiliser les interfaces utilisateur de diverses infrastructures et plateformes d’interface utilisateur. Si vous écrivez une application Windows, les classes que vous utilisez pour votre interface utilisateur fournissent déjà la prise en charge d’Automation d’interface utilisateur. Vous pouvez dériver de classes non verrouillées existantes pour définir un nouveau genre de contrôle d’interface utilisateur ou de classe de prise en charge. Au cours de ce processus, il se peut que votre classe ajoute un comportement qui doit offrir une prise en charge d’accessibilité non gérée par la prise en charge UI Automation par défaut. Dans ce cas, vous devez étendre la prise en charge d’Automation d’interface utilisateur existante en dérivant de la classe [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) utilisée par l’implémentation de base, en ajoutant la prise en charge nécessaire à votre implémentation d’homologue et en informant l’infrastructure de contrôle d’application Windows qu’elle doit créer votre homologue.

UI Automation permet de bénéficier non seulement d’applications d’accessibilité et de technologies d’assistance telles que les lecteurs d’écran, mais aussi du code (test) d’assurance qualité. Quel que soit le scénario, les clients UI Automation peuvent examiner les éléments d’interface utilisateur et simuler l’interaction utilisateur avec votre application à partir d’un autre code hors de votre application. Pour plus d’informations sur UI Automation sur toutes les plateformes et dans son sens le plus large, voir [Vue d’ensemble d’UI Automation](/windows/desktop/WinAuto/uiauto-uiautomationoverview).

Il existe deux publics distincts d’utilisateurs de l’infrastructure UI Automation.

* Les ***clients* UI Automation** appellent les API UI Automation pour en savoir plus sur toute l’interface utilisateur actuellement affichée pour l’utilisateur. Par exemple, une technologie d’assistance telle qu’un lecteur d’écran fait office de client UI Automation. L’interface utilisateur se présente sous la forme d’une arborescence d’éléments d’automation liés les uns aux autres. Le client UI Automation peut s’intéresser à une seule application à la fois ou à l’arborescence tout entière. Le client UI Automation peut se servir des API UI Automation pour parcourir l’arborescence ou bien consulter ou modifier des informations dans les éléments d’automation.
* Les ***fournisseurs* UI Automation** apportent des informations à l’arborescence UI Automation en implémentant des API qui exposent les éléments de l’interface utilisateur qu’ils ont introduits dans le cadre de leur application. Lorsque vous créez un contrôle, vous devez à présent agir en tant que participant au scénario du fournisseur UI Automation. En tant que fournisseur, vous devez vous assurer que tous les clients UI Automation peuvent utiliser l’infrastructure UI Automation pour interagir avec votre contrôle à des fins à la fois d’accessibilité et de test.

En règle générale, l’infrastructure UI Automation est dotée d’API parallèles : une API pour les clients UI Automation et une autre du même nom pour les fournisseurs UI Automation. Cette rubrique décrit, en grande partie, les API dédiées au fournisseur UI Automation, tout particulièrement les classes et interfaces qui permettent l’extensibilité du fournisseur dans cette infrastructure de l’interface utilisateur. Occasionnellement, nous citons les API UI Automation utilisées par les clients UI Automation pour offrir autre un point de vue, ou bien nous proposons une table de recherche qui met en corrélation les API des clients avec celles des fournisseurs. Pour en savoir plus sur le point de vue client, consultez le [Guide de programmeur du client UI Automation](/windows/desktop/WinAuto/uiauto-clientportal).

> [!NOTE]
> En règle générale, les clients UI Automation n’utilisent pas le code managé et ne sont pas implémentés en tant qu’applications UWP (il s’agit généralement d’applications de bureau). UI Automation est basé sur une norme et non sur une implémentation ni une infrastructure spécifique. De nombreux clients UI Automation existants, y compris des technologies d’assistance comme les lecteurs d’écran, utilisent des interfaces COM (Component Object Model) pour interagir avec UI Automation, le système et les applications exécutées dans les fenêtres enfants. Pour en savoir plus sur les interfaces COM et l’écriture d’un client UI Automation utilisant COM, voir [Notions de base d’UI Automation](/windows/desktop/WinAuto/entry-uiautocore-overview).

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Détermination de l’état existant de la prise en charge UI Automation pour votre classe d’interface utilisateur personnalisée  
Avant d’essayer de mettre en œuvre un homologue d’automation pour un contrôle personnalisé, vous devez vérifier si la classe de base et son homologue d’automation fournissent déjà l’accessibilité ou la prise en charge d’automation dont vous avez besoin. Dans de nombreux cas, la combinaison des implémentations [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer), des homologues spécifiques et des modèles qu’ils mettent en œuvre peut contribuer à une expérience d’accessibilité de niveau basique mais satisfaisante. Tout dépend de la quantité des modifications que vous avez apportées à l’exposition du modèle objet de votre contrôle par rapport à sa classe de base. Cela varie également selon que vos ajouts à la fonctionnalité de classe de base sont en corrélation avec de nouveaux éléments d’interface utilisateur dans le contrat modèle ou avec l’apparence visuelle du contrôle. Dans certains cas, vos modifications peuvent introduire de nouveaux aspects d’expérience utilisateur qui exigent une prise en charge supplémentaire de l’accessibilité.

Même si la classe homologue de base existante prend en charge l’accessibilité à un niveau basique, il est toujours préférable de définir un homologue pour communiquer des informations **ClassName** précises à UI Automation dans des scénarios de tests automatisés. Cet élément est particulièrement important si vous écrivez un contrôle destiné à un usage tiers.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Classes homologues d’automation  
UWP repose sur des techniques et conventions UI Automation existantes employées par des infrastructures d’interface utilisateur de code managé précédentes, telles que Windows Forms, Windows Presentation Foundation (WPF) et Microsoft Silverlight. Une grande partie des classes de contrôle et de leurs fonctions et rôles ont également pour origine une infrastructure d’interface utilisateur antérieure.

Par convention, les noms de classes homologues commencent par le nom de la classe de contrôle et se terminent par « AutomationPeer ». Par exemple, [**ButtonAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.ButtonAutomationPeer) est la classe homologue pour la classe de contrôle [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) .

> [!NOTE]
> Pour les besoins de cette rubrique, nous traitons les propriétés relatives à l’accessibilité comme des éléments plus importants lorsque vous implémentez un homologue de contrôle. Toutefois, pour obtenir un concept plus général de la prise en charge UI Automation, vous devez implémenter un homologue selon les recommandations qui figurent dans le [Guide de programmeur du fournisseur UI Automation](/windows/desktop/WinAuto/uiauto-providerportal) et dans [Notions de base d’UI Automation](/windows/desktop/WinAuto/entry-uiautocore-overview). Ces rubriques n’abordent pas les API [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) spécifiques que vous utilisez habituellement pour fournir les informations dans l’infrastructure UWP pour UI Automation, mais elles décrivent les propriétés qui identifient votre classe ou apportent d’autres informations ou bien proposent d’autres interactions.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Homologues, modèles et types de contrôle  
Un *modèle de contrôle* est une implémentation d’interface qui expose un aspect particulier des fonctionnalités d’un contrôle à un client UI Automation. Les clients UI Automation utilisent les propriétés et méthodes exposées par le biais d’un modèle de contrôle pour extraire des informations sur les fonctionnalités d’un contrôle ou pour manipuler son comportement au moment de l’exécution.

Les modèles de contrôle permettent de catégoriser et d'exposer les fonctionnalités d'un contrôle, indépendamment du type de contrôle ou de l'apparence du contrôle. Par exemple, un contrôle qui présente une interface sous forme de tableau utilise un modèle de contrôle **Grid** pour exposer le nombre de lignes et de colonnes du tableau et pour permettre à un client UI Automation d’extraire des éléments du tableau. Pour citer d’autres exemples, le client UI Automation peut avoir recours au modèle de contrôle **Invoke** pour les contrôles qu’il est possible d’appeler (des boutons, par exemple) et au modèle de contrôle **Scroll** pour les contrôles munis de barres de défilement, telles que des zones de liste, des affichages Liste ou des zones de liste modifiable. Chaque modèle de contrôle représente un type de fonctionnalité distinct et les modèles de contrôle peuvent être combinés pour décrire le jeu complet de fonctionnalités prises en charge par un contrôle donné.

Les modèles de contrôle sont à l’interface utilisateur ce que les interfaces sont aux objets COM. Dans COM, vous pouvez interroger un objet et lui demander quelles interfaces il prend en charge, puis vous servir de ces interfaces pour accéder aux fonctionnalités. Dans UI Automation, les clients UI Automation peuvent interroger un élément UI Automation sur les modèles de contrôle qu’il prend en charge, puis interagir avec l’élément et son contrôle homologue via des propriétés, des méthodes, des événements et des structures exposés par les modèles de contrôle pris en charge.

L’une des principales fonctions d’un homologue d’automation consiste à indiquer à un client UI Automation les modèles de contrôle que l’élément d’interface utilisateur peut prendre en charge via son homologue. Pour ce faire, les fournisseurs UI Automation implémentent de nouveaux homologues qui modifient le comportement de la méthode [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) en substituant la méthode [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) . Les clients UI Automation effectuent des appels que le fournisseur UI Automation mappe à la méthode d’appel **GetPattern** . Les clients UI Automation demandent chaque modèle spécifique avec lequel ils veulent interagir. Si l’homologue prend en charge le modèle, il retourne une référence d’objet à lui-même ; sinon, il retourne la valeur **null** . Si la valeur renvoyée n’est pas **null** , le client UI Automation s’attend à pouvoir appeler les API de l’interface de modèle en tant que client, pour pouvoir interagir avec ce modèle de contrôle.

Un *type de contrôle* est un moyen de définir globalement la fonctionnalité d’un contrôle représenté par l’homologue. Ce concept est différent de celui d’un modèle de contrôle. En effet, un modèle signale à UI Automation les informations qu’il peut obtenir ou les actions qu’il peut effectuer via une interface particulière, le type de contrôle se situe un niveau au-dessus. Chaque type de contrôle est associé à des instructions sur ces aspects d’UI Automation :

* Modèles de contrôle UI Automation : un type de contrôle peut prendre en charge plusieurs modèles, chacun d’entre eux représentant une classification d’informations ou une interaction différente. Chaque type de contrôle possède un jeu de modèles de contrôle que le contrôle doit prendre en charge, un jeu facultatif et un jeu que le contrôle ne doit pas prendre en charge.
* Valeurs de propriété UI Automation : chaque type de contrôle possède un jeu de propriétés que le contrôle doit prendre en charge. Ces propriétés sont d’ordre général, comme décrit dans [Vue d’ensemble des propriétés UI Automation](/windows/desktop/WinAuto/uiauto-propertiesoverview), et non spécifiques au modèle.
* Événements UI Automation : chaque type de contrôle possède un jeu d’événements que le contrôle doit prendre en charge. Là encore, ces événements, décrits dans la section [Vue d’ensemble des propriétés UI Automation](/windows/desktop/WinAuto/uiauto-eventsoverview), sont d’ordre général.
* Structure de l’arborescence UI Automation : chaque type de contrôle définit la façon dont le contrôle doit apparaître dans la structure de l’arborescence UI Automation.

Indépendamment de la manière dont les homologues d’automatisation de l’infrastructure sont implémentés, la fonctionnalité de client UI Automation n’est pas liée à UWP. En réalité, les clients UI Automation existants, comme les technologies d’assistance, vont probablement utiliser d’autres modèles de programmation, par exemple COM. Dans ce type de modèle, les clients peuvent effectuer une action **QueryInterface** pour l’interface de modèle de contrôle COM qui implémente le modèle demandé ou l’infrastructure UI Automation générale à des fins d’examen des propriétés, des événements ou de l’arborescence. Pour les modèles, l’infrastructure UI Automation assemble ce code d’interface avec le code UWP exécuté sur le fournisseur UI Automation de l’application et l’homologue approprié.

Quand vous implémentez des modèles de contrôle pour une infrastructure de code managé telle qu’une application UWP à l’aide de C \# ou de Microsoft Visual Basic, vous pouvez utiliser des interfaces .NET Framework pour représenter ces modèles au lieu d’utiliser la représentation de l’interface com. Par exemple, l’interface de modèle UI Automation pour une implémentation de fournisseur Microsoft .NET du modèle **Invoke** est [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider).

Pour obtenir une liste de modèles de contrôle, d’interfaces de fournisseur et de leurs fonctions, voir [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md). Pour obtenir la liste des types de contrôle, voir [Vue d’ensemble des types de contrôle d’UI Automation](/windows/desktop/WinAuto/uiauto-controltypesoverview).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Instructions relatives à l’implémentation des modèles de contrôle  
Les modèles de contrôle et ce à quoi ils sont destinés font partie d’une plus grande définition de l’infrastructure UI Automation et ne s’appliquent pas uniquement à la prise en charge de l’accessibilité pour une application UWP. Lorsque vous implémentez un modèle de contrôle, vous devez vous assurer que vous l’implémentez de manière à ce qu’il corresponde aux instructions décrites dans ces documents et également dans la spécification UI Automation. Si vous recherchez de l’aide, vous pouvez généralement utiliser la documentation de Microsoft et ne pas avoir à vous référer à la spécification. Les instructions pour chaque modèle sont documentées dans [Implémentation de modèles de contrôle UI Automation](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns). Notez que chaque rubrique sous cette zone contient une section « Instructions et conventions d’implémentation », ainsi qu’une section « Membres requis ». Les instructions font généralement référence à des API spécifiques de l’interface du modèle de contrôle appropriée dans le document de référence [Interfaces des modèles de contrôle pour fournisseurs](/windows/desktop/WinAuto/uiauto-cpinterfaces). Ces interfaces correspondent aux interfaces natives/COM (et leurs API utilisent une syntaxe de style COM). Cependant, tous les éléments que vous voyez ont un équivalent dans l’espace de noms [**Windows.UI.Xaml.Automation.Provider**](/uwp/api/Windows.UI.Xaml.Automation.Provider).

Si vous utilisez les homologues d’automatisation par défaut et étendez leur comportement, la rédaction de ces homologues répond déjà aux instructions d’UI Automation. S’ils prennent en charge les modèles de contrôle, vous pouvez être certain que la prise en charge des modèles est conforme aux instructions fournies dans la section [Implémentation de modèles de contrôle UI Automation](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns). Si un homologue de contrôle signale qu’il est représentatif d’un type de contrôle défini par UI Automation, les instructions documentées dans la section [Prise en charge des types de contrôle UI Automation](/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) sont alors respectées par cet homologue.

Néanmoins, vous aurez peut-être besoin d’instructions supplémentaires concernant les modèles ou les types de contrôle afin de respecter les recommandations d’UI Automation lors de votre implémentation d’homologue. Ceci est particulièrement vrai si vous implémentez un modèle ou un type de contrôle qui n’existe pas encore en tant qu’implémentation par défaut dans un contrôle UWP. Par exemple, le modèle pour les annotations n’est implémenté dans aucun contrôle XAML par défaut. Mais il est possible qu’une application utilise intensivement des annotations. Dans ce cas, vous devez mettre en évidence cette fonctionnalité afin qu’elle soit accessible. Dans ce scénario, votre homologue doit implémenter [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) et probablement se signaler en tant que type de contrôle **Document** avec des propriétés appropriées afin d’indiquer que vos documents prennent en charge les annotations.

Nous vous recommandons d’utiliser les instructions disponibles pour les modèles sous [Implémentation des modèles de contrôle UI Automation](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) ou les types de contrôle sous [Prise en charge des types de contrôle UI Automation](/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) afin d’obtenir des directions et des instructions d’ordre général. Vous pouvez même essayer de cliquer sur certains liens d’API pour obtenir des descriptions et des remarques relatives à la nature de chaque API. Toutefois, pour les caractéristiques de syntaxe nécessaires à la programmation d’applications UWP, recherchez l’API équivalente dans l’espace de noms [**Windows. UI. Xaml. Automation. Provider**](/uwp/api/Windows.UI.Xaml.Automation.Provider) et utilisez ces pages de référence pour plus d’informations.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Classes homologues d’automation intégrées  
En règle générale, les éléments implémentent une classe homologue d’automation s’ils acceptent une activité d’interface utilisateur de l’utilisateur ou s’ils contiennent des informations nécessaires aux utilisateurs de technologies d’assistance représentant l’interface utilisateur interactive ou significative des applications. Les éléments visuels UWP n’ont pas tous des homologues d’automation. Les exemples de classes qui implémentent des homologues Automation sont [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) et [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox). Les classes [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) et les classes basées sur [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) (telles que [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid) et [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas)) sont des exemples de classes qui ne mettent pas en œuvre d’homologues d’automatisation. Un élément **Panel** n’a aucun homologue, car il fournit un comportement de la disposition qui est uniquement visuel. Il n’existe, pour l’utilisateur, aucun moyen pertinent concernant l’accessibilité d’interagir avec un élément **Panel** . Tous les éléments enfants qu’un élément **Panel** contient sont en revanche signalés aux arborescences UI Automation en tant qu’éléments enfants du prochain parent disponible dans l’arborescence qui dispose d’un homologue ou d’une représentation d’élément.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>Limites de processus UI Automation et UWP  
En règle générale, le code client UI Automation qui accède à une application UWP s’exécute hors processus. L’infrastructure UI Automation permet de communiquer des informations au-delà des limites du processus. Ce concept est présenté de manière plus détaillée dans la rubrique [Notions de base d’UI Automation](/windows/desktop/WinAuto/entry-uiautocore-overview).

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Toutes les classes qui dérivent de [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) contiennent la méthode virtuelle protégée [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer). La séquence d’initialisation d’objet pour les homologues d’automatisation appelle **OnCreateAutomationPeer** pour se procurer l’objet homologue d’automatisation de chaque contrôle et créer ainsi une arborescence UI Automation à utiliser au moment de l’exécution. Le code UI Automation peut utiliser l’homologue pour obtenir des informations sur les caractéristiques et fonctionnalités d’un contrôle et pour simuler l’utilisation interactive au moyen de ses modèles de contrôle. Un contrôle personnalisé qui prend en charge l’automatisation doit substituer **OnCreateAutomationPeer** et renvoyer une instance d’une classe qui dérive de [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer). Par exemple, si un contrôle personnalisé est dérivé de la classe [**ButtonBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase), l’objet renvoyé par **OnCreateAutomationPeer** doit dériver de [**ButtonBaseAutomationPeer**](/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer).

Si vous écrivez une classe de contrôle personnalisé et envisagez de fournir également un nouvel homologue d’automatisation, vous devez remplacer la méthode [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) de votre contrôle personnalisé de sorte qu’il retourne une nouvelle instance de votre homologue. Votre classe pair doit dériver directement ou indirectement de [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer).

Par exemple, le code qui suit déclare que le contrôle personnalisé `NumericUpDown` doit utiliser l’homologue `NumericUpDownPeer` pour les besoins de UI Automation.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> L’implémentation de [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) ne doit faire rien de plus que d’initialiser une nouvelle instance de votre homologue Automation personnalisé, en passant le contrôle appelant comme propriétaire et en retournant cette instance. Ne tentez pas une logique supplémentaire dans cette méthode. En particulier, toute logique qui peut éventuellement aboutir à la destruction de [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) dans le même appel peut entraîner un comportement inattendu lors de l’exécution.

Dans des implémentations classiques de [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer), le *owner* est précisé sous la forme **this** ou **Me** puisque la substitution de méthode intervient dans la même étendue que le reste de la définition de classe de contrôle.

La définition de classe homologue réelle peut survenir dans le même fichier de code que le contrôle ou dans un fichier de code distinct. Les définitions homologues existent toutes dans l’espace de noms [**Windows. UI. Xaml. Automation. Peers**](/uwp/api/Windows.UI.Xaml.Automation.Peers) qui est un espace de noms distinct des contrôles pour lesquels ils fournissent des homologues. Vous pouvez également choisir de déclarer vos pairs dans des espaces de noms distincts, à condition que vous référençiez les espaces de noms nécessaires pour l’appel de la méthode [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) .

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Choix de la classe de base d’homologue appropriée  
Assurez-vous que votre [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) est dérivé d’une classe de base qui vous donne la meilleure correspondance pour la logique d’homologue existante de la classe de contrôle à partir de laquelle vous dérivez. Dans le cas de l’exemple précédent, étant donné que `NumericUpDown` dérive de [**RangeBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase), une classe [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) est disponible pour laquelle vous devez baser votre homologue. En utilisant la classe homologue correspondante la plus proche en parallèle à la manière dont vous dérivez le contrôle lui-même, vous pouvez au moins éviter de remplacer certaines des fonctionnalités [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) puisque la classe homologue de base les a déjà implémentées.

La classe de [**contrôle**](/uwp/api/Windows.UI.Xaml.Controls.Control) de base n’a pas de classe homologue correspondante. Si vous avez besoin de faire correspondre une classe homologue à un contrôle personnalisé dérivé de **Control** , dérivez la classe homologue personnalisée de [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer).

Si vous dérivez directement de [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl), cette classe ne présente aucun comportement d’homologue d’automatisation par défaut dans la mesure où aucune implémentation [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) référençant une classe homologue n’existe. Assurez-vous soit d’implémenter **OnCreateAutomationPeer** pour utiliser votre propre homologue, soit d’utiliser [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) en guise d’homologue si ce niveau de prise en charge de l’accessibilité convient pour votre contrôle.

> [!NOTE]
> En général, vous ne dérivez pas de [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) au lieu de [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer). Si vous avez dérivé directement de **AutomationPeer** , vous devez dupliquer une grande partie de la prise en charge de l’accessibilité de base qui, autrement, proviendrait de **FrameworkElementAutomationPeer** .

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Initialisation d’une classe homologue personnalisée  
L’homologue d’automatisation doit définir un constructeur de type sécurisé qui utilise une instance du contrôle propriétaire pour l’initialisation de base. Dans l’exemple qui suit, l’implémentation transmet la valeur *owner* à la base [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) et, en fin de compte, c’est le [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) qui utilise *owner* pour définir [**FrameworkElementAutomationPeer.Owner**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner).

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Méthodes principales d’AutomationPeer  
Pour des raisons liées à l’infrastructure UWP, les méthodes substituables d’un homologue d’automatisation font partie d’une paire de méthodes : la méthode d’accès publique que le fournisseur UI Automation utilise comme point de transfert pour les clients UI Automation, et la méthode de personnalisation « Core » protégée qu’une classe UWP peut remplacer pour influencer le comportement. La paire de méthodes est liée par défaut pour que l’appel à la méthode d’accès appelle toujours la méthode « Core » parallèle qui a l’implémentation de fournisseur ou, en guise de solution de secours, appelle une implémentation par défaut à partir des classes de base.

Au moment d’implémenter un homologue pour un contrôle personnalisé, remplacez toutes les méthodes « Core » à partir de la classe homologue d’automatisation de base dans laquelle vous souhaitez exposer le comportement qui est unique à votre contrôle personnalisé. Le code UI Automation obtient des informations sur votre contrôle en appelant des méthodes publiques de la classe homologue. Pour fournir des informations sur votre contrôle, remplacez chaque méthode par un nom se terminant par « Core » lorsque votre implémentation et structure de contrôle créent des scénarios d’accessibilité ou d’autres scénarios UI Automation qui diffèrent de ce que la classe homologue d’automatisation de base prend en charge.

Au minimum, chaque fois que vous définissez une nouvelle classe pair, implémentez la méthode [**GetClassNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) , comme indiqué dans l’exemple suivant.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Vous pouvez, si vous le souhaitez, stocker les chaînes sous forme de constantes plutôt que directement dans le corps de méthode. Pour [**GetClassNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), vous n’avez pas besoin de localiser cette chaîne. La propriété **LocalizedControlType** est utilisée chaque fois qu’une chaîne localisée est requise par un client UI Automation, et non **ClassName** .

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Certaines technologies d’assistance utilisent la valeur [**GetAutomationControlType**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) directement lors du signalement des caractéristiques des éléments dans une arborescence UI Automation, comme des informations supplémentaires au-delà de **Name** UI Automation. Si votre contrôle est très différent du contrôle duquel vous dérivez et que vous voulez signaler un type de contrôle différent de celui indiqué par la classe homologue de base utilisée par le contrôle, vous devez implémenter un homologue et remplacer [**GetAutomationControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) dans votre implémentation d’homologue. Cela est particulièrement important si vous dérivez d’une classe de base généralisée telle que [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) ou [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl), où l’homologue de base ne fournit pas d’informations précises sur le type de contrôle.

Votre implémentation de [**GetAutomationControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) décrit votre contrôle en retournant une valeur [**AutomationControlType**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) . Bien qu’il soit possible de renvoyer **AutomationControlType.Custom** , vous devez renvoyer l’un des types de contrôles plus spécifiques s’il décrit de manière précise les principaux scénarios de votre contrôle. Voici un exemple.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> Sauf si vous spécifiez [**AutomationControlType.Custom**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType), vous n’avez pas à implémenter [**GetLocalizedControlTypeCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) pour fournir une valeur de propriété **LocalizedControlType** aux clients. L’infrastructure UI Automation courante propose des chaînes traduites pour toutes les valeurs **AutomationControlType** possibles autres que **AutomationControlType.Custom** .

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern et GetPatternCore  
L’implémentation d’un homologue de [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) retourne l’objet qui prend en charge le modèle demandé dans le paramètre d’entrée. Plus précisément, un client UI Automation appelle une méthode qui est transmise à la méthode [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) du fournisseur et spécifie une valeur d’énumération [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) qui nomme le modèle demandé. Votre substitution de **GetPatternCore** doit renvoyer l’objet qui met en œuvre le modèle spécifié. Cet objet est l’homologue lui-même, car l’homologue doit implémenter l’interface de modèle correspondante chaque fois qu’il signale qu’il prend en charge un modèle. Si votre homologue ne propose aucune implémentation personnalisée pour un modèle mais que vous savez que la base de l’homologue implémente véritablement le modèle, vous pouvez appeler l’implémentation de la méthode **GetPatternCore** du type de base de votre **GetPatternCore** . La méthode **GetPatternCore** d’un homologue doit renvoyer une valeur **null** si un modèle n’est pas pris en charge par l’homologue. En revanche, plutôt que de renvoyer une valeur **null** directement depuis votre implémentation, vous devez généralement vous appuyer sur l’appel à l’implémentation de base pour renvoyer une valeur **null** pour tous les modèles non pris en charge.

Lorsqu’un modèle est pris en charge, l’implémentation de [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) peut renvoyer une valeur **this** ou **Me** . Le client UI Automation doit alors normalement effectuer un cast de la valeur de retour [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) vers l’interface de modèle demandée chaque fois qu’elle n’affiche pas la valeur **null** .

Si une classe homologue hérite d’un autre homologue et si tous les signalements de modèle et de prise en charge nécessaires sont déjà gérés par la classe de base, l’implémentation de [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) n’est pas utile. Par exemple, si vous implémentez un contrôle de plage qui dérive de [**RangeBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase), et que votre homologue dérive de [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer), cet homologue se retourne lui-même pour [**patternInterface. RangeValue**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) et a des implémentations opérationnelles de l’interface [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) qui prend en charge le modèle.

Bien qu’il ne s’agisse pas de code littéral, cet exemple ressemble à peu près à l’implémentation de [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) déjà présente dans [**RangeBaseAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

Si vous implémentez un homologue dans lequel vous ne disposez pas de toute la prise en charge dont vous avez besoin à partir d’une classe homologue de base, ou que vous souhaitez modifier ou ajouter à l’ensemble de modèles hérités de base que votre homologue peut prendre en charge, vous devez remplacer [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pour permettre aux clients UI Automation d’utiliser les modèles.

Pour obtenir la liste des modèles de fournisseur disponibles dans l’implémentation UWP de la prise en charge d’UI Automation, consultez [**Windows. UI. Xaml. Automation. Provider**](/uwp/api/Windows.UI.Xaml.Automation.Provider). Chacun de ces modèles a une valeur correspondante de l’énumération [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface), ce qui permet aux clients UI Automation de demander le modèle dans un appel [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern).

Un homologue peut signaler qu’il prend en charge plusieurs modèles. Dans ce cas, la substitution doit inclure la logique de chemin d’accès de retour pour chaque valeur [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) prise en charge et renvoyer l’homologue dans chaque cas correspondant. L’appelant doit généralement ne demander qu’une seule interface à la fois et il lui incombe d’effectuer un cast vers l’interface attendue.

Voici un exemple de substitution de [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pour un homologue personnalisé. Il indique la prise en charge de deux modèles, [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) et [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider). Il s’agit ici d’un contrôle d’affichage multimédia qui peut s’afficher en plein écran (mode bascule) et qui possède une barre de progression dans laquelle les utilisateurs peuvent sélectionner une position (contrôle de plage). Ce code provient de l’[exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>Transfert de modèles à partir de sous-éléments  
Une implémentation de méthode [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) peut également spécifier un sous-élément ou un composant en tant que fournisseur de modèles pour son hôte. Cet exemple imite la manière dont [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) transfère la gestion des modèles de défilement à l’homologue de son contrôle [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) interne. Pour spécifier un sous-élément pour la gestion des modèles, ce code obtient l’objet sous-élément, crée un homologue pour le sous-élément à l’aide de la méthode [**FrameworkElementAutomationPeer.CreatePeerForElement**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement), puis renvoie le nouvel homologue.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>Autres méthodes principales  
Il est possible que votre contrôle doive prendre en charge des équivalents clavier pour les principaux scénarios. Pour plus d’informations sur les raisons d’une telle nécessité, voir [Accessibilité du clavier](keyboard-accessibility.md). L’implémentation de la prise en charge des clés fait obligatoirement partie du code de contrôle et non du code homologue, car elle fait partie de la logique d’un contrôle, mais votre classe homologue doit substituer les méthodes [**GetAcceleratorKeyCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) et [**GetAccessKeyCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) pour signaler aux clients UI Automation les clés qui sont utilisées. Retenez que les chaînes qui signalent des informations de ce type devront peut-être localisées et doivent par conséquent provenir de ressources, et non de chaînes codées en dur.

Si vous fournissez un homologue pour une classe qui prend en charge une collection, il est préférable de dériver à la fois des classes fonctionnelles et des classes homologues qui offrent déjà ce genre de prise en charge de collection. Si vous ne le faites pas, les homologues pour les contrôles qui maintiennent des collections enfants devront peut-être substituer la méthode homologue associée à la collection [**GetChildrenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) pour signaler correctement les relations parent-enfant à l’arborescence UI Automation.

Implémentez les méthodes [**IsContentElementCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) et [**IsControlElementCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) pour indiquer si votre contrôle contient du contenu de données ou s’il respecte un rôle interactif dans l’interface utilisateur (ou les deux). Par défaut, les deux méthodes renvoient la valeur **true** . Ces paramètres améliorent la simplicité d’utilisation des technologies d’assistance telles que les lecteurs d’écran, qui peuvent avoir recours à ces méthodes pour filtrer l’arborescence d’automatisation. Si votre méthode [**GetPatternCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) transfère la gestion des modèles vers un homologue de sous-élément, la méthode **IsControlElementCore** de ce dernier peut renvoyer la valeur **false** afin de masquer l’homologue de sous-élément dans l’arborescence d’automatisation.

Certains contrôles peuvent prendre en charge des scénarios d’étiquetage dans lesquels une étiquette de texte apporte des informations sur une partie non textuelle ou un contrôle entretient une relation d’étiquetage connue avec un autre contrôle dans l’interface utilisateur. S’il est possible de fournir un comportement utile basé sur les classes, vous pouvez substituer [**GetLabeledByCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) pour fournir ce comportement.

[**GetBoundingRectangleCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) et [**GetClickablePointCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) sont principalement utilisés pour les scénarios de test automatisé. Si vous voulez prendre en charge les tests automatisés pour votre contrôle, vous pouvez remplacer ces méthodes. Cela peut être souhaitable pour les contrôles de type plage, où vous ne pouvez pas suggérer juste un seul point, car l’endroit où l’utilisateur clique dans l’espace de coordonnées a un effet différent sur une plage. Par exemple, l’homologue d’automatisation [**ScrollBar**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar) par défaut remplace **GetClickablePointCore** pour retourner une valeur [**Point**](/uwp/api/Windows.Foundation.Point) « n’est pas un nombre ».

[**GetLiveSettingCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) influence la valeur par défaut du contrôle pour la valeur **LiveSetting** pour UI Automation. Vous souhaiterez peut-être la remplacer si vous souhaitez que votre contrôle retourne une valeur autre que [**AutomationLiveSetting. OFF**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationLiveSetting). Pour plus d’informations sur ce que **LiveSetting** représente, voir [**AutomationProperties.LiveSetting**](/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty).

Vous pouvez substituer [**GetOrientationCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) si votre contrôle a une propriété d’orientation définissable qui peut être mappée à [**AutomationOrientation**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationOrientation). Les classes [**ScrollBarAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.ScrollBarAutomationPeer) et [**SliderAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.SliderAutomationPeer) effectuent cette opération.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Implémentation de base dans FrameworkElementAutomationPeer  
L’implémentation de base de [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) fournit des informations UI Automation qui peuvent être interprétées à partir de différentes propriétés de disposition et de comportement qui sont définies au niveau de l’infrastructure.

* [**GetBoundingRectangleCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): retourne une structure [**Rect**](/uwp/api/Windows.Foundation.Rect) basée sur les caractéristiques de disposition connues. Renvoie une valeur nulle 0 **Rect** si [**IsOffscreen**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) affiche la valeur **true** .
* [**GetClickablePointCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) : renvoie une structure [**Point**](/uwp/api/Windows.Foundation.Point) fondée sur les caractéristiques de disposition connues, tant qu’une valeur **BoundingRectangle** différente de zéro est disponible.
* [**GetNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore): comportement plus étendu que celui qui peut être résumé ici ; consultez [**GetNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore). En bref, cette méthode tente de convertir les chaînes du contenu connu d’une classe [**ContentControl**](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) ou de classes associées dotées d’un contenu. De même, si une valeur est disponible pour [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)), la valeur **Name** de cet élément est utilisée comme élément **Name** .
* [**HasKeyboardFocusCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): évaluée en fonction des propriétés [**FocusState**](/uwp/api/windows.ui.xaml.controls.control.focusstate) et [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) du propriétaire. Les éléments qui ne sont pas des contrôles renvoient toujours la valeur **false** .
* [**IsEnabledCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): évaluée en fonction de la propriété [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) du propriétaire s’il s’agit d’un [**contrôle**](/uwp/api/Windows.UI.Xaml.Controls.Control). Les éléments qui ne sont pas des contrôles renvoient toujours la valeur **true** . Cela ne signifie pas que le propriétaire est activé dans le sens de l’interaction classique, mais que l’homologue est activé même si le propriétaire n’a pas de propriété **IsEnabled** .
* [**IsKeyboardFocusableCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore) : retourne **true** si le propriétaire est un [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) ; sinon, retourne **false** .
* [**IsOffscreenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore) : une propriété [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) de type [**Collapsed**](/uwp/api/windows.ui.xaml.visibility) dans l’élément propriétaire ou l’un de ses parents équivaut à une valeur **true** pour [**IsOffscreen**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Exception : un objet [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) peut être visible même si les parents de son propriétaire ne le sont pas.
* [**SetFocusCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): appelle [**le focus**](/uwp/api/windows.ui.xaml.controls.control.focus).
* [**GetParent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent): appelle [**FrameworkElement. parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent) à partir du propriétaire et recherche l’homologue approprié. Comme il ne s’agit pas d’une paire de substitution avec une méthode « Core », vous ne pouvez pas modifier ce comportement.

> [!NOTE]
> Les homologues UWP par défaut implémentent un comportement à l’aide d’un code natif interne chargé d’implémenter UWP, et pas nécessairement en faisant appel à un code UWP réel. Vous ne serez pas en mesure de consulter le code ou la logique de cette implémentation via une technique de réflexion CLR (Common Language Runtime) ou d’autres techniques. Vous ne verrez pas non plus de pages de référence distinctes pour les substitutions de sous-classes du comportement d’homologue de base. Par exemple, il se peut qu’il existe un autre comportement pour la méthode [**GetNameCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore) d’une classe [**TextBoxAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer) qui ne sera pas décrite dans la page de référence **AutomationPeer.GetNameCore** et il n’y a pas de page de référence pour **TextBoxAutomationPeer.GetNameCore** . Il n’existe même pas de page de référence **TextBoxAutomationPeer.GetNameCore** . Lisez plutôt la rubrique de référence pour la classe homologue la plus immédiate et recherchez des notes d’implémentation dans la section Remarques.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Homologues et AutomationProperties  
Votre homologue d’automation doit fournir des valeurs par défaut appropriées pour les informations liées à l’accessibilité de votre contrôle. Notez que tout code d’application qui utilise le contrôle peut remplacer certains de ce comportement en incluant des valeurs de propriété attachée [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) sur les instances de contrôle. Les appelants peuvent procéder ainsi pour les contrôles par défaut ou pour les contrôles personnalisés. Par exemple, le code XAML suivant crée un bouton qui a deux propriétés UI Automation personnalisées : `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Pour plus d’informations sur les propriétés jointes de [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties), voir [Informations d’accessibilité élémentaires](basic-accessibility-information.md).

Certaines des méthodes [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) existent en raison de la manière générale dont les fournisseurs UI Automation doivent transmettre des informations mais ces méthodes ne sont généralement pas implémentées dans les homologues de contrôle. Cela est dû au fait que les informations doivent être fournies par les valeurs [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) appliquées au code d’application qui utilise les contrôles dans une interface utilisateur spécifique. Par exemple, pour définir la relation d’étiquetage entre les deux contrôles différents au sein de l’interface utilisateur, la plupart des applications appliqueraient une valeur [**AutomationProperties.LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)). Toutefois, [**LabeledByCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) est implémenté dans certains pairs qui représentent des relations de données ou d’éléments dans un contrôle, telles que l’utilisation d’une partie d’en-tête pour étiqueter une partie de champ de données, l’étiquetage des éléments avec leurs conteneurs, ou des scénarios similaires.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>Implémentation de modèles  
Examinons comment écrire un homologue pour un contrôle chargé de mettre en œuvre un comportement de développement/réduction en procédant à l’implémentation de l’interface de modèle de contrôle pour les actions développer/réduire. L’homologue doit activer l’accessibilité pour le comportement Expand-collapse en se retournant à chaque fois que [**GetPattern**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) est appelé avec une valeur de [**patternInterface. ExpandCollapse**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface). L’homologue doit alors hériter de l’interface du fournisseur pour ce modèle ( [**IExpandCollapseProvider**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) et proposer des implémentations pour chacun des membres de cette même interface. Dans le cas présent, l’interface dispose de trois membres à substituer : [**Expand**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) et [**ExpandCollapseState**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

Il est préférable de planifier l’accessibilité au moment de concevoir l’API de la classe proprement dite. Lorsque vous êtes confronté à un comportement potentiellement requis soit dans le cadre d’interactions standard avec un utilisateur qui travaille dans l’interface utilisateur, soit par le biais d’un modèle de fournisseur d’automation, vous devez préciser une méthode unique que la réponse d’interface utilisateur ou le modèle d’automation peut appeler. Par exemple, si votre contrôle dispose de boutons dotés de gestionnaires d’événements connectés capables de développer ou de réduire le contrôle, ainsi que d’équivalents clavier pour ces mêmes actions, faites en sorte que ces gestionnaires d’événements appellent la même méthode que celle que vous avez appelée à partir des implémentations [**Expand**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand) ou [**Collapse**](/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) de l’interface [**IExpandCollapseProvider**](/windows/desktop/api/uiautomationcore/nn-uiautomationcore-iexpandcollapseprovider) au sein de l’homologue. Le recours à une méthode logique courante peut également être utile pour s’assurer que les états visuels de votre contrôle sont mis à jour afin d’afficher l’état logique de façon uniforme, quelle que soit la manière dont le comportement a été appelé.

Une implémentation classique est que les API du fournisseur appellent d’abord le [**propriétaire**](/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) pour accéder à l’instance de contrôle au moment de l’exécution. Les méthodes de comportement nécessaires peuvent ensuite être appelées dans cet objet.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Le contrôle lui-même qui peut référencer son homologue est une autre implémentation possible. Il s’agit d’un modèle courant si vous déclenchez des événements d’Automation à partir du contrôle, car la méthode [**RaiseAutomationEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) est une méthode homologue.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Événements UI Automation  

Les événements UI Automation s’inscrivent dans les catégories suivantes.

| Événement | Description |
|-------|-------------|
| Modification de propriété | Se déclenche quand une propriété dans un élément ou un modèle de contrôle UI Automation change. Par exemple, si un client a besoin de surveiller le contrôle de case à cocher d’une application, il peut s’inscrire pour écouter un événement de changement de propriété dans la propriété [**ToggleState**](/uwp/api/windows.ui.xaml.automation.provider.itoggleprovider.togglestate). Lorsque le contrôle de la case à cocher est activé ou désactivé, le fournisseur déclenche l’événement et le client peut agir selon ses besoins. |
| Action d'élément | Se déclenche quand un changement dans l’interface utilisateur résulte d’une activité menée par l’utilisateur ou par programme (par exemple, lorsque vous cliquez sur un bouton ou l’appelez par le biais du modèle **Invoke** ). |
| Modification de la structure | Se déclenche lorsque la structure de l’arborescence UI Automation change. La structure évolue quand de nouveaux éléments de l’interface utilisateur deviennent visibles, sont masqués ou sont supprimés sur le Bureau. |
| Changement global | Se déclenche lorsque des actions d’intérêt général pour le client sont menées, par exemple lorsque le focus passe d’un élément à un autre ou quand une fenêtre enfant se ferme. Certains événements ne signifient pas nécessairement que l'état de l'interface utilisateur a changé. Par exemple, si l’utilisateur passe à un champ d’entrée de texte, puis clique sur un bouton pour mettre à jour le champ, un événement [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) se déclenche même si l’utilisateur n’a pas réellement modifié le texte. Pendant le traitement d'un événement, une application cliente peut être amenée à vérifier ce qui a réellement changé avant d'agir. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>Identificateurs AutomationEvents  
Les événements UI Automation sont identifiés par des valeurs [**AutomationEvents**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationEvents). Les valeurs de l’énumération identifient le type d’événement de manière unique.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Déclenchement d'événements  
Les clients UI Automation peuvent s’abonner à des événements d’automation. Dans le modèle d’homologue d’automatisation, les homologues des contrôles personnalisés doivent signaler les changements de l’état du contrôle en rapport avec l’accessibilité en appelant la méthode [**RaiseAutomationEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent). De même, quand une valeur de propriété UI Automation clé change, les homologues de contrôles personnalisés doivent appeler la méthode [**RaisePropertyChangedEvent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent).

L’exemple de code suivant montre comment se procurer l’objet homologue à partir du code de définition de contrôle et comment appeler une méthode pour déclencher un événement à partir de cet homologue. À des fins d’optimisation, le code détermine s’il existe des écouteurs pour ce type d’événement. Le fait de déclencher l’événement et de créer l’objet homologue uniquement lorsqu’il y a des écouteurs permet d’éviter toute surcharge inutile et aide à maintenir la réactivité du contrôle.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>Navigation parmi les homologues  
Après avoir localisé un homologue Automation, un client UI Automation peut naviguer dans la structure homologue d’une application en appelant les méthodes [**GetChildren**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildren) et [**GetParent**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent) de l’objet homologue. La navigation parmi les éléments d’interface utilisateur dans un contrôle est prise en charge par l’implémentation de la méthode [**GetChildrenCore**](/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) par l’homologue. Le système UI Automation appelle cette méthode pour créer une arborescence des sous-éléments contenus dans un contrôle, par exemple les éléments de liste d’une zone de liste. La méthode **GetChildrenCore** par défaut dans [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) traverse l’arborescence visuelle des éléments pour créer l’arborescence d’homologues d’automatisation. Les contrôles personnalisés peuvent substituer cette méthode pour exposer une représentation différente d’éléments enfants aux clients d’automatisation, en renvoyant les homologues d’automatisation d’éléments qui transmettent des informations ou autorisent une interaction utilisateur.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Prise en charge de l’automation native pour les modèles de texte  
Certains des homologues d’automatisation d’application UWP par défaut offrent une prise en charge des modèles de contrôles pour le modèle de texte ( [**PatternInterface.Text**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)). Mais ils offrent cette prise en charge par le biais de méthodes natives, et les homologues impliqués ne noteront pas l’interface [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) dans l’héritage (managé). En outre, si un client UI Automation managé ou non managé demande des modèles à l’homologue, il signale la prise en charge du modèle de texte et fournit le comportement pour certaines parties du modèle quand les API clientes sont appelées.

Si vous envisagez de dériver de l’un des contrôles de texte des applications UWP et de créer également un homologue personnalisé qui dérive de l’un des homologues associés au texte, examinez les sections Remarques pour l’homologue pour en savoir plus sur toute prise en charge des modèles au niveau natif. Vous pouvez accéder au comportement de base natif dans votre homologue personnalisé si vous appelez l’implémentation de base à partir des implémentations de l’interface du fournisseur managé, mais il est difficile de modifier les actions de l’implémentation de base, car les interfaces natives sur l’homologue et son contrôle propriétaire ne sont pas exposées. En règle générale, vous devez utiliser les implémentations de base en l’état (appel de la base uniquement) ou remplacer entièrement la fonctionnalité par votre propre code managé et ne pas appeler l’implémentation de base. Ce dernier scénario étant plus abouti, vous devez disposer de bonnes connaissances sur l’infrastructure des services de texte utilisés par votre contrôle pour prendre en charge les exigences d’accessibilité lors de l’utilisation de cette infrastructure.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
En plus de fournir un homologue personnalisé, vous pouvez également ajuster la représentation d’arborescence pour toute instance de contrôle, en définissant la propriété [**AutomationProperties.AccessibilityView**](/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) en XAML. Elle n’est pas implémentée dans le cadre d’une classe homologue, mais nous la signalons ici car elle est apparentée à la prise en charge de l’accessibilité soit pour des contrôles personnalisés, soit pour des modèles que vous personnalisez.

Le principal scénario d’utilisation de la propriété **AutomationProperties.AccessibilityView** consiste à omettre délibérément certains contrôles d’un modèle dans les vues UI Automation, car ils ne contribuent pas clairement à l’affichage avec accessibilité de l’ensemble du contrôle. Pour éviter cela, affectez à **AutomationProperties.AccessibilityView** la valeur « Raw ».

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Levée d’exceptions à partir d’homologues d’automation  
Les API que vous implémentez pour la prise en charge de votre homologue d’automation sont autorisées à lever des exceptions. Tout client UI Automation à l’écoute est censé être suffisamment robuste pour continuer après la plupart des exceptions levées. Selon toute vraisemblance, cet écouteur se trouve face à une arborescence Automation comprenant des applications autres que les vôtres. Du point de vue de la conception, il est inacceptable que le client soit rendu complètement hors service si l’une des zones de l’arborescence lève une exception basée sur l’homologue quand le client appelle ses API.

Pour les paramètres qui sont passés à votre homologue, il est acceptable de valider l’entrée, et par exemple de lever [**ArgumentNullException**](/dotnet/api/system.argumentnullexception) si la valeur **null** a été passée et qu’il ne s’agit pas d’une valeur valide pour votre implémentation. Cependant, si votre homologue effectue des opérations ultérieures, souvenez-vous que les interactions de l’homologue avec le contrôle d’hébergement sont de nature asynchrone. Tout ce que fait un homologue ne bloque pas nécessairement le thread d’interface utilisateur dans le contrôle (ce qui est probablement mieux ainsi). Ainsi, un objet qui était disponible ou qui disposait de certaines propriétés au moment de la création de l’homologue ou lors du premier appel de la méthode d’homologue d’automation peut avoir à présent un état de contrôle différent. Dans ces cas-là, un fournisseur peut lever deux exceptions dédiées :

* Levez l’exception [**ElementNotAvailableException**](/dotnet/api/system.windows.automation.elementnotavailableexception) si vous êtes dans l’incapacité d’accéder au propriétaire de l’homologue ou à un élément d’homologue lié en fonction des informations d’origine passées à votre API. Par exemple, un homologue peut tenter d’exécuter ses méthodes, mais dans le même temps, suite à la fermeture d’une boîte de dialogue modale par exemple, le propriétaire a été supprimé de l’interface utilisateur. Pour un client non-.NET, cela correspond à [**UIA \_ E \_ ELEMENTNOTAVAILABLE**](/windows/desktop/WinAuto/uiauto-error-codes).
* Levez [**ElementNotEnabledException**](/dotnet/api/system.windows.automation.elementnotenabledexception) si le propriétaire est encore présent, mais que celui-ci est dans un mode tel que [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled)`=`**false** qui bloque une partie des modifications par programme spécifiques que votre homologue cherche à accomplir. Pour un client non-.NET, cela correspond à [**UIA \_ E \_ ELEMENTNOTENABLED**](/windows/desktop/WinAuto/uiauto-error-codes).

Au-delà de cet aspect, les homologues doivent être relativement conservateurs concernant les exceptions qu’ils lèvent dans le cadre de la prise en charge des homologues. La plupart des clients ne sont pas en mesure de gérer les exceptions des homologues ni de les transformer en choix exploitables proposés aux utilisateurs lors de l’interaction avec le client. Par conséquent, il est parfois plus judicieux d’employer un « no-op » et d’intercepter les exceptions sans les lever une nouvelle fois dans vos implémentations d’homologue que de lever des exceptions chaque fois que l’homologue ne parvient pas à faire quelque chose. Tenez aussi compte du fait que la plupart des clients UI Automation ne sont pas écrits en code managé. La plupart sont écrits en COM et vérifient simplement **les \_ OK** dans un **HRESULT** chaque fois qu’ils appellent une méthode de client UI Automation qui finit par accéder à votre homologue.

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  
* [Accessibilité](accessibility.md)
* [Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [**FrameworkElementAutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)
* [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
* [**OnCreateAutomationPeer**](/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Modèles de contrôle et interfaces](control-patterns-and-interfaces.md)

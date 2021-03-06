---
description: Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Exigences de texte accessible
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aefc53f6d28d2c30566680ac985a4712040ea8e0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032611"
---
# <a name="accessible-text-requirements"></a>Exigences de texte accessible  




Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire. Elle traite également des rôles Microsoft UI Automation que peuvent avoir les éléments de texte dans une application UWP et des meilleures pratiques relatives au texte des graphiques.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Coefficients de contraste  
Bien que les utilisateurs aient toujours la possibilité de basculer en mode de contraste élevé, la conception de votre application en ce qui concerne le texte doit considérer cette option comme un dernier recours. L’idéal consiste à s’assurer que le texte de votre application remplit certains critères établis quant au niveau de contraste entre le texte et son arrière-plan. L’évaluation du niveau de contraste est basée sur des techniques déterministes qui ne prennent pas en compte la teinte. Par exemple, si vous avez du texte rouge sur fond vert, ce texte risque de ne pas être lisible pour quelqu’un souffrant de daltonisme. La vérification et la correction du coefficient de contraste peuvent éliminer ce genre de problème d’accessibilité.

Les recommandations en matière de contraste du texte documentées ici sont basées sur une norme d’accessibilité Web intitulée [G18: Ensuring that a contrast ratio of at least 4.5:1 exists between text (and images of text) and background behind the text](https://www.w3.org/TR/WCAG20-TECHS/G18.html). Ces conseils se trouvent dans la spécification *Techniques W3C pour WCAG 2.0* .

Pour être considéré comme accessible, le texte visible doit présenter un coefficient de contraste de luminosité minimal de 4,5 pour 1 par rapport à l’arrière-plan. Les exceptions comprennent les logos et le texte accessoire tel que le texte qui fait partie d’un composant d’interface utilisateur inactif.

Le texte décoratif et qui ne véhicule aucune information est exclu. Par exemple, si des mots aléatoires sont utilisés pour créer un arrière-plan, et que ces mots peuvent être réorganisés ou remplacés sans changer le sens, ils sont considérés comme étant décoratifs et n’ont pas besoin de répondre à ce critère.

Utilisez des outils de contraste des couleurs pour vérifier que le coefficient de contraste du texte visible est acceptable. Pour connaître les outils permettant de tester les coefficients de contraste, voir la spécification [Techniques for WCAG 2.0 G18 (section Resources)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

> [!NOTE]
> Certains outils répertoriés par la spécification Techniques for WCAG 2.0 G18 ne peuvent pas être utilisés de manière interactive avec une application UWP. Vous pouvez être amené à saisir manuellement des valeurs de couleur de premier plan et d’arrière-plan dans l’outil, ou à effectuer des captures d’écran de l’interface utilisateur de l’application, puis à exécuter l’outil de coefficient de contraste sur l’image de capture d’écran.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Rôles d’éléments de texte  
Une application UWP peut utiliser les éléments par défaut suivants (couramment appelés *éléments de texte* ou *contrôles d’édition de texte* ) :

* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) : le rôle est [**Text**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**Zone de texte**](/uwp/api/Windows.UI.Xaml.Controls.TextBox): le rôle est [ **modifier**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) (et classe de dépassement [**RichTextBlockOverflow**](/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)) : le rôle est du [**texte**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) : le rôle est [**Edit**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)

Lorsqu’un contrôle signale un rôle de [**modification**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType), les technologies d’assistance supposent qu’il existe des moyens permettant aux utilisateurs de modifier les valeurs. Par conséquent, si vous placez du texte statique dans une [**zone**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)de texte, vous signalez le rôle et, par conséquent, le rapport de la structure de votre application à l’utilisateur d’accessibilité.

Dans les modèles de texte pour XAML, deux éléments sont principalement utilisés pour le texte statique : [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) et [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock). Aucun de ces éléments n’est une sous-classe de [**contrôle**](/uwp/api/Windows.UI.Xaml.Controls.Control) et, par conséquent, aucune d’entre elles n’est axée sur le clavier ou peut apparaître dans l’ordre de tabulation. Toutefois, cela ne signifie pas que les technologies d’assistance ne peuvent pas ou ne veulent pas les lire. Les lecteurs d’écran sont généralement conçus pour prendre en charge plusieurs modes de lecture du contenu dans une application, notamment un mode de lecture dédié ou des modèles de navigation qui vont au-delà du focus et de l’ordre de tabulation, comme un « curseur virtuel ». Par conséquent, ne placez pas votre texte statique dans des conteneurs pouvant être actifs simplement pour que l’ordre de tabulation y amène l’utilisateur. Les utilisateurs des technologies d’assistance s’attendent à ce que tous les éléments inclus dans l’ordre de tabulation soient interactifs. Par conséquent, le fait d’y rencontrer du texte statique peut s’avérer plus déroutant qu’utile. Nous vous recommandons de tester cette fonction vous-même, via le Narrateur, afin de vous faire une idée de l’expérience utilisateur liée à votre application lorsque vous utilisez le lecteur d’écran pour examiner le texte statique de cette dernière.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Accessibilité de suggestion automatique  
Lorsqu’un utilisateur effectue une saisie dans un champ d’entrée et qu’une liste de suggestions potentielles apparaît, il s’agit d’une suggestion automatique. Ce scénario est courant dans le champ **À :** d’un message, dans la zone de recherche Cortana de Windows, dans le champ d’entrée d’URL de Microsoft Edge, dans le champ d’entrée de situation géographique de l’application Météo, etc. Si vous utilisez la fonction XAML [**AutosuggestBox**](/uwp/api/windows.ui.xaml.controls.autosuggestbox) ou les commandes intrinsèques HTML, cette expérience est d’ores et déjà raccordée par défaut. Pour la rendre accessible, le champ d’entrée et la liste doivent être associés. Cette procédure est expliquée dans la section [Implémentation de la suggestion automatique](#implementing_auto-suggest).

La Narrateur a été mis à jour pour rendre cette expérience accessible avec un mode spécial de suggestions. À un niveau élevé, quand le champ d’entrée et la liste sont associés de manière appropriée, l’utilisateur final :

* Aura connaissance de la liste et du moment de clôture de cette dernière
* Aura connaissance du nombre de suggestions disponibles
* Aura connaissance de l’élément sélectionné, le cas échéant
* Sera en mesure de déplacer le focus du Narrateur de la liste
* Sera en mesure de parcourir une suggestion avec l’ensemble des autres modes de lecture

![Liste de suggestions](images/autosuggest-list.png)<br/>
_Exemple de liste de suggestions_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>Implémentation de la suggestion automatique  
Pour rendre cette expérience accessible, le champ d’entrée et la liste doivent être associés dans l’arborescence UIA. Cette association est effectuée avec la propriété [UIA_ControllerForPropertyId](/windows/win32/winauto/uiauto-automation-element-propids) des applications de bureau ou la propriété [ControlledPeers](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) des applications UWP.

À un niveau élevé, il existe 2 types d’expériences de suggestion automatique.

**Sélection par défaut**  
Si une sélection par défaut est effectuée dans la liste, le narrateur recherche un événement  [**UIA_SelectionItem_ElementSelectedEventId**](/windows/desktop/WinAuto/uiauto-event-ids) dans une application de bureau ou l’événement [**AutomationEvents. SelectionItemPatternOnElementSelected**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) à déclencher dans une application UWP. Chaque fois que la sélection est modifiée, quand un utilisateur saisit une autre lettre et que les suggestions ont été mises à jour et quand un utilisateur parcourt la liste, l’événement **ElementSelected** doit être déclenché.

![Liste avec une sélection par défaut](images/autosuggest-default-selection.png)<br/>
_Exemple avec une sélection par défaut_

**Aucune sélection par défaut**  
S’il n’existe aucune sélection par défaut, par exemple dans la zone emplacement de l’application météo, le narrateur recherche l’événement de [**UIA_LayoutInvalidatedEventId**](/windows/desktop/WinAuto/uiauto-event-ids) de bureau ou l’événement [**LayoutInvalidated**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) UWP à déclencher sur la liste chaque fois que la liste est mise à jour.

![Liste sans sélection par défaut](images/autosuggest-no-default-selection.png)<br/>
_Exemple sans sélection par défaut_

### <a name="xaml-implementation"></a>Implémentation XAML  
Si vous utilisez la fonction XAML par défaut [**AutosuggestBox**](/uwp/api/windows.ui.xaml.controls.autosuggestbox), l’ensemble des connexions sont déjà effectuées. Si vous développez votre propre expérience de suggestion automatique à l’aide d’un élément [**TextBox**](/uwp/api/windows.ui.xaml.controls.textbox) et d’une liste, vous devrez définir la liste en tant que [**AutomationProperties.ControlledPeers**](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) sur l’élément **TextBox** . Vous devez déclencher l’événement **AutomationPropertyChanged** de la propriété [**ControlledPeers**](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) chaque fois que vous ajoutez ou supprimez cette propriété, mais également déclencher vos propres événements [**SelectionItemPatternOnElementSelected**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) ou [**LayoutInvalidated**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) en fonction de votre type de scénario, tel qu’expliqué plus haut dans cet article.

### <a name="html-implementation"></a>Implémentation HTML  
Si vous utilisez les contrôles intrinsèques du format HTML, l’implémentation UIA a d’ores et déjà été mappée. Vous trouverez ci-dessous un exemple d’une implémentation déjà connectée :

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 Si vous créez vos propres contrôles, vous devez définir vos propres contrôles ARIA, décrits dans les normes W3C.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>Texte dans les graphiques

Dans la mesure du possible, évitez d’inclure du texte dans un graphique. Par exemple, tout texte que vous placez dans le fichier source d’image et qui est affiché dans l’application en tant qu’élément [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) n’est pas automatiquement accessible ou lisible par les technologies d’assistance. Si vous devez utiliser du texte dans les graphiques, assurez-vous que la valeur [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) que vous fournissez comme équivalent de « alt text » comprend ce texte ou un résumé de la signification du texte. Des considérations semblables s’appliquent si vous créez des caractères texte à partir de vecteurs dans le cadre d’un objet [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) ou à l’aide de [**Glyphs**](/uwp/api/Windows.UI.Xaml.Documents.Glyphs).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>Taille et échelle de police du texte

Les utilisateurs peuvent avoir des difficultés à lire du texte dans une application lorsque les polices utilisées sont simplement trop petites. par conséquent, assurez-vous que le texte de votre application est de taille raisonnable.

Une fois que vous avez terminé, Windows comprend différents outils et paramètres d’accessibilité dont les utilisateurs peuvent tirer parti et s’adapter à leurs propres besoins et préférences pour lire du texte. Elles incluent notamment :

* L’outil loupe, qui agrandit la zone sélectionnée de l’interface utilisateur. Vous devez vous assurer que la disposition du texte dans votre application ne complique pas l’utilisation de la loupe pour la lecture.
* Les paramètres de mise à l’échelle et de résolution globaux dans **paramètres->>système d’affichage->de l’échelle et de la disposition** . Les options de dimensionnement disponibles peuvent varier en fonction des capacités du périphérique d’affichage.
* Paramètres de taille du texte dans **paramètres->facilité d’accès->affichage** . Ajustez le paramètre **agrandir le texte** pour spécifier uniquement la taille du texte dans les contrôles de prise en charge pour l’ensemble des applications et écrans (tous les contrôles de texte UWP prennent en charge l’expérience de mise à l’échelle du texte sans personnalisation ni création de modèles). 
> [!NOTE]
> Le paramètre **rendre tout** le plus grand permet à un utilisateur de spécifier la taille préférée du texte et des applications en général sur leur écran principal uniquement.

Les différents contrôles et éléments de texte ont une propriété [**IsTextScaleFactorEnabled**](/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled). La valeur par défaut de cette propriété est **true** . Lorsque la **valeur est true** , la taille du texte de cet élément peut être mise à l’échelle. La mise à l’échelle affecte le texte dont les **polices** sont réduites à une valeur supérieure à celle d’un texte dont les **polices** sont volumineuses. Vous pouvez désactiver le redimensionnement automatique en affectant à la propriété **IsTextScaleFactorEnabled** d’un élément la **valeur false** . 

Pour plus d’informations, consultez [mise à l’échelle du texte](../input/text-scaling.md) .

Ajoutez le balisage suivant à une application et exécutez-la. Ajustez le paramètre de **taille du texte** et observez ce qui se passe à chaque **TextBlock** .

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

Nous vous déconseillons de désactiver la mise à l’échelle du texte, car la mise à l’échelle du texte de l’interface utilisateur de manière universelle sur toutes les applications est une expérience d’accessibilité importante pour les utilisateurs.

Vous pouvez également utiliser l’événement [**TextScaleFactorChanged**](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) et la propriété [**TextScaleFactor**](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) pour évaluer les incidences sur le paramètre **Taille du texte** sur le téléphone. Voici comment procéder :

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

La valeur de **TextScaleFactor** est un double dans la plage \[ 1, 2,25 \] . Le texte le plus petit subit un agrandissement de cette ampleur. Vous pouvez par exemple utiliser la valeur pour adapter des éléments graphiques au texte. Gardez toutefois à l’esprit que tout le texte n’est pas mis à l’échelle selon le même facteur. En règle générale, plus la taille du texte initial est élevée, moins le texte est affecté par la mise à l’échelle.

Les types suivants possèdent une propriété **IsTextScaleFactorEnabled** :  
* [**ContentPresenter**](/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) et classes dérivées
* [**FontIcon**](/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**TextElement**](/uwp/api/Windows.UI.Xaml.Documents.TextElement) et classes dérivées

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes  

* [Mise à l’échelle du texte](../input/text-scaling.md)
* [Accessibilité](accessibility.md)
* [Informations d’accessibilité élémentaires](basic-accessibility-information.md)
* [Exemple d’affichage de texte XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))
* [Exemple de modification de texte XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
* [Exemple d’accessibilité XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)

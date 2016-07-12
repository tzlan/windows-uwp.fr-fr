---
description: "Grâce aux instructions de conception et de codage fournies dans cette section, vous pourrez personnaliser votre application UWP pour certains types de saisies et d’appareils."
title: "Facilité d’utilisation des apps UWP - Développement d’apps Windows"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: f02713dfee278866af53c6dd529d2faa3e9f625c

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Facilité d’utilisation des applications UWP

Les applications UWP gèrent automatiquement un grand nombre de saisies et fonctionnent sur différents appareils. Aucune action supplémentaire de votre part n’est requise pour activer la saisie tactile ou la compatibilité avec un téléphone, par exemple. 

Mais il peut arriver que vous souhaitiez optimiser votre application pour certains types de saisies ou d’appareils. Par exemple, si vous créez une application de peinture, vous voudrez peut-être personnaliser la gestion des saisies au stylet. 

Grâce aux instructions de conception et de codage fournies dans cette section, vous pourrez personnaliser votre application UWP pour certains types de saisies et d’appareils. 

## Accessibilité

L’accessibilité consiste à rendre vos applications utilisables par des personnes ayant des limites qui empêchent ou entravent l’utilisation d’interfaces utilisateur conventionnelles. Pour certaines situations, les exigences en matière d’accessibilité sont imposées par la loi. Il est toutefois préférable de gérer les aspects liés à l’accessibilité quelles que soient les exigences juridiques, afin que votre application ait l’audience la plus étendue possible.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Vue d’ensemble de l’accessibilité](../accessibility/accessibility-overview.md)</b> <br/> Cet article est une vue d’ensemble des concepts et technologies associés aux scénarios d’accessibilité des applications UWP.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Conception de logiciels inclusifs](../accessibility/designing-inclusive-software.md)</b><br/>En savoir plus sur l’évolution de la conception inclusive avec les applications de la plateforme Windows universelle(UWP) pour Windows10.  Concevez et développez un logiciel inclusif en tenant compte de l’accessibilité.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Développement d’applications Windows inclusives](../accessibility/developing-inclusive-windows-apps.md)</b><br/> Cet article fait office de feuille de route pour développer des applications UWP accessibles.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Test de l’accessibilité](../accessibility/accessibility-testing.md) </b><br/>Procédures de test à appliquer pour s’assurer de l’accessibilité de votre application UWP.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Accessibilité dans le WindowsStore](../accessibility/accessibility-in-the-store.md)</b><br/>Décrit la configuration requise pour la déclaration de votre application UWP comme étant accessible dans le Windows Store.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Liste de vérification de l’accessibilité](../accessibility/accessibility-checklist.md)</b><br/>Fournit une liste de vérification pour vous aider à garantir que votre applicationUWP est accessible.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Présenter des informations d’accessibilité élémentaires](../accessibility/basic-accessibility-information.md)</b><br/>Les informations d’accessibilité élémentaires sont souvent classées en trois catégories : nom, rôle et valeur. Cette rubrique décrit le code qui aide votre application à exposer les informations élémentaires nécessaires aux technologies d’assistance.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Accessibilité du clavier](../accessibility/keyboard-accessibility.md)</b><br/>Si votre application ne fournit pas un bon accès par le clavier, les non-voyants ou les utilisateurs ayant des problèmes de mobilité peuvent rencontrer des difficultés à utiliser votre application ou risquent de ne pas pouvoir l’utiliser du tout.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Thèmes à contraste élevé](../accessibility/high-contrast-themes.md)</b><br/>Décrit les étapes nécessaires pour s’assurer que votre application UWP est utilisable quand un thème à contraste élevé est actif. </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Exigences de texte accessible](../accessibility/accessible-text-requirements.md)</b><br/>Cette rubrique décrit les meilleures pratiques relatives à l’accessibilité du texte dans une application, en garantissant que les couleurs et de l’arrière-plan respectent le coefficient de contraste nécessaire. Elle traite également des rôles MicrosoftUIAutomation que peuvent avoir les éléments de texte dans une applicationUWP et des meilleures pratiques relatives au texte des graphiques.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Pratiques d’accessibilité à éviter](../accessibility/practices-to-avoid.md)</b><br/>Répertorie les pratiques à éviter si vous voulez créer une applicationUWP accessible.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Homologues d’automation personnalisés](../accessibility/custom-automation-peers.md)</b><br/>Décrit le concept des homologues d’automatisation pour UIAutomation, et la manière dont vous pouvez fournir une prise en charge de l’automatisation pour votre propre classe d’interface utilisateur personnalisée.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Modèles de contrôle et interfaces](../accessibility/control-patterns-and-interfaces.md)</b><br/>Répertorie les modèles de contrôle Microsoft UI Automation, les classes que les clients utilisent pour y accéder, ainsi que les interfaces que les fournisseurs utilisent pour les implémenter.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b>   
</p>
  </div>
</div>
</div>



## Globalisation et localisation

Windows est utilisé dans le monde entier, par des publics de diverses cultures, régions et langues. Un utilisateur peut parler une ou même plusieurs langues. Un utilisateur peut se trouver n’importe où dans le monde et peut parler une langue quelconque selon l’endroit où il se trouve. Pour étendre le marché potentiel de votre application, vous pouvez la rendre facilement adaptable grâce aux fonctionnalités de globalisation et de localisation. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Pratiques conseillées et déconseillées](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>Suivez ces meilleures pratiques en globalisant vos applications pour un public plus large, et en localisant vos applications pour un marché spécifique.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Utiliser des formats compatibles avec la globalisation](../globalizing/use-global-ready-formats.md)</b><br/>Développez une application dans une perspective de globalisation en mettant correctement en forme les dates, les heures, les nombres et les devises.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Gérer la langue et la région](../globalizing/manage-language-and-region.md)</b><br/>Contrôlez la façon dont Windows sélectionne les ressources de l’interface utilisateur et formate les éléments de l’interface utilisateur de l’application, en utilisant les différents paramètres de langue et de région fournis par Windows.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Utiliser des modèles de format des dates et heures](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>Utilisez l’API [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) avec des modèles personnalisés pour afficher les dates et les heures dans le format que vous désirez.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Ajuster la disposition et les polices, et prendre en charge le sens du flux DàG](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>Développez votre application pour prendre en charge les dispositions et les polices de plusieurs langues, notamment le sens du flux de droite à gauche (DàG).</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Préparer votre application en vue de sa localisation](../globalizing/prepare-your-app-for-localization.md)</b><br/>Préparez votre application en vue de sa localisation pour d’autres marchés, langues ou régions.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Placer des chaînes d’interface utilisateur dans des ressources](../globalizing/put-ui-strings-into-resources.md)</b><br/>Placez les ressources de type chaîne de votre interface utilisateur dans des fichiers de ressources. Vous pouvez ensuite référencer ces chaînes dans votre code ou votre balisage.</p>
  </div>
  <div class="side-by-side-content-right">
<b></b>   
<p></p>
  </div>
</div>
</div>


## Paramètres d’application

Les paramètres d’application permettent à l’utilisateur de personnaliser votre application afin de l’adapter à ses propres besoins et préférences. Grâce à une configuration et à un stockage adéquats des paramètres, une expérience utilisateur de bonne qualité peut devenir encore meilleure. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Recommandations](../app-settings/guidelines-for-app-settings.md)</b><br/>Meilleures pratiques pour créer et afficher des paramètres d’application</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Stocker et récupérer des données d’application](../app-settings/store-and-retrieve-app-data.md)</b><br/>Comment stocker et récupérer des données d’application locales, itinérantes et temporaires</p>
  </div>
</div>
</div>

## Aide dans l’application
Même si votre application a été très bien conçue, certains utilisateurs auront besoin d’un peu d’aide. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Recommandations en matière d’aide de l’application](../app-help-guidelines/guidelines-for-app-help.md)</b><br/>Du fait de la complexité de certaines applications, la fourniture d’une aide efficace sur ces dernières peut améliorer considérablement l’expérience des utilisateurs. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Interface utilisateur d’instruction](../app-help-guidelines/instructional-ui.md)</b><br/>Dans certains cas, il peut être utile de former l’utilisateur aux fonctions les plus subtiles de votre application, telles que des interactions tactiles spécifiques. Vous devez alors fournir des instructions par le biais de l’interface utilisateur pour signaler à l’utilisateur les fonctionnalités dont il risque de ne pas encore avoir eu connaissance.</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Aide dans l’application](../app-help-guidelines/in-app-help.md)</b><br/>Dans la plupart des cas, il est préférable d’afficher l’aide au sein de l’application et à la demande de l’utilisateur. Tenez compte des recommandations suivantes lors de la création d’une aide dans l’application.</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Aide externe](../app-help-guidelines/external-help.md)</b><br/>Dans la plupart des cas, il est préférable d’afficher l’aide au sein de l’application et à la demande de l’utilisateur. Tenez compte des recommandations suivantes lors de la création d’une aide dans l’application.</p>
  </div>
</div>
</div>






<!--HONumber=Jun16_HO4-->


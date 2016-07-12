---
description: "Personnalisez votre application UWP pour des types de saisie et d’appareils donnés."
title: "Conception selon la saisie &amp; l’appareil - Développement d’apps Windows"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: fa1567d3ff80dc9c9376736c7d25c2bb06e79cc9
ms.openlocfilehash: f2055318fe67a0af2bcc009c6f9782029f9032f1

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# Entrée et appareils

Les applications UWP gèrent automatiquement un grand nombre de saisies et fonctionnent sur différents appareils. Aucune action supplémentaire de votre part n’est requise pour activer la saisie tactile ou la compatibilité avec un téléphone, par exemple. 

Mais il peut arriver que vous souhaitiez optimiser votre application pour certains types de saisies ou d’appareils. Par exemple, si vous créez une application de peinture, vous voudrez peut-être personnaliser la gestion des saisies au stylet. 

Grâce aux instructions de conception et de codage fournies dans cette section, vous pourrez personnaliser votre application UWP pour certains types de saisies et d’appareils. 

## Saisies et interactions

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Notions fondamentales sur la saisie](input-primer.md)</b><br/> Familiarisez-vous avec chaque type de périphérique d’entrée, ses comportements, ses fonctionnalités et ses limites, selon son association à certains facteurs de forme.   
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Cortana](cortana-interactions.md) </b><br/> Complétez la fonctionnalité de base de Cortana avec des commandes vocales qui lancent et exécutent une action unique dans une application externe.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>[Boîtier de commande et télécommande](gamepad-and-remote-interactions.md)</b><br/>Les applications UWP prennent désormais en charge les entrées du boîtier de commande et de la télécommande. Les boîtiers de commandes et les télécommandes sont les principaux appareils d’entrée utilisables avec une Xbox et un téléviseur.  
  </div>
  <div class="side-by-side-content-right">
<b>[Clavier](keyboard-interactions.md)</b><br/>Les entrées via le clavier représentent une part importante de l’expérience d’interaction utilisateur globale pour les applications. Le clavier est indispensable pour certaines personnes souffrant d’un handicap et les utilisateurs qui le considèrent simplement comme un moyen plus efficace d’interaction avec une application.  
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Souris](mouse-interactions.md)</b><br/>Les entrées de la souris conviennent mieux aux interactions utilisateur qui demandent de la précision comme le pointage et le clic. Cette précision inhérente est naturellement prise en charge par l’interface utilisateur de Windows qui permet de gérer la nature imprécise de l’entrée tactile.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Stylet](pen-and-stylus-interactions.md)</b><br/>Optimisez votre application UWP pour la saisie au stylet afin de fournir une fonctionnalité standard du dispositif de pointage et d’octroyer une expérience WindowsInk optimale à vos utilisateurs.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Voix](speech-interactions.md)</b><br/>Intégrez la reconnaissance vocale et la conversion de texte par synthèse vocale (également appelée TTS ou synthèse vocale) directement dans l’expérience utilisateur de votre application.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Interaction tactile](touch-interactions.md)</b><br/>UWP inclut différents mécanismes pour gérer les entrées tactiles, qui vous permettent de créer une expérience immersive que les utilisateurs de vos applications peuvent explorer avec confiance.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Pavé tactile](touchpad-interactions.md)  </b><br/>Concevez votre application afin que les utilisateurs puissent interagir avec elle par le biais d’un pavé tactile. Un pavé tactile combine l’entrée tactile multipoint indirecte et l’entrée de précision d’un dispositif de pointage comme la souris. Fort de cette combinaison, le pavé tactile est adapté à l’interface utilisateur optimisée pour l’interaction tactile et aux cibles d’applications de productivité plus petites.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Entrées multiples](multiple-input-design-guidelines.md)  </b><br/>Pour vous adapter au plus grand nombre possible d’utilisateurs et d’appareils, nous vous recommandons de concevoir vos applications de manière à ce qu’elles fonctionnent avec le plus grand nombre possible de types d’entrées (mouvement, commande vocale, écran tactile, pavé tactile, souris et clavier). Cela a pour effet d’optimiser la flexibilité, la facilité d’utilisation et l’accessibilité.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Zoom optique et redimensionnement](guidelines-for-optical-zoom.md)</b><br/>Cet article décrit les éléments de zoom et de redimensionnement Windows. Elle fournit également des recommandations en matière d’expérience utilisateur en cas d’utilisation de ces mécanismes d’interaction dans vos applications.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Mouvement panoramique](guidelines-for-panning.md)</b><br/>Le mouvement panoramique ou défilement permet à l’utilisateur de naviguer au sein d’une vue unique pour afficher le contenu de la vue qui ne tient pas entièrement dans la fenêtre d’affichage.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Rotation](guidelines-for-rotation.md)</b><br/> Cet article décrit la nouvelle interface utilisateur Windows pour la rotation et fournit des recommandations en matière d’expérience utilisateur à prendre en compte lors de l’utilisation de ce nouveau mécanisme d’interaction dans votre application UWP.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Sélection de texte et d’images](guidelines-for-textselection.md)</b><br/>Cet article décrit la sélection et la manipulation de texte, d’images et de contrôles, et fournit des recommandations en matière d’expérience utilisateur à prendre en compte lors de l’utilisation de ces mécanismes dans vos applications.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Ciblage](guidelines-for-targeting.md)</b><br/>Le ciblage tactile dans Windows utilise la zone de contact entière de chaque doigt détecté par un numériseur tactile. Le jeu de données d’entrée plus grand et plus complexe généré par le numériseur est utilisé pour accroître la précision lors de la détermination de la cible souhaitée (ou la plus probable) de l’utilisateur.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Retour visuel](guidelines-for-visualfeedback.md)</b><br/>Utilisez le retour visuel pour indiquer aux utilisateurs quand leurs interactions sont détectées, interprétées et gérées. Le retour visuel peut aider les utilisateurs en encourageant l’interaction. Il indique le succès d’une interaction et améliore ainsi le sentiment de contrôle de l’utilisateur. Il transmet également l’état du système et réduit les erreurs.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Identifier des périphériques d’entrée](identify-input-devices.md)</b><br/>Identifiez les périphériques d’entrée connectés à un appareil de plateforme Windows universelle (UWP), ainsi que leurs fonctionnalités et attributs. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Saisie de texte personnalisé](custom-text-input.md)</b><br/>Les API Core Text de l’espace de noms Windows.UI.Text.Core activent une application UWP pour recevoir une entrée de texte à partir de n’importe quel service de texte pris en charge sur les appareils Windows. Cela permet à l’application de recevoir du texte dans n’importe quelle langue et à partir de n’importe quel type d’entrée, comme la saisie sur clavier, la saisie vocale ou la saisie à l’aide d’un stylet.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Gérer les entrées du pointeur](handle-pointer-input.md)</b><br/>Recevez, traitez et gérez les données d’entrée à partir de dispositifs de pointage, tels que l’interaction tactile, la souris, le stylet et le pavé tactile, dans les applications UWP.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b><br/>   
</p>
  </div>
</div>
</div>


## Appareils

Une bonne connaissance des appareils qui prennent en charge les applications UWP peut vous aider à offrir la meilleure expérience utilisateur pour chaque facteur de forme. Lors de la conception d’une application pour un appareil particulier, vous devez surtout prendre en compte l’affichage de l’application sur cet appareil, ainsi que le moment, l’endroit ou la façon dont l’application sera utilisée sur cet appareil et le type d’interaction de l’utilisateur avec ce dernier.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Notions fondamentales sur les appareils](device-primer.md)</b><br/>Une bonne connaissance des appareils qui prennent en charge les applications UWP peut vous aider à offrir la meilleure expérience utilisateur pour chaque facteur de forme. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Conception pour Xbox et télévision](designing-for-tv.md)</b><br/>Concevez votre application de plateformeWindows universelle (UWP) pour une esthétique et un fonctionnement optimaux sur les écrans de télévision et XboxOne.
</p>
  </div>
</div>
</div>




<!--HONumber=Jul16_HO1-->


---
description: Découvrez comment permettre aux utilisateurs d’accéder à leurs contacts et leurs rendez-vous pour partager du contenu, des e-mails, des informations de calendrier ou des messages.
title: Contacts et calendriers
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, contacts, calendrier, rendez-vous, e-mails
ms.localizationpriority: medium
ms.openlocfilehash: 49fe277aa18627e304d2d6f27c023a13c85dccd0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174053"
---
# <a name="contacts-my-people-and-calendar"></a>Contacts, Mes Contacts et Calendrier


Vous pouvez permettre aux utilisateurs d’accéder à leurs contacts et rendez-vous afin de partager du contenu, des courriers électroniques, des informations de calendrier ou des messages, ou toute autre fonctionnalité de votre conception.

Pour en savoir plus sur les différentes façons d’accéder aux contacts et aux rendez-vous dans votre application, voir les rubriques suivantes :

| Rubrique | Description |
|-------|-------------|
| [Sélectionner des contacts](selecting-contacts.md) | L’espace de noms [<strong>Windows.ApplicationModel.Contacts</strong>](/uwp/api/Windows.ApplicationModel.Contacts) fournit plusieurs options de sélection des contacts. Ici, nous allons vous montrer comment sélectionner un ou plusieurs contacts, et comment configurer le sélecteur de contacts pour récupérer uniquement les informations de contact dont votre application a besoin. |
| [Envoyer un e-mail](sending-email.md) | Montre comment lancer la boîte de dialogue de rédaction d’un message électronique pour permettre à l’utilisateur d’envoyer un message électronique. Vous pouvez préremplir les champs de l’e-mail avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi. |
| [Envoyer un message SMS](sending-an-sms-message.md) | Cette rubrique vous montre comment lancer la boîte de dialogue de rédaction d’un message SMS pour permettre à l’utilisateur d’envoyer un message SMS. Vous pouvez préremplir les champs du message SMS avec des données avant d’afficher la boîte de dialogue. Le message ne sera pas envoyé tant que l’utilisateur n’aura pas appuyé sur le bouton d’envoi. |
| [Gérer des rendez-vous](managing-appointments.md) | L’espace de noms [<strong>Windows.ApplicationModel.Appointments</strong>](/uwp/api/Windows.ApplicationModel.Appointments) vous permet de créer et gérer des rendez-vous dans l’application Calendrier d’un utilisateur. Ici, nous allons vous montrer comment créer un rendez-vous, l’ajouter à l’application Calendrier, le remplacer dans l’application Calendrier et le supprimer de l’application Calendrier. Nous allons également expliquer comment afficher une période de temps pour une application Calendrier et créer un objet appointment-recurrence. |
| [Connecter votre application à des actions sur une carte de visite](integrating-with-contacts.md) | Explique comment faire apparaître votre application à côté des actions sur une carte de visite ou une mini carte de visite. Les utilisateurs peuvent choisir votre application pour effectuer une action telle qu’ouvrir une page de profil, effectuer un appel ou envoyer un message. |
| [Ajout de la prise en charge de Mes Contacts à une application](my-people-support.md) | Explique comment ajouter la prise en charge de Mes Contacts à une application et comment épingler et désépingler des contacts dans la barre des tâches. |
| [Partage de Mes Contacts](my-people-sharing.md) | Explique comment ajouter la prise en charge du partage de Mes Contacts qui permet aux utilisateurs de partager du contenu avec leurs contacts épinglés en faisant glisser des fichiers à partir de l’Explorateur de fichiers vers une épingle de Mes Contacts. |
| [Notifications de Mes Contacts](my-people-notifications.md) | Explique comment créer et utiliser les notifications de Mes Contacts, un nouveau type de notification toast envoyée à partir d’un contact épinglé. |

 

## <a name="related-topics"></a>Rubriques connexes

* [Exemple d’API de rendez-vous](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Appointments)
* [Exemple d’API du Gestionnaire de contacts](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Contact%20manager%20API%20sample)
* [Exemple d’application du sélecteur de contacts](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/ContactPicker)
* [Exemple de gestion des actions de contact](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/Handling%20Contact%20Actions)
* [Exemple d’intégration à une carte de visite](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
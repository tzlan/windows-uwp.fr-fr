---
description: Vous pouvez ajouter des utilisateurs, des groupes et des Azure AD applications à votre compte espace partenaires.
title: Ajouter des utilisateurs, des groupes et des applications Azure AD à votre compte Espace partenaires
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, application Azure ad, AAD, utilisateur, groupe, utilisateurs multiples, multi-utilisateur
ms.localizationpriority: medium
ms.openlocfilehash: 59f3a69399b33164688da3c80c781828802662f5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032882"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Ajouter des utilisateurs, des groupes et des applications Azure AD à votre compte Espace partenaires

La section **utilisateurs** de l' [espace partenaires](https://partner.microsoft.com/dashboard) (sous **paramètres du compte** ) vous permet d’utiliser Azure Active Directory pour ajouter des utilisateurs à votre compte espace partenaires. Chaque utilisateur se voit attribuer un rôle (ou un ensemble d’autorisations personnalisées) qui définit son accès au compte. Vous pouvez également ajouter [des groupes d’utilisateurs et des](#groups) [applications de Azure ad](#azure-ad-applications) pour leur accorder l’accès à votre compte espace partenaires.

Une fois que les utilisateurs ont été ajoutés au compte, vous pouvez [modifier les détails du compte](#edit), modifier les [rôles et les autorisations](set-custom-permissions-for-account-users.md)ou supprimer des [utilisateurs](#remove).

> [!IMPORTANT]
> Pour ajouter des utilisateurs à votre compte, vous devez d’abord [associer votre compte espace partenaires au locataire Azure Active Directory de votre organisation](associate-azure-ad-with-partner-center.md). 

Lorsque vous ajoutez des utilisateurs, vous devez spécifier leur accès à votre compte espace partenaires en leur affectant un [rôle ou un ensemble d’autorisations personnalisées](set-custom-permissions-for-account-users.md). 

N’oubliez pas que tous les utilisateurs de l’espace partenaires (y compris les groupes et les applications Azure AD) doivent disposer d’un compte actif dans [un client Azure AD associé à votre compte espace partenaires](associate-azure-ad-with-partner-center.md). La gestion des utilisateurs est effectuée dans un seul client à la fois ; vous devez vous connecter avec un compte de responsable pour le locataire dans lequel vous souhaitez ajouter ou modifier des utilisateurs. La création d’un utilisateur dans l’espace partenaires crée également un compte pour cet utilisateur dans le client Azure AD auquel vous êtes connecté, et le fait de modifier le nom d’un utilisateur dans l’espace partenaires apporte les mêmes modifications au locataire Azure AD de votre organisation.

> [!NOTE]
> Si votre organisation utilise l’[intégration d’annuaire](/previous-versions/azure/azure-services/jj573653(v=azure.100)) pour synchroniser le service d’annuaire local avec votre compte Azure AD, vous ne pourrez pas créer de nouveaux utilisateurs, groupes ou applications Azure AD dans l’Espace partenaires. Vous (ou un autre administrateur de l’annuaire local) devez les créer directement dans le répertoire local avant de pouvoir les afficher et les ajouter dans l’Espace partenaires.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Ajouter des utilisateurs à votre compte espace partenaires

Pour ajouter des utilisateurs à votre compte espace partenaires, accédez à la page **utilisateurs** dans **paramètres du compte** , puis sélectionnez **Ajouter des utilisateurs.** Vous devez être connecté avec un compte de gestionnaire pour le locataire Azure AD que vous souhaitez utiliser. 

### <a name="add-existing-users"></a>Ajouter des utilisateurs existants 

Vous pouvez sélectionner des utilisateurs qui existent déjà dans le locataire de votre organisation et leur attribuer un accès à votre compte espace partenaires. 

<span id="from-directory" />

1.  Sélectionnez l’icône d’engrenage (près de l’angle supérieur droit de l’espace partenaires), puis sélectionnez **paramètres du développeur** . Dans le menu **paramètres** , sélectionnez **utilisateurs** .
2.  Dans la page **utilisateurs** , sélectionnez **Ajouter des utilisateurs** . 
3.  Sélectionnez un ou plusieurs utilisateurs dans la liste qui s’affiche. Utilisez la zone de recherche pour rechercher des utilisateurs spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs utilisateurs à ajouter à votre compte espace partenaires, vous devez leur attribuer le même rôle ou ensemble d’autorisations personnalisées. Pour ajouter plusieurs utilisateurs avec des rôles/autorisations différents, répétez les étapes ci-dessous pour chaque rôle ou ensemble d’autorisations personnalisées.
4.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter la sélection** .
5.  Dans la section **rôles** , spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour le ou les utilisateurs sélectionnés.
6.  Cliquez sur **Enregistrer** .

### <a name="additional-methods-for-adding-users"></a>Méthodes supplémentaires pour l’ajout d’utilisateurs

Si vous êtes connecté avec un compte de responsable qui a également des autorisations d' [administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) pour le locataire Azure ad dans lequel vous travaillez, vous disposerez d’options supplémentaires pour ajouter des utilisateurs à votre compte espace partenaires. Vous devez sélectionner l’une des options suivantes :

-   **Ajouter des utilisateurs existants** : choisissez les utilisateurs qui existent déjà dans l’annuaire de votre organisation et donnez-leur accès à votre compte espace partenaires, à l’aide de la méthode décrite ci-dessus.
-   **Créer des utilisateurs** : créer un nouveau compte d’utilisateur à ajouter à l’annuaire de votre organisation et à votre compte espace partenaires
-   **Inviter en dehors des utilisateurs** : envoyez des invitations par courrier électronique aux utilisateurs qui ne se trouvent pas dans l’annuaire de votre organisation. Ils seront invités à accéder à votre compte espace partenaires, et un nouveau compte d' [utilisateur invité](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) sera créé pour eux dans votre locataire Azure ad.

<span id="new-user" />

### <a name="create-new-users"></a>Créer de nouveaux utilisateurs

> [!IMPORTANT]
> Vous devez être connecté avec un compte d’administrateur général dans votre locataire Azure AD pour pouvoir créer des utilisateurs.

1.  Dans la page **utilisateurs** (sous **paramètres du compte** ), sélectionnez Ajouter des **utilisateurs** , puis choisissez **créer des utilisateurs** .
2.  Entrez le prénom, le nom et le nom d’utilisateur du nouvel utilisateur.
3.  Si vous souhaitez que le nouvel utilisateur dispose d’un [compte d’administrateur général](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) dans l’annuaire de votre organisation, activez la case à cocher **faire de cet utilisateur un administrateur général dans votre Azure ad, avec un contrôle total sur toutes les ressources d’annuaire** . Cela offrira à l’utilisateur un accès complet à toutes les fonctionnalités d’administration dans l’Azure AD de l’entreprise. Ils peuvent ajouter et gérer des utilisateurs dans l’annuaire de votre organisation (mais pas dans l’espace partenaires, sauf si vous accordez au compte le [rôle ou les autorisations](set-custom-permissions-for-account-users.md)appropriés). Si vous activez cette case à cocher, vous devez fournir un **e-mail de récupération de mot de passe** pour l’utilisateur.
4.  Si vous avez coché la case pour **faire de cet utilisateur un administrateur général dans votre Azure ad** , entrez un e-mail que l’utilisateur peut utiliser s’il doit récupérer son mot de passe.
5.  Dans la section **Appartenance au groupe** , sélectionnez tous les groupes auxquels vous souhaitez que le nouvel utilisateur appartienne.
6.  Dans la section **rôles** , spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour l’utilisateur.
7.  Cliquez sur **Enregistrer** .
8.  Les informations de connexion du nouvel utilisateur, y compris un mot de passe temporaire, s’affichent sur la page de confirmation. Veillez à noter ces informations et à les fournir au nouvel utilisateur. Vous n’aurez plus accès au mot de passe temporaire après avoir quitté cette page.


<span id="email" />

### <a name="invite-outside-users"></a>Inviter des utilisateurs externes

> [!IMPORTANT]
> Vous devez être connecté avec un compte d’administrateur général dans votre locataire Azure AD pour inviter des utilisateurs externes.

1.  Dans la page **utilisateurs** (sous **paramètres du compte** ), sélectionnez Ajouter des **utilisateurs** , puis choisissez **inviter des utilisateurs par courrier électronique** .
1.  Entrez une ou plusieurs adresses e-mail (jusqu’à dix), séparées par des virgules ou des points-virgules.
2.  Dans la section **rôles** , spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour l’utilisateur.
3.  Cliquez sur **Enregistrer** .

Les utilisateurs que vous avez invités recevront une invitation par courrier électronique pour joindre votre compte, et un nouveau compte d' [utilisateur invité](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) sera créé pour eux dans votre locataire Azure ad. Chaque utilisateur devra accepter l’invitation avant de pouvoir accéder à votre compte.

Si vous avez besoin de renvoyer une invitation, recherchez l’utilisateur dans la page **utilisateurs** et sélectionnez son adresse de messagerie (ou le texte qui indique **invitation en attente** ). Puis, en bas de la page, cliquez sur **Renvoyer invitation** .

> [!IMPORTANT]
> Les utilisateurs externes que vous invitez à joindre à votre compte espace partenaires peuvent se voir attribuer les mêmes rôles et autorisations que les autres utilisateurs. Toutefois, les utilisateurs externes ne seront pas en mesure d’effectuer certaines tâches dans Visual Studio, telles que l’Association d’une application au magasin ou la création de packages à charger dans le magasin. Si un utilisateur doit effectuer ces tâches, choisissez **créer des utilisateurs** au lieu d' **inviter des utilisateurs externes** . (Si vous ne souhaitez pas ajouter ces utilisateurs à votre locataire Azure AD existant, vous pouvez [créer un nouveau locataire](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account), puis créer des comptes d’utilisateur pour eux dans ce locataire.) 


### <a name="changing-a-users-directory-password"></a>Modification du mot de passe d’annuaire d’un utilisateur

Si l’un de vos utilisateurs doit modifier son mot de passe, il peut le faire si vous avez fourni un **e-mail de récupération de mot de passe** lors de la création du compte d’utilisateur. Vous pouvez également mettre à jour le mot de passe d’un utilisateur en suivant les étapes ci-dessous (si vous êtes connecté avec un compte d’administrateur général dans votre locataire Azure AD pour modifier le mot de passe d’un utilisateur). Notez que cela modifiera le mot de passe utilisateur dans votre locataire Azure AD, ainsi que le mot de passe permettant d’accéder à l’Espace partenaires. 

1.  Depuis la page **Utilisateurs** (sous **Paramètres de compte** ), sélectionnez le nom du compte d’utilisateur que vous souhaitez modifier.
2.  Cliquez sur le bouton **Réinitialiser le mot de passe** en bas de la page.
3.  Une page de confirmation affiche les informations de connexion de l’utilisateur, y compris un mot de passe temporaire.

    > [!IMPORTANT]
    >  N’oubliez pas d’imprimer ou de copier ces informations et de les fournir à l’utilisateur, comme vous ne pourrez pas accéder au mot de passe temporaire une fois que vous aurez quitté cette page.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Ajouter des groupes à votre compte espace partenaires

Vous pouvez ajouter un groupe à partir de l’annuaire de votre organisation à votre compte espace partenaires. Dans ce cas, tous les utilisateurs qui sont membres de ce groupe pourront y accéder, avec les autorisations associées au rôle attribué au groupe.

### <a name="add-groups-from-your-organizations-directory"></a>Ajouter des groupes à partir de l’annuaire de votre organisation

1.  Sélectionnez l’icône d’engrenage (près de l’angle supérieur droit de l’espace partenaires), puis sélectionnez **paramètres du développeur** . Dans le menu **paramètres** , sélectionnez **utilisateurs** .
2. Dans la page **utilisateurs** , sélectionnez **Ajouter des groupes** .
2.  Sélectionnez un ou plusieurs groupes dans la liste qui s’affiche. Utilisez la zone de recherche pour rechercher des groupes spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs groupes à ajouter à votre compte Espace partenaires, vous devez leur attribuer le même rôle ou le même ensemble d’autorisations personnalisées. Pour ajouter plusieurs groupes avec des rôles/autorisations différents, répétez les étapes ci-dessous pour chaque rôle ou ensemble d’autorisations personnalisées.

3.  Une fois les groupes sélectionnés, cliquez sur **Ajouter la sélection** .
4.  Dans la section **rôles** , spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour le (s) groupe (s) sélectionné (s). Tous les membres du groupe seront en mesure d’accéder à votre compte espace partenaires avec les autorisations que vous appliquez au groupe, quels que soient les rôles/autorisations associés à leur compte individuel.
5.  Cliquez sur **Enregistrer** .


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Créer un compte de groupe dans l’annuaire de votre organisation et l’ajouter à votre compte espace partenaires

Si vous souhaitez accorder l’accès de l’espace partenaires à un nouveau groupe, vous pouvez créer un nouveau groupe dans la section **utilisateurs** . Notez que le nouveau groupe sera créé dans le répertoire de votre organisation, et pas seulement dans votre compte espace partenaires.

1.  Dans la page **utilisateurs** (sous **paramètres de développement** ), cliquez sur **Ajouter des groupes** .
2.  Sur la page suivante, sélectionnez **Nouveau groupe** .
3.  Entrez le nom d’affichage du nouveau groupe.
4.  Spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour le groupe. Tous les membres du groupe seront en mesure d’accéder à votre compte espace partenaires avec les autorisations que vous appliquez au groupe, quels que soient les rôles/autorisations associés à leur compte individuel.
5.  Sélectionnez le ou les utilisateurs à assigner au nouveau groupe dans la liste qui s’affiche. Utilisez la zone de recherche pour rechercher des utilisateurs spécifiques.
6.  Une fois les utilisateurs sélectionnés, cliquez sur **Ajouter les éléments sélectionnés** pour les ajouter au nouveau groupe.
7.  Cliquez sur **Enregistrer** .


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Ajouter des applications de Azure AD à votre compte espace partenaires

Vous pouvez autoriser les applications ou les services qui font partie de l’Azure AD de votre organisation à accéder à votre compte espace partenaires. Ces Azure AD comptes d’utilisateur d’application peuvent être utilisés pour appeler les API REST fournies par les [services Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Ajouter des applications Azure AD à partir de l’annuaire de votre organisation

1.  1.  Sélectionnez l’icône d’engrenage (près de l’angle supérieur droit de l’espace partenaires), puis sélectionnez **paramètres du développeur** . Dans le menu **paramètres** , sélectionnez **utilisateurs** .
2. Dans la page **utilisateurs** , sélectionnez **Ajouter des applications de Azure ad** .
3.  Sélectionnez une ou plusieurs applications Azure AD dans la liste qui s’affiche. Utilisez la zone de recherche pour rechercher des applications Azure AD spécifiques.
    > [!TIP]
    > Si vous sélectionnez plusieurs applications Azure AD à ajouter à votre compte Espace partenaires, vous devez leur attribuer le même rôle ou le même ensemble d’autorisations personnalisées. Pour ajouter plusieurs applications Azure AD avec des rôles/autorisations différents, répétez les étapes ci-dessous pour chaque rôle ou ensemble d’autorisations personnalisées.

4.  Une fois les applications Azure AD sélectionnées, cliquez sur **Ajouter les éléments sélectionnés** .
5.  Dans la section **rôles** , spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour les applications de Azure ad sélectionnées.
6.  Cliquez sur **Enregistrer** .


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Créer un compte d’application Azure AD dans le répertoire de votre organisation et l’ajouter à votre compte espace partenaires

Si vous souhaitez accorder à l’espace partenaires un accès à un tout nouveau Azure AD compte d’application, vous pouvez en créer un dans la section **utilisateurs** . Notez que cela créera un nouveau compte dans le répertoire de votre organisation, et pas seulement dans votre compte espace partenaires.

> [!TIP]
> Si vous utilisez principalement cette application Azure AD pour l’authentification à l’Espace partenaires, et que vous ne souhaitez pas que les utilisateurs y accèdent directement, vous pouvez entrer une adresse valide pour l’ **URL de réponse** et l’ **URI ID d’application** , tant que ces valeurs ne sont pas utilisées par une autre application Azure AD de votre répertoire.

1.  Depuis la page **Utilisateurs** (sous **Paramètres de compte** ), sélectionnez **Ajouter des applications Azure AD** .
2.  Sur la page suivante, sélectionnez **Nouvelle application Azure AD** .
3.  Entrez l’ **URL de réponse** pour la nouvelle application Azure AD. Il s’agit de l’URL grâce à laquelle les utilisateurs peuvent se connecter et utiliser votre application Azure AD (parfois également appelée URL de l’application ou URL de connexion). L’ **URL de réponse** ne peut pas dépasser 256 caractères et doit être unique dans votre répertoire.
4.  Entrez l’ **URI ID d’application** pour la nouvelle application Azure AD. Il s’agit d’un identificateur logique pour l’application Azure AD qui est présenté lors de l’envoi d’une demande d’authentification unique à Azure AD. Notez que l’ **URI ID d’application** doit être unique pour chaque application Azure AD de votre annuaire et ne doit pas comporter plus de 256 caractères. Pour plus d’informations sur l' **URI ID d’application** , consultez [intégration d’applications avec Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Dans la section **rôles** , spécifiez le [ou les rôles ou les autorisations personnalisées](set-custom-permissions-for-account-users.md) pour l’application Azure ad.
6.  Cliquez sur **Enregistrer** .

Une fois que vous avez ajouté ou créé une application Azure AD, vous pouvez revenir à la section **Utilisateurs** et sélectionner le nom de l’application pour passer en revue les paramètres de l’application, tels que l’ID du locataire, l’ID client, l’URL de réponse et l’URI ID d’application.

> [!NOTE]
> Si vous envisagez d’utiliser les API REST fournies par les [services Microsoft Store](../monetize/using-windows-store-services.md), vous aurez besoin de l’ID de locataire et des valeurs d’ID client indiquées sur cette page pour obtenir un jeton d’accès Azure ad que vous pouvez utiliser pour authentifier les appels aux services.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Gérer les clés d’une application Azure AD

Si votre application Azure AD lit et écrit des données dans Microsoft Azure AD, elle aura besoin d’une clé. Vous pouvez créer des clés pour une application Azure AD en modifiant ses informations dans l’espace partenaires. Vous pouvez également supprimer les clés qui ne sont plus nécessaires.

1.  Depuis la page **Utilisateurs** (sous **Paramètres de compte** ), sélectionnez le nom de l’application Azure AD.
    > [!TIP]
    > Lorsque vous cliquez sur le nom de l’application Azure AD, vous voyez toutes les clés actives pour l’application Azure AD, y compris la date à laquelle la clé a été créée et la date d’expiration. Pour supprimer une clé devenue inutile, cliquez sur **Supprimer** .

2.  Pour ajouter une nouvelle clé, sélectionnez **Ajouter une nouvelle clé** .
3.  Un écran s’affiche pour indiquer l' **ID client** et les valeurs de **clé** .
    > [!IMPORTANT]
    > N’oubliez pas d’imprimer ou de copier ces informations, car vous ne pourrez plus y accéder une fois que vous aurez quitté cette page.

4.  Si vous souhaitez créer plus de clés, sélectionnez **Ajouter une autre clé** .

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Modifier un utilisateur, un groupe ou une application Azure AD

Une fois que vous avez ajouté des utilisateurs, des groupes et/ou des applications Azure AD à votre compte espace partenaires, vous pouvez apporter des modifications à leurs informations de compte. 

> [!IMPORTANT]
> Les modifications apportées aux [rôles ou aux autorisations](set-custom-permissions-for-account-users.md) affectent uniquement l’accès à l’espace partenaires. Toutes les autres modifications (telles que la modification du nom d’un utilisateur ou de l’appartenance à un groupe, ou l’URL de réponse et l’URI ID d’application pour une application Azure AD) sont reflétées dans le client Azure AD de votre organisation et dans votre compte espace partenaires. 

1.  Dans la page **utilisateurs** (sous **paramètres du compte** ), sélectionnez le nom de l’utilisateur, du groupe ou de Azure AD compte d’application que vous souhaitez modifier.
2.  Apportez les modifications voulues. Les éléments que vous pouvez modifier sont les suivants :
    -   Pour un **utilisateur** , vous pouvez modifier le prénom, le nom ou le nom d’utilisateur de l’utilisateur. Vous pouvez également sélectionner ou désélectionner des groupes dans la section **appartenance au groupe** pour mettre à jour leur appartenance à un groupe.
    -   Pour un **groupe** , vous pouvez modifier le nom du groupe. (Pour mettre à jour l’appartenance à un groupe, modifiez les utilisateurs que vous souhaitez ajouter ou supprimer du groupe et apportez des modifications à la section **appartenance au groupe** .)
    -   Pour une **application Azure ad** , vous pouvez entrer de nouvelles valeurs pour l' **URL de réponse** ou l' **URI ID d’application** .
    N’oubliez pas que ces modifications seront apportées dans l’annuaire de votre organisation et dans votre compte espace partenaires.
3.  Pour les modifications liées à l’accès à l’espace partenaires, sélectionnez ou désélectionnez les rôles que vous souhaitez appliquer, ou sélectionnez **personnaliser les autorisations** et apportez les modifications souhaitées. Ces modifications affectent uniquement l’accès au centre partenaires et ne modifient pas les autorisations au sein du locataire Azure AD de votre organisation.
3.  Cliquez sur **Enregistrer** .


## <a name="view-history-for-account-users"></a>Afficher l’historique des utilisateurs du compte

En tant que propriétaire du compte, vous pouvez afficher l’historique de navigation détaillé de tous les utilisateurs supplémentaires que vous avez ajoutés au compte.

Dans la page **utilisateurs** (sous **paramètres du compte** ), sélectionnez le lien affiché sous **dernière activité** pour l’utilisateur dont vous souhaitez passer en revue l’historique de navigation. Vous serez en mesure d’afficher les URL de toutes les pages que l’utilisateur a visitées au cours des 30 derniers jours.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Supprimer des utilisateurs, des groupes et des applications de Azure AD

Pour supprimer un utilisateur, un groupe ou une Azure AD application de votre compte espace partenaires, sélectionnez le lien **supprimer** qui apparaît en regard de son nom dans la page **utilisateurs** . Une fois que vous avez confirmé que vous souhaitez le supprimer, cet utilisateur, ce groupe ou Azure AD application ne sera plus en mesure d’accéder à votre compte espace partenaires (à moins que vous ne l’ajoutiez à nouveau plus tard).

> [!IMPORTANT]
> La suppression d’un utilisateur, d’un groupe ou d’une application Azure AD signifie qu’il n’aura plus accès à votre compte espace partenaires. Elle ne supprime **pas** l’utilisateur, le groupe ou l’Azure ad application de l’annuaire de votre organisation.

 

---
author: mcleanbyron
description: Utilisez cette méthode dans l’API de soumission au Microsoft Store pour mettre à jour le pourcentage de déploiement du package pour une soumission de version d’évaluation du package.
title: Met à jour le pourcentage de lancement d’une soumission de version d’évaluation
ms.author: mcleans
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de soumission au Microsoft Store, lancement du package, soumission de version d’évaluation, mise à jour, pourcentage
ms.assetid: ee9aa223-e945-4c11-b430-1f4b1e559743
ms.localizationpriority: medium
ms.openlocfilehash: f028e1632eb0a330f9d4b282aa3f8968eff647de
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815974"
---
# <a name="update-the-rollout-percentage-for-a-flight-submission"></a>Met à jour le pourcentage de lancement d’une soumission de version d’évaluation


Utilisez cette méthode dans l’API de soumission au Microsoft Store pour [mettre à jour le pourcentage de déploiement](../publish/gradual-package-rollout.md#setting-the-rollout-percentage) pour une soumission de version d’évaluation du package. Pour plus d’informations sur le processus de création d’une soumission de version d’évaluation de package à l’aide de l’API de soumission au Microsoft Store, voir [Gérer les soumissions de versions d’évaluation du package](manage-flight-submissions.md).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes:

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission pour une application dans votre compte du Centre de développement. Pour cela, vous pouvez utiliser le tableau de bord du Centre de développement ou la méthode [Créer une soumission d’application](create-an-app-submission.md).
* Autorisez un déploiement de package progressif pour la soumission. Pour cela, vous pouvez utiliser le [tableau de bord du Centre de développement](../publish/gradual-package-rollout.md) ou [l’API de soumission au Microsoft Store](manage-flight-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et des paramètres de la requête.

| Méthode | URI de la requête                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | chaîne | Obligatoire. ID Windows Store de l’application qui contient la soumission de version d’évaluation du package avec le pourcentage de déploiement du package que vous voulez mettre à jour. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | chaîne | Obligatoire. ID de la version d’évaluation du package contenant la soumission avec le pourcentage de déploiement du package que vous voulez mettre à jour. Cet ID est disponible dans les données de réponse des requêtes pour [créer une version d’évaluation du package](create-a-flight.md) ou [obtenir des versions d’évaluation du package pour une application](get-flights-for-an-app.md). Concernant une version d’évaluation créée dans le tableau de bord du Centre de développement, cet ID est également disponible dans l’URL de la page de la version d’évaluation, dans le tableau de bord.  |
| submissionId | chaîne | Obligatoire. ID de la soumission avec le pourcentage de déploiement du package que vous voulez mettre à jour. Cet ID est disponible dans les données de réponse des requêtes pour [créer une soumission de version d’évaluation du package](create-a-flight-submission.md). Concernant une soumission créée dans le tableau de bord du Centre de développement, cet ID est également disponible dans l’URL de la page de soumission, dans le tableau de bord.  |
| pourcentage  |  flottant  |  Obligatoire. Le pourcentage d’utilisateurs qui recevront le package de déploiement progressif.  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment mettre à jour le pourcentage de déploiement du package d’une soumission de version d’évaluation de package.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/updatepackagerolloutpercentage?percentage=25 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Réponse

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de déploiement du package](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | La soumission de version d’évaluation du package est introuvable. |
| 409  | Ce code indique l’une des erreurs suivantes:<br/><br/><ul><li>La soumission n’est pas dans un état valide pour l’opération de déploiement progressif (avant d’appeler cette méthode, la soumission doit être publiée et la valeur [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) doit être définie sur **PackageRolloutInProgress**).</li><li>La soumission n’appartient pas à l’application spécifiée.</li><li>L’app utilise une fonctionnalité du tableau de bord du Centre de développement qui n’est [actuellement pas prise en charge par l’API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Rubriques associées

* [Lancement progressif de packages](../publish/gradual-package-rollout.md)
* [Gérer les soumissions de versions d’évaluation de package à l’aide de l’API de soumission au Microsoft Store](manage-flight-submissions.md)
* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)
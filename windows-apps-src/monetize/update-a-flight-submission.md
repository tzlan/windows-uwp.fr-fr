---
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour mettre à jour une soumission de vol de package existante.
title: Mettre à jour une soumission de version d’évaluation de package
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, soumission de vol, mise à jour
ms.localizationpriority: medium
ms.openlocfilehash: d2603319e2a1fc242210f79ef38c2ba5362c9c3e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171333"
---
# <a name="update-a-package-flight-submission"></a>Mettre à jour une soumission de version d’évaluation de package


Utilisez cette méthode dans l’API de soumission Microsoft Store pour mettre à jour une soumission de vol de package existante. Après avoir mis à jour une soumission à l’aide de cette méthode, vous devez [valider la soumission](commit-a-flight-submission.md) en vue de son intégration et de sa publication.

Pour plus d’informations sur la façon dont cette méthode s’intègre au processus de création d’une soumission de vol de packages à l’aide de l’API de soumission Microsoft Store, consultez [gérer les envois de vols de packages](manage-flight-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Créez une soumission de vol de package pour l’une de vos applications. Vous pouvez effectuer cette opération dans l’espace partenaires, ou vous pouvez le faire à l’aide de la méthode [créer une soumission de vol de package](create-a-flight-submission.md) .

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez mettre à jour une soumission de version d’évaluation de package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |
| flightId | string | Obligatoire. ID de la version d’évaluation de package pour laquelle vous voulez mettre à jour une soumission. Cet ID est disponible dans les données de réponse pour les demandes de [création d’un vol de packages](create-a-flight.md) et l' [extraction des vols de packages pour une application](get-flights-for-an-app.md). Pour un vol créé dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de vol de l’espace partenaires.  |
| submissionId | string | Obligatoire. ID de la soumission à mettre à jour. Cet ID est disponible dans les données de réponse pour les demandes de [création d’une soumission de vol de packages](create-a-flight-submission.md). Pour une soumission qui a été créée dans l’espace partenaires, cet ID est également disponible dans l’URL de la page de soumission dans l’espace partenaires.  |


### <a name="request-body"></a>Corps de la demande

Le corps de la requête contient les paramètres suivants.

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | tableau  | Contient des objets qui fournissent des détails sur chaque package de la soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de package de version d’évaluation](manage-flight-submissions.md#flight-package-object). Quand vous appelez cette méthode pour mettre à jour une soumission d’application, seules les valeurs *fileName*, *fileStatus*, *minimumDirectXVersion* et *minimumSystemRam* de ces objets sont nécessaires dans le corps de la requête. Les autres valeurs sont remplies par l’espace partenaires. |
| packageDeliveryOptions    | object  | Contient les paramètres de déploiement de package progressif et de mise à jour obligatoire de la soumission. Pour plus d’informations, consultez [Objet options de remise du package](manage-flight-submissions.md#package-delivery-options-object).  |
| targetPublishMode           | string  | Mode de publication pour la soumission. Il peut s’agir de l’une des valeurs suivantes : <ul><li>Immédiat</li><li>Manuel</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Date de publication de la soumission au format ISO 8601, si le paramètre *targetPublishMode* a la valeur SpecificDate.  |
| notesForCertification           | string  |  Fournit des informations supplémentaires aux testeurs de certification, telles que les informations d’identification du compte de test et les étapes permettant d’accéder aux fonctionnalités et de les vérifier. Pour plus d’informations, voir [Notes de certification](../publish/notes-for-certification.md). |


### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment mettre à jour une soumission de version d’évaluation de package pour une application.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la soumission mise à jour. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission de version d’évaluation du package](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | Impossible de mettre à jour la soumission de version d’évaluation de package, car la requête n’est pas valide. |
| 409  | Impossible de mettre à jour l’envoi de vols de packages en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gérer les soumissions de versions d’évaluation de package](manage-flight-submissions.md)
* [Obtenir une soumission de version d’évaluation du package](get-a-flight-submission.md)
* [Créer une soumission de version d’évaluation du package](create-a-flight-submission.md)
* [Valider une soumission de version d’évaluation de package](commit-a-flight-submission.md)
* [Supprime une soumission de version d’évaluation du package](delete-a-flight-submission.md)
* [Obtenir l’état d’une soumission de version d’évaluation de package](get-status-for-a-flight-submission.md)
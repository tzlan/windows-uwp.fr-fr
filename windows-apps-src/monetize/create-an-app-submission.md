---
ms.assetid: D34447FF-21D2-44D0-92B0-B3FF9B32D6F7
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer une soumission pour une application inscrite auprès de votre compte espace partenaires.
title: Créer une soumission d’applications
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de soumission, créer une soumission d’application
ms.localizationpriority: medium
ms.openlocfilehash: cbaf7e13c937762716bfd0cf35fbfdfb4776b1f1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158623"
---
# <a name="create-an-app-submission"></a>Créer une soumission d’applications

Utilisez cette méthode dans l’API de soumission Microsoft Store pour créer une soumission pour une application inscrite auprès de votre compte espace partenaires. Après avoir créé une soumission à l’aide de cette méthode, [mettez à jour cette soumission](update-an-app-submission.md) pour apporter les modifications nécessaires aux données de soumission, puis [validez la soumission](commit-an-app-submission.md) pour permettre son intégration et sa publication.

Pour plus d’informations sur la façon dont cette méthode s’intègre au processus de création d’une soumission d’application à l’aide de l’API de soumission Microsoft Store, consultez [gérer les envois d’applications](manage-app-submissions.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.
* Vérifiez que l’application a déjà fait l’objet d’au moins une soumission avec les informations de [classification par âge](../publish/age-ratings.md) spécifiées.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions` |

### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |

### <a name="request-parameters"></a>Paramètres de la demande

| Nom        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatoire. ID Windows Store de l’application pour laquelle vous voulez mettre à jour une soumission. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |

### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-example"></a>Exemple de requête

L’exemple suivant montre comment créer une soumission pour une application.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON d’un appel réussi à cette méthode. Le corps de la réponse contient des informations sur la nouvelle soumission. Pour plus d’informations sur les valeurs figurant dans le corps de la réponse, voir la [ressource de soumission d’application](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
           "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 400  | Impossible de créer la soumission, car la requête n’est pas valide. |
| 409  | Impossible de créer la soumission en raison de l’état actuel de l’application, ou l’application utilise une fonctionnalité d’espace partenaires qui n’est [pas prise en charge actuellement par le Microsoft Store API de soumission](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir une soumission d’application](get-an-app-submission.md)
* [Valider une soumission d’application](commit-an-app-submission.md)
* [Mettre à jour une soumission d’application](update-an-app-submission.md)
* [Supprimer une soumission d’application](delete-an-app-submission.md)
* [Obtenir l’état d’une soumission d’application](get-status-for-an-app-submission.md)
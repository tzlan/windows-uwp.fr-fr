---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Utilisez cette méthode dans l’API de soumission Microsoft Store pour récupérer les informations de vol de package pour une application inscrite auprès de votre compte espace partenaires.
title: Obtenir des versions d’évaluation du package pour une application
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, API de soumission Microsoft Store, vols, vols de packages
ms.localizationpriority: medium
ms.openlocfilehash: a5cfe7aa579e5d050ddf808e35c06d52c84eeca5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171433"
---
# <a name="get-package-flights-for-an-app"></a>Obtenir des versions d’évaluation du package pour une application

Utilisez cette méthode dans l’API de soumission Microsoft Store pour répertorier les vols de packages pour une application inscrite auprès de votre compte espace partenaires. Pour plus d’informations sur les versions d’évaluation du package, voir [Versions d’évaluation du package](../publish/package-flights.md).

## <a name="prerequisites"></a>Prérequis

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si vous ne l’avez pas déjà fait, renseignez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) pour l’API de soumission Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Une fois que vous avez récupéré le jeton d’accès, vous avez 60 minutes pour l’utiliser avant qu’il n’expire. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de demande                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisation | string | Obligatoire. Jeton d’accès Azure AD sous la forme **Bearer** &lt;*jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la demande

|  Nom  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  applicationId  |  string  |  ID Windows Store de l’application pour laquelle vous voulez récupérer les versions d’évaluation du package. Pour plus d’informations sur l’ID Windows Store, voir [Visualiser les informations d’identité des applications](../publish/view-app-identity-details.md).  |  Oui  |
|  top  |  int  |  Nombre d’éléments à retourner dans la requête (autrement dit, nombre de versions d’évaluation du package à retourner). Si votre compte comporte plus de versions d’évaluation du package que la valeur que vous spécifiez dans la requête, le corps de la réponse comprend un chemin d’URI relatif que vous pouvez ajouter à l’URI de la méthode pour solliciter la page suivante de données.  |  Non  |
|  skip  |  int  |  Nombre d’éléments à ignorer dans la requête avant de retourner les éléments restants. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, top=10 et skip=0 permettent de récupérer les éléments 1 à 10, top=10 et skip=10 permettent de récupérer les éléments 11 à 20, et ainsi de suite.  |  Non  |


### <a name="request-body"></a>Corps de la demande

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment répertorier toutes les versions d’évaluation du package pour une application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment répertorier la première version d’évaluation du package pour une application.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>response

L’exemple suivant illustre le corps de réponse JSON renvoyé par une requête réussie de la première version d’évaluation du package pour une application qui en comporte trois au total. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>Response body

| Valeur      | Type   | Description       |
|------------|--------|---------------------|
| @nextLink  | string | S’il existe des pages supplémentaires de données, cette chaîne contient un chemin relatif que vous pouvez ajouter à l’URI de requête `https://manage.devcenter.microsoft.com/v1.0/my/` de base pour solliciter la page suivante de données. Par exemple, si le paramètre *Top* du corps de la demande initiale est défini sur 2, mais qu’il y a 4 vols de packages pour l’application, le corps de la réponse inclut la @nextLink valeur `applications/{applicationid}/listflights/?skip=2&top=2` , qui indique que vous pouvez appeler `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2` pour demander les deux vols de packages suivants. |
| value      | tableau  | Tableau d’objets qui fournissent des informations sur les versions d’évaluation du package pour l’application spécifiée. Pour plus d’informations sur les données incluses dans chaque objet, voir [Ressource de version d’évaluation du package ](get-app-data.md#flight-object).               |
| totalCount | int    | Nombre total de lignes dans les résultats de données pour la requête (autrement dit, nombre total de versions d’évaluation du package pour l’application spécifiée).   |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Code d'erreur |  Description   |
|--------|------------------|
| 404  | Aucune version d’évaluation du package n’a été trouvée. |
| 409  | L’application utilise une fonctionnalité d’espace partenaires qui n’est [actuellement pas prise en charge par l’API de soumission Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenir toutes les applications](get-all-apps.md)
* [Obtenir une application](get-an-app.md)
* [Obtenir des extensions pour une application](get-add-ons-for-an-app.md)
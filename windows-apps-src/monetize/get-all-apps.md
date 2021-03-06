---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer des informations sur toutes les applications qui sont inscrits dans votre compte espace partenaires.
title: Obtenir toutes les applications
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, apps
ms.localizationpriority: medium
ms.openlocfilehash: 267e1d4de3917ae332cdfe15309f3871ef7b6647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641984"
---
# <a name="get-all-apps"></a>Obtenir toutes les applications


Utilisez cette méthode dans l’API de soumission de Microsoft Store pour récupérer des données pour les applications qui sont inscrits dans votre compte espace partenaires.

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser cette méthode, vous devez d’abord effectuer les opérations suivantes :

* Si ce n’est pas déjà le cas, remplissez toutes les [conditions préalables](create-and-manage-submissions-using-windows-store-services.md#prerequisites) relatives à l’API de soumission au Microsoft Store.
* [Obtenez un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) à utiliser dans l’en-tête de requête de cette méthode. Après avoir obtenu un jeton d’accès, vous avez 60 minutes pour l’utiliser avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en obtenir un nouveau.

## <a name="request"></a>Requête

Cette méthode présente la syntaxe suivante. Consultez les sections suivantes pour obtenir des exemples d’utilisation et une description de l’en-tête et du corps de la requête.

| Méthode | URI de requête                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications` |


### <a name="request-header"></a>En-tête de requête

| En-tête        | Type   | Description                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | chaîne | Obligatoire. Le jeton d’accès Azure AD sous la forme **PORTEUR** &lt; *jeton*&gt;. |


### <a name="request-parameters"></a>Paramètres de la requête

Tous les paramètres de la requête sont facultatifs pour cette méthode. Si vous appelez cette méthode sans paramètres, la réponse contient les données pour les 10 premières applications qui sont inscrites à votre compte.

|  Paramètre  |  Type  |  Description  |  Obligatoire  |
|------|------|------|------|
|  top  |  entier  |  Nombre d’éléments à retourner dans la requête (autrement dit, nombre d’applications à retourner). Si votre compte comporte plus d’applications que la valeur que vous spécifiez dans la requête, le corps de la réponse comprend un chemin d’URI relatif que vous pouvez ajouter à l’URI de la méthode pour solliciter la page suivante de données.  |  Non  |
|  skip  |  entier  |  Nombre d’éléments à ignorer dans la requête avant de retourner les éléments restants. Utilisez ce paramètre pour parcourir des ensembles de données. Par exemple, top=10 et skip=0 permettent de récupérer les éléments 1 à 10, top=10 et skip=10 permettent de récupérer les éléments 11 à 20, et ainsi de suite.  |  Non  |


### <a name="request-body"></a>Corps de la requête

Ne fournissez pas de corps de requête pour cette méthode.

### <a name="request-examples"></a>Exemples de demande

L’exemple suivant montre comment récupérer les 10 premières applications inscrites dans votre compte.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

L’exemple suivant montre comment récupérer des informations sur toutes les applications inscrites dans votre compte. Tout d’abord, obtenez les applications de 10 principaux :

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Puis à appeler de manière récursive `GET https://manage.devcenter.microsoft.com/v1.0/my/{@nextLink}` jusqu'à ce que `{@nextlink}` est null ou n’existe pas dans la réponse. Exemple :

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```
  
```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=20&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=30&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Si vous connaissez déjà le nombre total d’applications que vous avez dans votre compte, vous pouvez simplement transmettre ce nombre dans le **haut** pour obtenir des informations sur toutes vos applications.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=23 HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Réponse

L’exemple suivant montre le corps de réponse JSON renvoyé par une requête réussie sur les 10 premières applications inscrites dans un compte de développeur qui en possède 21 au total. Pour des raisons de concision, cet exemple affiche uniquement les données des deux premières applications retournées par la requête. Pour plus d’informations sur les valeurs figurant dans le corps de réponse, voir la section suivante.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>Corps de la réponse

| Valeur      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | tableau  | Tableau d’objets contenant des informations sur chaque application inscrite dans votre compte. Pour plus d’informations sur les données incluses dans chaque objet, voir [Ressource d’application](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | chaîne | S’il existe des pages supplémentaires de données, cette chaîne contient un chemin relatif que vous pouvez ajouter à l’URI de requête `https://manage.devcenter.microsoft.com/v1.0/my/` de base pour solliciter la page suivante de données. Par exemple, si le paramètre *top* du corps de requête initial a la valeur 10, mais qu’il existe 20 applications inscrites dans votre compte, le corps de réponse comprendra une valeur  @nextLink de `applications?skip=10&top=10`, ce qui indique que vous pouvez appeler `https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10` pour solliciter les 10 applications suivantes. |
| totalCount | entier    | Nombre total de lignes dans le résultat de données pour la requête (autrement dit, nombre total d’applications inscrites dans votre compte).                                                |


## <a name="error-codes"></a>Codes d’erreur

Si la requête ne peut pas aboutir, la réponse contient l’un des codes d’erreur HTTP suivants.

| Error code |  Description   |
|--------|------------------|
| 404  | Aucune application n’a été trouvée. |
| 409  | Les applications utilisent des fonctionnalités de partenaires qui sont [actuellement ne pas pris en charge par l’API de soumission de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtenez une application](get-an-app.md)
* [Obtenir les vols de package pour une application](get-flights-for-an-app.md)
* [Obtenir des modules complémentaires pour une application](get-add-ons-for-an-app.md)

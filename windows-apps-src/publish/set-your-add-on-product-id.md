---
author: jnHs
Description: When you create a new add-on in the Windows Dev Center dashboard, you need to specify a product type and assign it a product ID.
title: Définir le type et l’ID produit d’une extension
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, uwp, extensions, iap, durable, consommable, abonnement, type de produit, id produit, achat in-app, produit in-app
ms.localizationpriority: medium
ms.openlocfilehash: 0673048fc9a1ed8fb7c439607ebc4197039699e9
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3369445"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Définir le type et l’ID produit d’une extension

Une extension doit être associée à une application que vous avez déjà créée dans le tableau de bord (même si vous ne l’avez pas encore soumise). Recherchez le bouton **Créer un nouveau module complémentaire** sur la page **Vue d’ensemble** ou **Modules complémentaires** de votre application.

Après avoir sélectionné **Créer un nouveau module complémentaire**, vous serez invité à spécifier un type de produit et à attribuer un ID de produit pour votre extension.

## <a name="product-type"></a>Type de produit

Vous devez commencer par indiquer le type de module complémentaire que vous proposez. Cette sélection détermine la façon dont le client pourra utiliser votre extension.

> [!NOTE]
> Vous ne pourrez pas modifier le type de produit après avoir enregistré cette page pour créer l’extension. Si vous choisissez un type de produit incorrect, vous pouvez toujours supprimer votre soumission d’extension en cours et recommencer en créant une autre extension.

<span id="durable" />

### <a name="durable"></a>Durable

Sélectionnez **Durable** comme type de produit si votre extension n’est généralement achetée qu’une seule fois. Ces extensions servent généralement à déverrouiller des fonctionnalités supplémentaires d’une application.

Par défaut, le champ **Durée de vie du produit** d’une extension durable affiche la valeur **Toujours**, ce qui signifie que cette extension n’expire jamais. Vous avez la possibilité de définir le champ **Durée de vie du produit** sur une autre durée à l’étape [Propriétés](enter-add-on-properties.md) du processus de soumission de l’extension. Si vous procédez ainsi, l’extension arrivera à expiration au terme de la durée que vous spécifiez (comprise entre 1 et 365jours), auquel cas un client pourra la racheter après son expiration.

<span id="consumable" />

### <a name="consumable"></a>Consommable

Si l’extension peut être achetée, utilisée (consommée), puis rachetée, vous devez sélectionner l’un des types de produits **consommables**. Les modules complémentaires consommables sont souvent utilisés pour la monnaie d’un jeu par exemple (or, pièces, etc.), qui peuvent être achetés en montants prédéfinis puis dépensés par le client. Pour plus d’informations, voir [Activer les achats d’extensions consommables](../monetize/enable-consumable-add-on-purchases.md).

Il existe deux types d’extensions consommables:
- **Consommable géré par le développeur**: le solde et l’acquisition doivent être gérés dans votre application. Pris en charge dans toutes les versions de système d’exploitation.
- **Consommable géré par le Windows store:** Microsoft assure le suivi du solde sur tous les appareils du client fonctionnant sous Windows10, version1607 ou ultérieure; non pris en charge sur les versions antérieures du système d’exploitation. Pour que cette option soit utilisable, le produit parent doit être compilé à l’aide du Kit de développement logiciel (SDK) Windows10 version14393 ou ultérieure. Notez également que vous ne pouvez pas soumettre au WindowsStore une extension consommable gérée par le WindowsStore tant que le produit parent n’a pas été publié (toutefois, vous pouvez créer la soumission dans votre tableau de bord, puis commencer à travailler dessus à tout moment). Vous devez renseigner la quantité concernant votre extension consommable gérée par le WindowsStore à l’étape **Propriétés** de votre soumission.

<span id="subscription" />

### <a name="subscription"></a>Abonnement

Si vous souhaitez facturer les clients de manière récurrente pour votre extension, choisissez **Abonnement**.

Une fois qu’un client a acquis une extension d’abonnement, il est facturé à intervalles réguliers afin de continuer à utiliser l’extension. Le client peut annuler l’abonnement à tout moment pour éviter d’autres frais. Vous devez spécifier la période d’abonnement et choisir de proposer ou non un essai gratuit à l’étape **Propriétés** de votre soumission.

Les extensions d’abonnement sont uniquement prises en charge pour les clients qui exécutent Windows10, version1607 ou ultérieure. L’application parente doit être compilée à l’aide du SDK Windows10, version14393 ou ultérieure, et elle doit utiliser l’API d’achat in-app de l’espace de noms **Windows.Services.Store** en lieu et place de l’espace de noms **Windows.ApplicationModel.Store**. Pour plus d’informations, consultez l’article [Activer les extensions d’abonnement de votre application](../monetize/enable-subscription-add-ons-for-your-app.md).

Vous devez soumettre le produit parent avant de publier des extensions d’abonnement dans le Store (toutefois, vous pouvez créer la soumission dans votre tableau de bord et commencer à travailler dessus à tout moment).

## <a name="product-id"></a>ID produit

Quel que soit le type de produit que vous choisissez, vous devez entrer un ID produit unique pour votre extension. Ce nom servira à identifier votre extension dans le tableau de bord, et vous pourrez utiliser cet identificateur pour [faire référence à l’extension dans votre code](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

Voici quelques éléments à prendre en considération lors du choix d’un ID produit:

-   Les clients ne verront pas cet ID produit. (Par la suite, vous pourrez entrer un [titre et une description](create-add-on-descriptions.md) qui seront visibles par les clients.)
-   Vous ne pouvez plus modifier ni supprimer l’ID produit d’un module complémentaire après la publication de ce dernier.
-   Un ID produit ne peut pas comporter plus de 100caractères.
-   Un ID produit ne peut comporter aucun de ces caractères: **&lt; &gt; \* % & : \\ ? + ,**
-   Pour offrir votre module complémentaire sur toutes les versions de système d’exploitation, utilisez uniquement des caractères alphanumériques, des points et/ou des traits de soulignement. Si vous utilisez d’autres types de caractère, le module complémentaire ne sera pas disponible à l’achat pour les clients utilisant WindowsPhone8.1 ou une version antérieure.
-   Un ID produit ne doit pas nécessairement être unique dans le Microsoft Store, mais doit l’être dans votre compte de développeur.
 
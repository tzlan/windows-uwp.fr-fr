---
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: Déclarations de produit
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1e17fbd81c84ca4ce72d36dbabf9991fe8c6d75d
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8700067"
---
# <a name="product-declarations"></a>Déclarations de produit

La section **déclarations de produit** de la page de [Propriétés](enter-app-properties.md) du [processus de soumission](app-submissions.md) permet de vous assurer que votre application s’affiche correctement et proposée à un ensemble approprié de clients et permet de les comprennent comment ils peuvent utiliser votre application.

Les sections suivantes décrivent certaines des déclarations et ce que vous devez prendre en compte lors de la détermination si chaque déclaration s’applique à votre application. Notez que deux de ces déclarations sont vérifiées par défaut (comme décrit ci-dessous). En fonction de la catégorie de votre produit, vous pouvez également voir des déclarations supplémentaires. Veillez à passer en revue toutes les déclarations et vous assurer qu’ils reflètent précisément votre soumission.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Cette application permet aux utilisateurs d’effectuer des achats, mais n’utilise pas le système de commerce Microsoft Store.

Pour presque chaque soumission, vous devez laisser cette case désactivée, depuis les applications qui offrent des possibilités d’acheter des éléments qui sont ou peuvent être consommés ou utilisés au sein de votre application doivent utiliser l’API d’achat dans l’application Microsoft Store pour créer et soumettre les modules complémentaires. Par le [Contrat développeur d’applications](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), les applications qui ont été créées et soumises avant le 29 juin 2015, peuvent continuer à offrir la fonctionnalité d’achat dans l’application sans utiliser le moteur de commerce de Microsoft, tant que la fonctionnalité d’achat est conforme à la [ Politiques du Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Si cela s’applique à votre application, vous devez activer cette case. Sinon, laissez-la désactivée.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Cette application a fait l'objet de tests pour voir si elle est conforme aux recommandations d'accessibilité.

En cochant cette case, vous rendez votre application détectable par les clients qui recherchent tout particulièrement des applications accessibles dans le Windows Store.

Cochez uniquement cette case si vous avez :

-   défini toutes les informations d'accessibilité relatives aux éléments de l'interface utilisateur, comme les noms accessibles ;
-   implémenté la navigation et l'utilisation à partir du clavier en tenant compte de l'ordre des onglets, de l'activation du clavier, de la navigation à l'aide des touches de direction et des raccourcis ;
-   vérifié la présence d'une expérience visuelle accessible respectant notamment un coefficient de contraste de texte de 4.5:1 et n'utilisant pas simplement la couleur pour transmettre des informations à l'utilisateur ;
-   utilisé des outils de test d'accessibilité, comme Inspect ou AccChecker, afin de vérifier votre application et de résoudre les erreurs de priorité élevée détectées par ces outils ;
-   vérifié les scénarios clés de votre application de bout en bout à l'aide de fonctionnalités et d'outils tels que le Narrateur, la Loupe, le clavier sur écran, le contraste élevé et la haute résolution.

Lorsque vous déclarez votre application comme étant accessible, vous certifiez qu'elle est accessible à tous les utilisateurs, y compris ceux souffrant de handicaps. Cela signifie, par exemple, que vous avez testé l'application avec le mode de contraste élevé et avec un lecteur d'écran. Vous avez également vérifié que l’interface utilisateur fonctionnait correctement avec un clavier, la Loupe et d’autres outils d’accessibilité.

Pour plus d’informations, voir [l’accessibilité](../design/accessibility/accessibility.md), [test d’accessibilité](../design/accessibility/accessibility-testing.md)et [l’accessibilité dans le Windows Store](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> Pas de liste votre application comme accessible, sauf si vous avez spécifiquement conçue et testée à cet effet. Si votre application est déclarée comme étant accessible, mais qu’elle ne prend pas réellement en charge l’accessibilité, vous allez probablement recevoir des commentaires négatifs de la part de la communauté.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Les clients peuvent installer cette application sur d'autres disques ou dispositifs de stockage amovible.

Cette case est cochée par défaut, pour permettre aux clients d’installer votre application dans le stockage amovible ou externe du lecteur multimédia tel qu’une carte SD ou un volume non système, comme un lecteur externe.

Si vous souhaitez empêcher que votre application en cours d’installation pour les autres lecteurs ou le stockage amovible et autoriser uniquement l’installation sur le disque dur interne sur leur appareil, décochez cette case. (Notez qu’il n’existe aucune option pour restreindre installation afin qu’une application peut *uniquement* être installé sur un support de stockage amovible).


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows peut intégrer les données de cette application dans les sauvegardes automatiques sur OneDrive.

Cette case est cochée par défaut pour permettre l'insertion des données de votre application quand un client choisit de paramétrer Windows pour des sauvegardes automatiques sur OneDrive.

Si vous voulez empêcher l’insertion des données de votre application dans les sauvegardes automatiques, décochez cette case.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Cette application envoie des données de Kinect à des services externes. 

Si votre application utilise les données de Kinect et l’envoie à n’importe quel service externe, vous devez activer cette case.



 

 

 




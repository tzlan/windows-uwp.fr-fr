---
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: Vue d’ensemble des technologies de réseau disponibles pour un développeur UWP, avec des suggestions sur la façon de choisir les technologies appropriées pour votre application.
title: Quelle technologie de réseau ?
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3f14e06f5e6f7508c90df9f04265740daaccb49
ms.sourcegitcommit: b99e2f4dffa603b68c2a8273fe6313432f91b353
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90569382"
---
# <a name="which-networking-technology"></a>Quelle technologie de réseau ?

Vue d’ensemble des technologies de réseau disponibles pour un développeur UWP, avec des suggestions sur la façon de choisir les technologies appropriées pour votre application.

## <a name="sockets"></a>Sockets

Utilisez des [sockets](sockets.md) quand vous communiquez avec un autre appareil et souhaitez utiliser votre propre protocole.

Deux implémentations de sockets sont à la disposition des développeurs pour la plateforme Windows universelle (UWP) : [**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets) et [Winsock](/windows/desktop/WinSock/windows-sockets-start-page-2). Si vous écrivez un nouveau code, Windows.Networking.Sockets présente l’avantage d’être une API moderne, conçue à l’usage des développeurs UWP. Si vous utilisez des bibliothèques réseau inter-plateforme ou un autre code Winsock existant, ou préférez l’API Winsock, utilisez-la.

### <a name="when-to-use-sockets"></a>Quand utiliser des sockets

-   Les deux implémentations de sockets vous permettent de communiquer avec d’autres appareils à l’aide des protocoles de votre choix, en utilisant TCP ou UDP.

-   Choisissez les API de sockets qui répondent le mieux à vos besoins en fonction de l’expérience et de tout code existant que vous pourriez utiliser.

### <a name="when-not-to-use-sockets"></a>Quand ne pas utiliser de sockets

-   N’implémentez votre propre pile HTTP(S) à l’aide de sockets. Utilisez plutôt [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient).
-   Si les WebSockets (classes [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) et [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket)) répondent à vos besoins de communications (TCP vers/depuis un serveur web), envisagez de les utiliser au lieu de consacrer votre temps et vos ressources de développement à implémenter des fonctionnalités similaires avec des sockets.

## <a name="websockets"></a>WebSockets

Le protocole [WebSocket](websockets.md) définit un mécanisme de communication bidirectionnelle, sécurisée et rapide entre un client et un serveur sur le Web. Les données sont transférées immédiatement via une connexion duplex intégral à un seul socket, ce qui permet d’envoyer et de recevoir les messages à partir des deux points de terminaison en temps réel. Les WebSockets conviennent parfaitement pour les jeux en temps réel, où les notifications instantanées de réseau social et les affichages actualisés d’informations (telles que des statistiques de jeu) doivent être sécurisés et utiliser un transfert de données rapide. Les développeurs UWP peuvent utiliser les classes [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) et [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket) pour se connecter à des serveurs qui prennent en charge le protocole Websocket.

### <a name="when-to-use-websockets"></a>Quand utiliser les Websockets

-   Lorsque vous souhaitez envoyer et recevoir des données de manière continue entre un appareil et un serveur.

### <a name="when-not-to-use-websockets"></a>Quand ne pas utiliser les WebSocket

-   Si vous envoyez ou recevez des données rarement, il peut s’avérer plus simple d’effectuer des requêtes HTTP individuelles à partir de l’appareil vers le serveur, au lieu d’établir et maintenir une connexion de WebSocket.
-   Les WebSockets peuvent ne pas convenir pour des situations où les volumes traités sont importants. Songez à modéliser vos flux de données et à simuler votre trafic via des WebSockets avant de valider leur utilisation dans votre conception.

## <a name="httpclient"></a>HttpClient

Utilisez [HttpClient](httpclient.md) (et le reste de l’API d’espace de noms [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)) lorsque vous utilisez HTTP(S) pour communiquer avec un service ou un serveur web.

### <a name="when-to-use-httpclient"></a>Quand utiliser HttpClient

-   Lorsque vous utilisez HTTP(S) pour communiquer avec des services web.
-   Lors du chargement ou téléchargement d’un petit nombre de fichiers plus petits.
-   Si les WebSockets (classes [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) et [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket)) répondent à vos besoins de communications (TCP vers/depuis un serveur Web), et si le serveur Web en question prend en charge les WebSockets, envisagez de les utiliser au lieu de consacrer votre temps et vos ressources de développement à implémenter des fonctionnalités similaires avec HttpClient.
-   Lorsque vous diffusez du contenu sur le réseau.

### <a name="when-not-to-use-httpclient"></a>Quand ne pas utiliser HttpClient

-   Si vous transférez des fichiers volumineux ou en grand nombre, songez à utiliser des transferts en arrière-plan à la place.
-   Si vous voulez être en mesure de retreindre les limites de chargement/téléchargement selon le type de connexion, ou si vous souhaitez enregistrer la progression et la reprise du chargement/téléchargement après une interruption, vous devez utiliser les transferts en arrière-plan.
-   Si vous communiquez entre deux appareils dont aucun n’est conçu pour fonctionner en tant que serveur HTTP(S), vous devez utiliser des sockets. N’essayez pas d’implémenter votre propre serveur HTTP et d’utiliser [HttpClient](httpclient.md) de communiquer avec lui.

## <a name="background-transfers"></a>Transferts en arrière-plan

Utilisez l’[API de transfert en arrière-plan](background-transfers.md) lorsque vous voulez transférer de manière fiable des fichiers sur le réseau. L’API de transfert en arrière-plan offre des fonctionnalités avancées de chargement et téléchargement, qui s’exécutent en arrière-plan pendant la suspension d’une application, et perdurent après l’arrêt de l’application. L’API surveille l’état du réseau. Elle suspend et reprend automatiquement les transferts en cas de perte de connexion. Les transferts sont par ailleurs régis par l’Assistant Données et l’Assistant batterie, ce qui signifie que l’activité de téléchargement s’ajuste en fonction de l’état actuel de la batterie de l’appareil et de la connexion. Ces fonctionnalités sont essentielles lorsque votre application s’exécute sur des appareils mobiles ou alimentés par batterie. L’API est idéale pour le chargement et le téléchargement de fichiers volumineux à l’aide du protocole HTTP(S). Le protocole FTP est également pris en charge, mais uniquement pour les téléchargements.

Pour le transfert en arrière-plan dans Windows 10, il est désormais possible de déclencher un post-traitement à la fin d’un transfert de fichiers, afin de mettre à jour des catalogues locaux, activer d’autres applications ou avertir l’utilisateur quand un téléchargement est terminé.

### <a name="when-to-use-background-transfers"></a>Quand utiliser des transferts en arrière-plan

-   Utilisez des transferts en arrière-plan pour transférer de manière fiable des fichiers volumineux ou en grand nombre.
-   Utilisez des transferts en arrière-plan avec des groupes d’achèvement de transfert en arrière-plan quand vous souhaitez post-traiter les transferts de fichiers à l’aide d’une tâche en arrière-plan.
-   Utilisez les transferts en arrière-plan si vous voulez être en mesure de reprendre un transfert en cours après une interruption du réseau.
-   Utilisez les transferts en arrière-plan si vous voulez être en mesure de modifier le comportement de transfert en fonction des conditions réseau, par exemple lorsque vous utilisez un plan de facturation à l’usage.

### <a name="when-not-to-use-background-transfers"></a>Quand ne pas utiliser des transferts en arrière-plan

-   Si vous transférez un petit nombre de petits fichiers, et n’avez pas besoin d’effectuer de post-traitement à la fin du transfert, songez à utiliser les méthodes [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) PUT ou POST.
-   Si vous voulez diffuser des données et les utiliser en local dès leur arrivée à destination, utilisez [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient).

## <a name="additional-network-related-technologies"></a>Technologies supplémentaires liées au réseau

### <a name="connection-quality"></a>Qualité de la connexion

Les API dans l’espace de noms [**Windows.Networking.Connectivity**](/uwp/api/Windows.Networking.Connectivity) vous permettent d’accéder aux informations de connectivité, de coût et d’utilisation du réseau. Pour plus d’informations sur l’utilisation de ces API, consultez [Accès aux informations d’état de la connexion réseau et gestion des coûts de la connexion réseau](/previous-versions/windows/apps/hh452985(v=win.10)).

### <a name="dns-service-discovery"></a>Découverte de service DNS

L’API [**Windows.Networking.ServiceDiscovery.Dnssd**](/uwp/api/Windows.Networking.ServiceDiscovery.Dnssd) vous permet d’annoncer un service réseau à d’autres appareils sur le réseau en utilisant le protocole DNS-SD décrit dans IETF RFC[RFC 2782](https://www.rfc-archive.org/getrfc.php?rfc=2782).

### <a name="communicating-over-bluetooth"></a>Communication par Bluetooth

Entre autres choses, l’API [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth) vous permet d’utiliser la technologie Bluetooth pour vous connecter à d’autres appareils et transférer des données. Pour plus d’informations, voir [Envoyer ou recevoir des fichiers avec RFCOMM](../devices-sensors/send-or-receive-files-with-rfcomm.md).

### <a name="push-notifications-wns"></a>Notifications Push (WNS)

L’API [**Windows.Networking.PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) vous permet d’utiliser le service de notification Windows (WNS) pour recevoir des notifications Push via le réseau. Pour plus d’informations sur l’utilisation de cette API, consultez [Vue d’ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)

### <a name="near-field-communications"></a>Communication en champ proche (NFC)

L’API [**Windows.Networking.Proximity**](/uwp/api/Windows.Networking.Proximity) vous permet d’utiliser la technologie de communication en champ proche pour des applications qui font appel à la fonctionnalité de proximité ou d’effleurement avec des appareils pour permettre un transfert aisé des données. Pour plus d’informations sur l’utilisation de cette API, consultez [Proximité et geste tactile](/previous-versions/windows/apps/hh465221(v=win.10)).

### <a name="rssatom-feeds"></a>Flux RSS/Atom

L’API [**Windows.Web.Syndication**](/uwp/api/Windows.Web.Syndication) vous permet de gérer des flux de syndication à l’aide de formats RSS et Atom. Pour plus d'informations sur l'utilisation de cette API, voir [Flux RSS/Atom](web-feeds.md).

### <a name="wi-fi-enumeration-and-connection-control"></a>Contrôle d’énumération et de connexion Wi-Fi

L’API [**Windows.Devices.WiFi**](/uwp/api/Windows.Devices.WiFi) vous permet d’énumérer les cartes Wi-Fi, de rechercher des réseaux Wi-Fi disponibles, et de connecter une carte à un réseau.

### <a name="radio-control"></a>Contrôle radio

L’API [**Windows.Devices.Radios**](/uwp/api/Windows.Devices.Radios) vous permet de détecter et contrôler les signaux radios sur l’appareil local, notamment Wi-Fi et Bluetooth.

### <a name="wi-fi-direct"></a>Wi-Fi Direct

L’API [**Windows.Devices.WiFiDirect**](/uwp/api/Windows.Devices.WiFiDirect) vous permet de vous connecter à d’autres appareils locaux et de communiquer avec eux en utilisant Wi-Fi Direct pour créer des réseaux locaux sans fil ad hoc.

### <a name="wi-fi-direct-services"></a>Services Wi-Fi Direct

L’API [**Windows.Devices.WiFiDirect.Services**](/uwp/api/Windows.Devices.WiFiDirect.Services) vous permet de fournir des services Wi-Fi Direct et de vous y connecter. Les services Wi-Fi Direct permettent à un appareil sur un réseau ad hoc Wi-Fi Direct (annonceur de service) d’offrir des fonctionnalités à un autre appareil (demandeur de service) via une connexion Wi-Fi Direct.

### <a name="mobile-operators"></a>Opérateurs mobiles

Windows 10 expose à un vaste public de développeurs des API qui ont étaient précédemment exposées uniquement aux fabricants d’appareils et aux opérateurs mobiles. Notez que bien, si ces API sont exposées maintenant, elles sont également contrôlées par des fonctionnalités d’application spécifiques qui doivent être approuvées par Microsoft avant qu’une application puisse être publiée. L’utilisation réelle de ces API reste limitée principalement aux fabricants d’appareils et aux opérateurs mobiles.

### <a name="network-operations"></a>Opérations réseau

L’API [**Windows.Networking.NetworkOperators**](/uwp/api/Windows.Networking.NetworkOperators) a trait principalement à la configuration et la mise en service des téléphones. De ce fait, l’autorisation d’utiliser les fonctionnalités qui la contrôlent sont limitées aux fabricants d’appareils et aux fournisseurs de télécommunications.

### <a name="sms"></a>SMS

L’espace de noms [**Windows.Devices.Sms**](/uwp/api/Windows.Devices.Sms) a trait aux SMS et aux messages similaires en tant qu’entités de bas niveau. Il est fourni à l’usage des opérateurs mobiles pour une utilisation des SMS orientée application, et est contrôlé par une fonctionnalité dont l’utilisation ne sera pas approuvée par la plupart des développeurs d’applications. Si vous écrivez une application pour traiter des messages, vous devez utiliser l’API [**Windows.ApplicationModel.Chat**](/uwp/api/Windows.ApplicationModel.Chat) à la place, qui est conçue pour gérer non seulement les messages SMS, mais aussi les messages provenant d’autres sources telles que des applications de conversation en temps réel, en permettant une expérience de conversation/messagerie beaucoup plus riche.
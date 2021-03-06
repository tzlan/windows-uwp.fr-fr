---
title: Développement d’applications pour Windows as a service
description: Découvrez l’approche Windows as a Service (WaaS) concernant l’innovation, le développement et la distribution, ainsi que le programme de test Windows Insider centré sur la communauté.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f384ca56-f2b2-4793-b251-f7f5735376bb
ms.localizationpriority: medium
ms.openlocfilehash: d301a2dfcc478f6d90b8aa562bfbd34cf79493c1
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750135"
---
# <a name="application-development-for-windows-as-a-service"></a>Développement d’applications pour Windows as a service

**S’applique à**
-   Windows 10
-   Windows 10 Mobile
-   Windows 10 IoT Standard 

Dans l’environnement d’aujourd’hui, où les attentes des utilisateurs sont souvent déterminées par des expériences liées à des appareils spécifiques, les cycles de produit complets se mesurent en mois plutôt qu’en années. En outre, de nouvelles versions doivent être rendues disponibles en permanence, dont le déploiement ne doit avoir qu’un impact minimal sur les utilisateurs. Microsoft a conçu Windows 10 pour répondre à ces exigences, en implémentant une nouvelle approche en termes d’innovation, de développement et de livraison appelée [Windows as a service (WaaS)](/windows/deployment/update/waas-overview). La clé de la mise en place de cycles produit sensiblement plus courts tout en conservant des niveaux élevés de qualité est une approche innovante de la phase de test, centrée sur une communauté d’initiés, que Microsoft a implémentée pour Windows 10. Cette communauté d’initiés, appelés « Windows Insiders », compte des millions d’utilisateurs dans le monde entier. Les Windows Insiders qui décident de rejoindre la communauté testent de nombreuses builds tout au long du cycle d’un produit, et leur feedback apporte à Microsoft un retour d’expérience via une méthodologie itérative nommée distribution de version d’évaluation.

Les builds distribuées en tant que versions d’évaluation permettent à l’équipe d’ingénieurs de Windows de recueillir des données importantes relatives aux performances des builds dans des conditions d’utilisation réelle. La distribution de version d’évaluation, avec la collaboration des Windows Insiders, permet également à Microsoft de tester les builds dans des environnements matériels, d’applications et de réseau sensiblement plus variés que par le passé, et d’identifier les problèmes beaucoup plus rapidement. Par conséquent, Microsoft estime que la phase de distribution de version d’évaluation axée sur la communauté permettra plus que jamais d’accélérer le rythme des innovations et d’améliorer la qualité des versions publiques.

## <a name="windows-10-release-types-and-cadences"></a>Types et cadences des versions de Windows 10

Parallèlement à la publication de builds de version d’évaluation pour les Windows Insiders, Microsoft publie en permanence deux types de versions de Windows 10 destinées au grand public :

Les **mises à jour des fonctionnalités** installent les nouvelles fonctions, expériences et fonctionnalités sur des appareils exécutant déjà Windows 10. Ces mises à jour contenant une copie complète de Windows, elles permettent également aux clients d’installer Windows 10 sur des appareils exécutant déjà Windows 7 ou Windows 8.1, et sur de nouveaux appareils où aucun système d’exploitation n’est encore installé. Microsoft prévoit de publier des mises à jour semestrielles. 

Les **mises à jour qualité** contiennent les résolutions des problèmes de sécurité et des correctifs pour d’autres bogues importants. Elles sont fournies pour améliorer chaque fonctionnalité actuellement prise en charge, à une cadence d’une à plusieurs fois par mois. Microsoft continuera à publier des mises à jour qualité le mardi (parfois appelées « Update Tuesday » ou « Patch Tuesday »). En outre, Microsoft peut publier des mises à jour qualité supplémentaires pour Windows 10 en dehors des mardis si nécessaire, pour répondre aux besoins des clients.

Au cours du développement de Windows 10, Microsoft a simplifié l’ingénierie et le cycle de publication des produits Windows, afin de pouvoir fournir plus rapidement les fonctions, expériences et fonctionnalités souhaitées par les clients. Nous avons également créé de nouvelles façons de fournir et d’installer les mises à jour des fonctionnalités et les mises à jour qualité, qui simplifient les déploiements et la gestion continue, élargissent la base des employés qui peuvent être tenus informés des dernières fonctionnalités et expériences de Windows, et qui réduisent le coût total de possession. Nous avons donc implémenté de nouvelles options de maintenance (appelées Canal semi-annuel et Long-Term Servicing Channel (LTSC)) qui offrent des solutions pragmatiques permettant de maintenir plus d’appareils à jour dans les environnements d’entreprise qu’auparavant.

Le tableau ci-après décrit les différents canaux de maintenance et leurs principaux attributs.

| Option de maintenance | Disponibilité de nouvelles mises à niveau de fonctionnalités pour installation | Durée de vie de maintenance | Principaux avantages | Éditions prises en charge |
| --- | --- | --- | --- | --- |
| Canal semi-annuel (ciblé) | Immédiatement après la première publication par Microsoft | 18 mois | Met les nouvelles fonctionnalités à la disposition des utilisateurs dès que possible | Famille, Professionnel, Éducation, Entreprise, Mobile, IoT Standard, Windows 10 IoT Standard Professionnel (IoT Standard Professionnel) |
| Canal semi-annuel | Environ 4 mois après la première publication par Microsoft | 18 mois à compter de la première publication | Offre plus de temps pour tester les nouvelles mises à niveau de fonctionnalités avant le déploiement | Professionnel, Éducation, Entreprise, Mobile Entreprise, IoT Standard Professionnel |
| Canal de maintenance à long terme (LTSC) | Immédiatement après la publication par Microsoft | 10 ans | Permet le déploiement à long terme de versions sélectionnées de Windows 10 dans des configurations impliquant peu de changements | Entreprise LTSB |

Pour plus d’informations, consultez [Options de maintenance de Windows 10 pour les mises à jour et les mises à niveau](/windows/deployment/update/waas-overview#servicing-channels).

## <a name="supporting-apps-in-windows-as-a-service"></a>Prise en charge des applications dans Windows as a service

L’approche traditionnelle de prise en charge des applications consiste à publier une nouvelle version de l’application en réponse à une publication de Windows. Elle part du principe qu’il existe des modifications fondamentales dans le système d’exploitation sous-jacent, qui peuvent potentiellement provoquer une régression de l’application. Ce modèle implique un cycle de développement et de validation dédié qui nécessite que nos partenaires éditeurs de logiciels indépendants s’alignent sur la cadence de publication de Windows.

Dans le modèle Windows as a service, Microsoft s’engage à maintenir la compatibilité du système d’exploitation sous-jacent. Cela signifie que Microsoft s’efforce, dans une démarche concertée, de n’introduire aucune modification cassante nuisant à l’écosystème d’applications. Dans ce scénario, quand une nouvelle build de Windows est publiée, la plupart des applications (celles qui sont dépourvues de dépendances vis-à-vis du noyau) continuent de fonctionner.

Dans la perspective de ce changement, nous recommandons à nos partenaires éditeurs de logiciels indépendants de dissocier la publication et le support de leur application des builds Windows spécifiques. Une approche de type cycle de vie des applications convient mieux à nos clients mutuels. Cela signifie que quand une version d’une application est publiée, elle reste prise en charge pendant une période donnée, indépendamment du nombre de versions Windows publiées pendant ce laps de temps. L’éditeur de logiciels indépendant s’engage à assurer la prise en charge de cette version spécifique de l’application tant qu’elle est prise en charge dans le cycle de vie. Pour Windows, Microsoft suit une approche du cycle de vie similaire, qui peut être référencée [ici](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy?C2=14019).

Cette approche permet de réduire la charge de la maintenance d’une planification d’application qui s’aligne sur les publications de Windows. Les partenaires éditeurs de logiciels indépendants doivent pouvoir publier librement des fonctionnalités ou des mises à jour à leur propre rythme. Nous pensons que nos partenaires peuvent tenir informée leur clientèle des dernières mises à jour des applications indépendamment d’une publication de Windows. Par ailleurs, nos clients n’ont pas à rechercher une déclaration de prise en charge explicite à chaque publication d’une build Windows. Voici un exemple de déclaration de prise en charge qui indique dans quelle mesure une application peut être prise en charge dans différentes versions du système d’exploitation :

> **Exemple de déclaration de prise en charge du cycle de vie d’une application**
>
> La société de développement logiciel Contoso est propriétaire de l’application populaire Mojave, qui occupe une place importante dans l’entreprise. Contoso publie sa prochaine version majeure de Mojave 14.0 et déclare une prise en charge standard de trois ans à compter de la date de publication. Pendant la prise en charge standard, toutes les mises à jour et le support sont gratuits pour le produit sous licence. Contoso déclare également deux années supplémentaires de support étendu, pendant lesquelles les clients peuvent acheter des mises à jour et bénéficier du support pendant une période de grâce. Au-delà de la date de fin du support étendu, le support de cette version du produit n’est plus assuré. Pendant la période de prise en charge standard, Contoso prend en charge Mojave 14.0 sur l’ensemble des builds de Windows publiées. Contoso publie également des mises à jour pour Mojave selon les besoins et indépendamment des versions du produit Windows.

Les sections suivantes contiennent des informations supplémentaires sur les mesures prises par Microsoft pour maintenir la compatibilité du système d’exploitation sous-jacent. Vous y trouverez également des conseils sur les mesures que vous pouvez prendre pour maintenir la compatibilité du système d’exploitation et de l’écosystème des applications. Une section indique également comment tirer parti des builds de distribution de version d’évaluation de Windows afin de détecter les régressions d’application avant la publication d’une build Windows. Pour finir, nous expliquons comment utiliser une approche pilotée par l’instrumentation et la télémétrie afin d’augmenter la qualité des builds Windows. Nous recommandons aux éditeurs de logiciels indépendants d’adopter une approche similaire avec leur portefeuille d’applications.

## <a name="key-changes-since-windows7-to-ensure-app-compatibility"></a>Principales modifications depuis Windows 7 pour assurer la compatibilité des applications

Nous comprenons que la compatibilité est importante pour les développeurs. Les éditeurs de logiciels indépendants et les développeurs veulent garantir que leurs applications s’exécuteront comme prévu sur toutes les versions prises en charge du système d’exploitation Windows. L’investissement des consommateurs et des entreprises est essentiel ici : ils veulent être sûrs que les applications qu’ils ont payées continueront de fonctionner. Nous savons que la compatibilité est le principal critère motivant les décisions d’achat. Les applications qui sont écrites correctement en respectant les bonnes pratiques entraînent beaucoup moins d’évolution du code quand une nouvelle version de Windows est publiée et réduisent la fragmentation. Pour ces applications, l’investissement en termes d’ingénierie est réduit pour la maintenance, et le délai de commercialisation est plus court.

À l’époque de Windows 7, la compatibilité était une approche extrêmement réactive. Dans Windows 8, nous avons commencé à l’envisager différemment, en travaillant dans Windows pour nous assurer que la compatibilité tenait plus de la conception que d’un ajout ultérieur. Windows 10 est la version du système d’exploitation relevant de la conception la plus compatible à ce jour. Voici quelques moyens clés qui nous ont permis d’atteindre cet objectif :
-   **Télémétrie applicative** : Elle nous permet de comprendre la popularité des applications dans l’écosystème Windows afin de fournir des informations sur les tests de compatibilité.
-   **Partenariats avec des éditeurs de logiciels indépendants** : Nous collaborons directement avec des partenaires externes pour leur fournir des données et les aider à résoudre les problèmes rencontrés par nos utilisateurs.
-   **Vérifications de la conception, détection en amont** : Nous travaillons en partenariat avec les équipes chargées des fonctionnalités pour réduire le nombre de modifications fondamentales dans Windows. La vérification de la compatibilité est l’une des étapes que doivent effectuer nos équipes chargées des fonctionnalités.
-   **Communication** : Contrôle plus étroit des modifications des API et communication améliorée.
-   **Distribution de version d’évaluation et système de remontée de l’information** : Les Windows Insiders reçoivent des builds avec version d’évaluation, qui nous aident à améliorer notre capacité à trouver les problèmes de compatibilité avant la publication d’une version finale. Ce processus de feedback ne révèle pas seulement les bogues : il nous permet également de garantir que nous fournissons à nos clients les fonctionnalités qu’ils souhaitent.

## <a name="best-practices-for-app-compatibility"></a>Bonnes pratiques en matière de compatibilité des applications

Nous utilisons des données de diagnostic et d’utilisation pour identifier et résoudre les problèmes, améliorer nos produits et services, et proposer à nos utilisateurs des expériences personnalisées. Les données d’utilisation que nous collectons concernent également les applications exécutées sur les PC dans l’écosystème Windows. En fonction de l’utilisation de nos clients, nous créons une liste pour tester ces applications, appareils et pilotes dans les nouvelles versions du système d’exploitation Windows. À ce jour, Windows 10 est la version de Windows la plus compatible, à plus de 90 % de compatibilité avec des milliers d’applications courantes. L’équipe de compatibilité de Windows est à l’écoute de nos partenaires éditeurs de logiciels indépendants pour leur fournir un feedback si des problèmes sont détectés, afin que nous puissions travailler en partenariat sur des solutions. Dans l’idéal, nous voulons que nos clients communs soient en mesure de mettre à jour Windows de façon transparente et sans perte de fonctionnalités dans leur système d’exploitation, ou dans les applications dont ils dépendent pour leur productivité ou leurs loisirs.

Les sections suivantes contiennent certaines recommandations de Microsoft pour vous permettre de garantir la compatibilité de vos applications avec Windows 10.

### <a name="windows-version-check"></a>Vérification de la version de Windows

La version du système d’exploitation a été incrémentée pour passer à Windows 10. Cela signifie que le numéro de version interne a été changé en 10.0. Comme précédemment, nous faisons le maximum pour maintenir la compatibilité entre les applications et les appareils après un changement de version du système d’exploitation. Pour la plupart des catégories d’applications (sans dépendances vis-à-vis du noyau), cette modification n’a pas d’impact négatif sur les fonctionnalités des applications, et les applications existantes continuent de fonctionner parfaitement sur Windows 10.

La manifestation de cette modification est propre à l’application. En d’autres termes, une application qui vérifie spécifiquement la version du système d’exploitation obtient un numéro de version supérieur, ce qui peut conduire aux situations suivantes :
-   Les programmes d’installation des applications peuvent ne pas être en mesure d’installer les applications, ou les applications peuvent être dans l’impossibilité de démarrer.
-   Les applications peuvent devenir instables ou se planter.
-   Les applications peuvent générer des messages d’erreur, tout en continuant de fonctionner correctement.

Certaines applications effectuent une vérification de version et transmettent simplement un avertissement aux utilisateurs. Cependant, certaines d’entre elles sont liées très étroitement à une vérification de la version (dans les pilotes ou en mode noyau pour éviter la détection). Dans ce cas, les applications échouent si une version incorrecte est détectée. Au lieu d’une vérification de la version, nous vous recommandons une des approches suivantes :
-   Si l’application dépend de fonctionnalités d’API spécifiques, veillez à cibler la version d’API correcte.
-   Veillez à détecter la modification via APISet ou une autre API publique, et n’utilisez pas la version comme proxy pour certaines fonctionnalités ou certains correctifs. En cas de changements cassants, et si une vérification correcte n’est pas exposée, il s’agit d’un bogue.
-   Veillez à ce que l’application ne vérifie PAS la version de façon non standard, par exemple via le Registre, les versions des fichiers, des décalages, le mode noyau, les pilotes ou d’autres façons. Si l’application doit absolument vérifier la version, utilisez les API GetVersion, qui doivent retourner le numéro majeur, le numéro mineur et le numéro de build.
-   Si vous utilisez l’API [GetVersion](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversion), n’oubliez pas que le comportement de cette API a changé depuis Windows 8.1.

Si vous avez des applications comme des applications anti-programme malveillant ou de pare-feu, vous devez utiliser vos canaux de feedback habituels et le programme Windows Insider.

### <a name="undocumented-apis"></a>API non documentées

Vos applications ne doivent pas appeler des API Windows non documentées ni dépendre d’exportations de fichiers spécifiques ou de clés de Registre de Windows. Cela peut aboutir à des fonctionnalités qui ne marchent plus, à la perte de données et à des problèmes de sécurité potentiels. Si des fonctionnalités nécessaires à votre application ne sont pas disponibles, vous pouvez fournir un feedback via vos canaux de feedback habituels et via le programme Windows Insider.

### <a name="develop-universal-windows-platform-uwp-and-centennial-apps"></a>Développer des applications Centennial et de plateforme Windows universelle (UWP)

Nous encourageons tous les éditeurs d’applications Win32 indépendants à développer dorénavant des applications de [plateforme Windows universelle (UWP)](https://blogs.windows.com/windowsdeveloper/2016/02/25/an-update-on-the-developer-opportunity-and-windows-10/) et, plus spécifiquement, des applications [Centennial](https://channel9.msdn.com/Events/Build/2015/2-692). Les avantages sont nombreux à développer ces packages d’applications au lieu d’utiliser des programmes d’installation Win32 traditionnels. Comme les applications UWP sont également prises en charge dans le [Microsoft Store](https://blogs.windows.com/windowsdeveloper/2016/02/04/windows-store-trends-february-2016/), il vous est plus facile de mettre à jour automatiquement les applications de vos utilisateurs vers une version cohérente, ce qui réduit vos coûts de support technique.

Si vos types d’application Win32 ne fonctionnent pas avec le modèle Centennial, nous vous recommandons vivement d’utiliser le programme d’installation approprié et de vous assurer qu’il a été complètement testé. Un programme d’installation représente la première expérience de votre utilisateur ou de votre client avec votre application : vous devez donc garantir son bon fonctionnement. Il arrive trop souvent qu’il ne fonctionne pas correctement ou qu’il n’a pas été complètement testé pour tous les scénarios. Le [Kit de certification des applications Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) peut vous aider à tester l’installation et la désinstallation de votre application Win32. Il peut également vous permettre d’identifier l’utilisation d’API non documentées ainsi que d’autres problèmes de base liés aux performances, avant vos utilisateurs.

**Meilleures pratiques :**
-   Utilisez des programmes d’installation qui fonctionnent à la fois pour les versions 32 bits et 64 bits de Windows.
-   Concevez vos programmes d’installation pour qu’ils fonctionnent dans plusieurs scénarios (niveau de l’utilisateur ou de l’ordinateur).
-   Conservez tous les composants redistribuables de Windows dans le package d’origine. Si vous réorganisez le package de ces composants, il se peut que le programme d’installation ne fonctionne pas correctement.
-   Planifiez le temps de développement de vos programmes d’installation ; ils sont souvent négligés en tant que livrables au cours du cycle de développement logiciel.

## <a name="optimized-test-strategies-and-flighting"></a>Stratégies de test et distribution de version d’évaluation optimisées

La distribution de version d’évaluation du système d’exploitation Windows désigne les builds intermédiaires proposées aux Windows Insiders avant la publication d’une build finale. Plus les Insiders qui évaluent ces builds intermédiaires sont nombreux, plus les feedbacks que nous recevons sur la qualité des builds, leur compatibilité, etc. le sont aussi, ce qui contribue à l’amélioration de la qualité des builds finales. Vous pouvez prendre part à ce programme de distribution de version d’évaluation pour garantir que vos applications fonctionneront comme prévu dans les builds successives du système d’exploitation. Nous vous invitons également à fournir un feedback sur le fonctionnement de ces builds avec version d’évaluation, sur les problèmes que vous avez rencontrés, etc.

Si votre application figure dans le Store, vous pouvez distribuer une version d’évaluation de votre application par son intermédiaire, ce qui signifie que votre application pourra être installée par nos Windows Insiders. Les utilisateurs peuvent installer votre application et vous envoyer un feedback préliminaire sur celle-ci avant une publication générale. Les sections suivantes décrivent les étapes permettant de tester vos applications par rapport aux builds avec version d’évaluation de Windows.

### <a name="step-1-become-a-windows-insider-and-participate-in-flighting"></a>Étape 1 : Devenez un Windows Insider et prenez part à la distribution de version d’évaluation
Au titre de [Windows Insider](https://insider.windows.com/), vous pouvez nous aider à façonner l’avenir de Windows : votre feedback nous aidera à améliorer les fonctions et fonctionnalités de la plateforme. Il s’agit d’une communauté active dans laquelle vous pouvez communiquer avec d’autres passionnés, rejoindre des forums, échanger des conseils et découvrir les prochains événements destinés uniquement aux Insiders.

Comme vous avez accès aux versions d’évaluation de Windows 10, de Windows 10 Mobile, et à la dernière version du SDK Windows et de l’émulateur, vous disposez de tous les outils nécessaires pour développer des applications réussies et explorer les nouveautés de la plateforme Windows universelle et du Microsoft Store.

C’est là aussi une excellente occasion de créer du matériel remarquable, avec les versions d’évaluation des kits de développement de matériel, pour pouvoir ainsi développer des pilotes universels pour Windows. Windows IoT Standard Insider Preview est également disponible sur les plateformes de développement IoT prises en charge, pour vous permettre de créer des solutions connectées de premier plan en utilisant la plateforme Windows universelle.

Avant de devenir un Windows Insider, notez que ce programme s’adresse aux utilisateurs qui :
-   Veulent tester des logiciels qui sont encore en développement.
-   Veulent partager un feedback sur les logiciels et la plateforme.
-   N’ont aucun problème pour installer de nombreuses mises à jour ou une interface utilisateur susceptible de changer considérablement.
-   S’y connaissent vraiment en PC et ne voient aucun inconvénient à résoudre les problèmes, sauvegarder leurs données, formater un disque dur, installer un système d’exploitation depuis le début ou restaurer un ancien système si nécessaire.
-   Savent ce qu’est un fichier ISO et comment l’utiliser.
-   N’effectuent pas l’installation de ces logiciels sur leur ordinateur ou leur appareil qu’ils utilisent tous les jours.

### <a name="step-2-test-your-scenarios"></a>Étape 2 : Testez vos scénarios

Une fois que vous avez procédé à une mise à jour vers une build avec version d’évaluation, vous pouvez commencer à tester l’application et à collecter des feedbacks à l’aide des exemples de cas de test suivants. Pour la plupart de ces tests, veillez à utiliser des systèmes x86 et AMD64.
**Test de nouvelle installation :** Sur une nouvelle installation de Windows 10, vérifiez que votre application est totalement fonctionnelle. Si votre application échoue à ce test et au test de mise à niveau, il est probable que le problème soit imputable à des modifications du système d’exploitation sous-jacent ou à des bogues dans l’application. Si, après un examen approfondi, il en ressort que le problème est dû à des modifications du système d’exploitation sous-jacent, veillez à utiliser le programme Windows Insider pour fournir un feedback et collaborer sur des solutions.

**Test de mise à niveau :** Vérifiez que votre application fonctionne après la mise à niveau d’une version de niveau inférieur de Windows (Windows 7 ou Windows 8.1) vers Windows 10. Votre application ne doit pas provoquer de restaurations pendant la mise à niveau et doit continuer de fonctionner comme prévu après celle-ci : ce point est essentiel pour obtenir une mise à niveau sans problèmes.

**Test de réinstallation :** Vérifiez que les fonctionnalités de l’application peuvent être restaurées en la réinstallant une fois que vous avez mis à niveau le PC vers Windows 10 à partir d’un système d’exploitation de niveau inférieur. Si votre application a échoué au test de mise à niveau et que vous n’êtes pas en mesure d’identifier la cause de ces problèmes, il est possible qu’une réinstallation puisse restaurer les fonctionnalités perdues. Un test de réinstallation réussi indique que certaines parties de l’application peuvent ne pas avoir été migrées vers Windows 10.

**Test des fonctionnalités du système d’exploitation\\de l’appareil :** Vérifiez que votre application fonctionne comme prévu si elle s’appuie sur des fonctionnalités spécifiques du système d’exploitation. Les éléments courants à tester sont listés ci-dessous, souvent par rapport à une sélection de modèles de PC couramment utilisés afin d’assurer une large couverture :
-   Audio
-   Fonctionnalités des périphériques USB (clavier, souris, clé USB, disque dur externe, etc.)
-   Bluetooth
-   Graphismes\\Affichage (multimoniteur, projection, rotation d’écran, etc.)
-   Écran tactile (orientation, clavier visuel, stylet, mouvements, etc.)
-   Pavé tactile (boutons droit\\gauche, appui, défilement, etc.)
-   Stylet (appui simple\\double, appui prolongé, gomme, etc.)
-   Impression\\Numérisation
-   Capteurs (accéléromètre, fusion, etc.)
-   Appareil photo

### <a name="step-3-provide-feedback"></a>Étape 3 : Fournir un feedback

Faites-nous savoir comment votre application se comporte par rapport aux builds avec version d’évaluation. Quand vous découvrez des problèmes avec votre application au cours des tests, indiquez les bogues via le portail des partenaires si vous y avez accès ou en contactant votre représentant Microsoft. Nous vous encourageons à nous transmettre ces informations, qui nous permettent de créer une expérience de qualité pour nos utilisateurs.


## <a name="related-topics"></a>Rubriques connexes
[Options de maintenance de Windows 10 pour les mises à jour et les mises à niveau](/windows/manage/introduction-to-windows-10-servicing)
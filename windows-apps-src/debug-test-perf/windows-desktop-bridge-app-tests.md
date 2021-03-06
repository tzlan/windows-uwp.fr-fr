---
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: Tests d’application Pont du bureau Windows
description: Utilisez les tests intégrés du Pont du bureau pour vous assurer que votre application de bureau est optimisée pour sa conversion en application UWP.
ms.date: 12/18/2017
ms.topic: article
keywords: windows 10, uwp, certification des applications
ms.localizationpriority: medium
ms.openlocfilehash: bdc9c3ee51523120f1e50ba9d2a2aba2b828be48
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169873"
---
# <a name="windows-desktop-bridge-app-tests"></a>Tests d’application Pont du bureau Windows

Les [applications Pont du bureau](/windows/msix/desktop/desktop-to-uwp-root) sont des applications de bureau Windows converties en applications de plateforme Windows universelle (UWP) à l’aide du [Pont du bureau](https://developer.microsoft.com/windows/bridges/desktop). Après la conversion, les applications de bureau Windows sont empaquetées, soumises à maintenance et déployées sous la forme d’un package d’application UWP (fichier .appx ou .appxbundle) ciblant Windows 10 Desktop.

## <a name="required-versus-optional-tests"></a>Tests obligatoires et tests facultatifs
Les tests facultatifs des applications Pont du bureau Windows sont destinés uniquement à informer. Ils ne sont pas utilisés pour évaluer votre application lors de son intégration au Microsoft Store. Nous recommandons d’examiner les résultats de ces tests pour produire des applications de meilleure qualité. Les critères généraux de réussite/d’échec d’intégration au Windows Store sont déterminés par les tests obligatoires et non par ces tests facultatifs.

## <a name="current-optional-tests"></a>Tests facultatifs actuels

### <a name="1-digitally-signed-file-test"></a>1. Test des fichiers signés numériquement 
**Arrière-plan**  
Ce test vérifie que tous les fichiers exécutables portables (PE) contiennent une signature valide. La présence de fichiers signés numériquement permet aux utilisateurs de savoir que le logiciel est authentique.

**Détails du test**  
Le test analyse tous les fichiers exécutables portables contenus dans le package et recherche une signature dans leur en-tête. Il est recommandé de signer numériquement tous les fichiers PE. Un avertissement est généré si l’un des fichiers PE n’est pas signé.
 
**Actions correctives**  
Il est toujours recommandé de signer numériquement les fichiers. Pour plus d’informations, consultez [Introduction to Code Signing](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85)) (Présentation du code de signature).

### <a name="2-file-association-verbs"></a>2. Verbes d’association de fichiers 
**Arrière-plan**  
Ce test analyse le Registre de package pour vérifier si des verbes d’association de fichiers sont enregistrés. 

**Détails du test**  
Les applications de bureau converties peuvent être améliorées avec un large éventail d’API Windows Runtime. Ce test vérifie que les fichiers binaires UWP de l’application n’appellent pas des API autres que des API Windows Runtime. L’indicateur **AppContainer** est défini pour les fichiers binaires UWP.

**Actions correctives**  
Voir [Pont du bureau vers UWP : Extensions d’application](/windows/apps/desktop/modernize/desktop-to-uwp-extensions) pour obtenir une explication de ces extensions et apprendre à les utiliser correctement. 

### <a name="3-debug-configuration-test"></a>3. Test de configuration du débogage
Ce test vérifie que le package .msix ou .appx n’est pas une version de débogage.
 
**Arrière-plan**  
Pour pouvoir être certifiées pour le Microsoft Store, les applications ne doivent pas être compilées pour le débogage et ne doivent pas référencer les versions de débogage d’un fichier exécutable. En outre, vous devez générer votre code de manière optimisée pour que votre application réussisse ce test.
 
**Détails du test**  
Testez l’application de manière à vérifier qu’il ne s’agit pas d’une version de débogage et qu’elle n’est pas liée à des infrastructures de débogage.
 
**Actions correctives**  
* Générez l’application en tant que version de débogage avant de la soumettre au Microsoft Store.
* Vérifiez que la version correcte du .NET Framework est installée.
* Assurez-vous que l’application ne crée pas de liens vers des versions de débogage d’une infrastructure et qu’elle est créée avec une version commerciale. Si l’application contient des composants .NET, assurez-vous que vous avez installé la version correcte du .NET Framework.

### <a name="4-package-sanity-test"></a>4. Tests d’intégrité des packages
#### <a name="41-archive-files-usage"></a>4.1 Utilisation des fichiers archivés

**Arrière-plan**  
Ce test vous permet de créer des applications Pont du bureau plus performantes à exécuter sur des ordinateurs [Windows 10 S](https://www.microsoft.com/windows/windows-10-s).

**Détails du test**  
Ce test vérifie tous les fichiers exécutables inclus dans des fichiers archivés ou du contenu à extraction automatique. Comme les fichiers exécutables inclus dans ce type de contenu ne sont pas signés lors de l’intégration au Windows Store, l’application risque de ne pas fonctionner comme prévu sur les systèmes Windows 10 S.
 
**Actions correctives**
* Examinez les fichiers signalés par le test afin de déterminer leur impact éventuel sur votre application qui s'exécute dans un environnement Windows 10 S.
* Si votre application est affectée, supprimez les fichiers exécutables à partir des fichiers archivés et n'utilisez pas d'archives à extraction automatique pour placer des fichiers exécutables sur le disque. Cela devrait empêcher la perte de fonctionnalités de l’application.

#### <a name="42-blocked-executables"></a>4.2 Fichiers exécutables bloqués

**Arrière-plan**  
Ce test vous permet de créer des applications Pont du bureau plus performantes à exécuter sur des ordinateurs [Windows 10 S](https://www.microsoft.com/windows/windows-10-s). 

**Détails du test**  
Ce test vérifie si l’application tente de lancer des fichiers exécutables, ce qui est limité sur les systèmes Windows 10 S. Les applications qui reposent sur cette fonctionnalité risquent de ne pas fonctionner comme prévu sur les systèmes Windows 10 S. 

**Actions correctives**  
* Identifiez quelles entrées signalées par le test représentent un appel pour lancer un fichier exécutable qui ne fait pas partie de votre application, et supprimez ces appels. 
* Si le ou les fichiers signalés font partie de votre application, vous pouvez ignorer l’avertissement.


## <a name="current-required-tests"></a>Tests obligatoires actuels

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. Test des fonctionnalités d’application (fonctionnalités spéciales)

**Arrière-plan**  
Les fonctionnalités à usage spécial sont destinées à des scénarios très spécifiques. Seuls les comptes d’entreprise sont autorisés à utiliser ces fonctionnalités. 

**Détails du test**  
Le test est validé si l’application déclare une ou plusieurs des fonctionnalités suivantes : 
* EnterpriseAuthentication
* SharedUserCertificates
* DocumentsLibrary

Si au moins une de ces fonctionnalités est déclarée, le test affiche un message d’avertissement pour l’utilisateur. 

**Actions correctives**  
Envisagez de supprimer la fonctionnalité à usage spécial si votre application n’en a pas besoin. Par ailleurs, l’utilisation de ces fonctionnalités est sujette à un examen supplémentaire de la stratégie d’intégration.

### <a name="2-app-manifest-resources-tests"></a>2. Tests des ressources du manifeste d’application 
#### <a name="21-app-resources-validation"></a>2.1 Validation des ressources de l’application
Votre application risque de ne pas s’installer correctement si les chaînes ou les images déclarées dans son manifeste sont incorrectes. Si l’application s’installe avec ces erreurs, son logo ou d’autres images associées risquent de ne pas s’afficher correctement.    

**Détails du test**  
Inspecte les ressources définies dans le manifeste de l’application afin de vérifier qu’elles sont présentes et valides.

**Action corrective**  
Inspirez-vous du tableau suivant.

Message d’erreur | Commentaires
--------------|---------
L’image {image name} définit à la fois les qualificateurs Scale et TargetSize ; vous ne pouvez définir qu’un seul qualificateur à la fois. | Vous pouvez personnaliser les images pour différentes résolutions. Dans le message réel, {image name} représente le nom de l’image affectée par l’erreur. Assurez-vous que chaque image définit Scale ou TargetSize comme qualificateur. 
L’image {image name} ne respecte pas les restrictions imposées pour la taille.  | Assurez-vous que toutes les images de l’application adhèrent aux restrictions définissant la taille appropriée. Dans le message réel, {image name} représente le nom de l’image affectée par l’erreur. 
L’image {image name} ne se trouve pas dans le package.  | Une image requise est manquante. Dans le message réel, {image name} représente le nom de l’image manquante. 
L’image {image name} n’est pas un fichier image valide.  | Assurez-vous que toutes les images de l’application adhèrent aux restrictions définissant le type de format de fichier approprié. Dans le message réel, {image name} représente le nom de l’image non valide. 
L’image « BadgeLogo » a une valeur ABGR {value} à la position (x, y) qui n’est pas valide. Le pixel doit être blanc (##FFFFFF) ou transparent (00######).  | Le logo du badge représente une image qui apparaît à côté de la notification de badge afin d’identifier l’application sur l’écran de verrouillage. L’image doit être monochrome (elle ne peut contenir que des pixels blancs ou transparents). Dans le message réel, {value} représente la valeur de couleur qui n’est pas valide dans l’image. 
L’image « BadgeLogo » a une valeur ABGR « {value} » non valide pour une image blanche à contraste élevé à la position (x, y). Les pixels doivent être (##2A2A2A) ou plus sombres, ou transparents (00######).  | Le logo du badge représente une image qui apparaît à côté de la notification de badge afin d’identifier l’application sur l’écran de verrouillage. Étant donné que le logo du badge apparaît sur un arrière-plan blanc lors de l’utilisation d’un motif blanc à contraste élevé, il doit être une version sombre du logo de badge normal. Lors de l’utilisation d’un motif blanc à contraste élevé, le logo du badge ne peut contenir que des pixels plus sombres que (##2A2A2A) ou transparents. Dans le message réel, {value} représente la valeur de couleur qui n’est pas valide dans l’image. 
L’image doit définir au moins un type Variant sans qualificateur TargetSize. Elle doit définir un qualificateur Scale ou laisser Scale et TargetSize non spécifiés, ce qui donne la valeur par défaut Scale-100.  | Pour plus d’informations, consultez les guides sur la [conception réactive](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) et [les ressources d’application](../design/app-settings/store-and-retrieve-app-data.md). 
Un fichier « resources.pri » manque dans le package.  | Si le manifeste de votre application comporte du contenu localisable, veillez à ce que le package de votre application contienne un fichier resources.pri valide. 
Le fichier « resources.pri » doit contenir un mappage des ressources avec un nom qui correspond au nom du package « {package full name} ».  | Vous pouvez obtenir cette erreur si le manifeste a changé et que le nom du mappage de ressources dans resources.pri ne correspond plus au nom du package dans le manifeste. Dans le message réel, {package full name} représente le nom du package que resources.pri doit contenir. Pour résoudre ce problème, vous devez régénérer resources.pri ; la façon la plus facile de le faire consiste à régénérer le package de l’application. 
La fusion automatique ne doit pas être activée pour le fichier « resources.pri ».  | MakePRI.exe prend en charge une option appelée AutoMerge. La valeur par défaut d’AutoMerge est Désactivée. Lorsque l’option AutoMerge est activée, elle fusionne les ressources du module linguistique d’une application en un fichier resources.pri unique au moment de l’exécution. Ce paramétrage est déconseillé pour les applications que vous envisagez de distribuer par le biais du Microsoft Store. Le fichier resources.pri d’une application distribuée par le biais du Microsoft Store doit se trouver à la racine du package de l’application et contenir toutes les références linguistiques prises en charge par l’application. 
La chaîne « {string} » ne respecte pas la limite maximale de {number} caractères.  | Consultez les [Exigences relatives aux packages d’applications](../publish/app-package-requirements.md). Dans le message réel, {string} est remplacé par la chaîne affectée par l’erreur et {number} représente la longueur maximale. 
La chaîne {string} ne doit pas comporter d’espace de début/fin.  | Le schéma des éléments du manifeste de l’application n’autorise pas les espaces de début ou de fin. Dans le message réel, {string} est remplacé par la chaîne affectée par l’erreur. Assurez-vous qu’aucune des valeurs localisées des champs du manifeste dans resources.pri ne possède d’espaces de début ou de fin. 
La chaîne ne doit pas être vide (sa longueur doit être supérieure à zéro).  | Pour plus d’informations, voir [Exigences relatives aux packages d’applications](../publish/app-package-requirements.md). 
Il n’y a aucune ressource par défaut spécifiée dans le fichier « resources.pri ».  | Pour plus d’informations, voir le guide sur les [ressources d’application](../design/app-settings/store-and-retrieve-app-data.md). Dans la configuration de build par défaut, Visual Studio inclut uniquement les ressources d’image avec qualificateur « Scale-200 » dans le package d’application lors de la génération des offres groupées, et place les autres ressources dans le package de ressources. Prenez soin d’inclure les ressources d’image avec qualificateur « Scale-200 » ou de configurer votre projet pour qu’il intègre les ressources dont vous disposez. 
Aucune valeur de ressource n’est spécifiée dans le fichier « resources.pri ».  | Assurez-vous que des ressources valides sont définies dans resources.pri pour le manifeste de l’application. 
La taille du fichier image {filename} doit être inférieure à 204 800 octets.  | Réduisez la taille des images indiquées. 
Le fichier {filename} ne doit pas contenir de section de correspondance inverse.  | Bien que la correspondance inverse soit générée pendant un débogage F5 Visual Studio lors d’un appel de makepri.exe, elle peut être supprimée en exécutant makepri.exe sans le paramètre /m lors de la génération d’un fichier .pri. 



#### <a name="22-branding-validation"></a>2.2 Validation de la personnalisation
**Arrière-plan**  
Les applications Pont du bureau doivent être complètes et pleinement fonctionnelles. Les applications qui utilisent les images par défaut (provenant des exemples ou exemples SDK) offrent une expérience utilisateur médiocre et sont difficilement identifiables dans le catalogue du Windows Store.

**Détails du test**  
Le test réussit si les images utilisées par l’application ne sont pas des images par défaut provenant des exemples du Kit de développement logiciel (SDK) ou de Visual Studio. 

**Actions correctives**  
Remplacez les images par défaut par quelque chose de plus singulier et de plus représentatif de votre application.

### <a name="3-package-compliance-tests"></a>3. Tests de conformité du package
#### <a name="31-app-manifest"></a>3.1 Manifeste de l’application
Teste le contenu du manifeste de l’application pour vérifier qu’il est correct.

**Arrière-plan**  
Les applications doivent avoir un manifeste d’application correctement mis en forme.

**Détails du test**  
Examine le manifeste de l’application afin de vérifier que son contenu est correct, comme décrit dans [Exigences relatives aux packages d’applications](../publish/app-package-requirements.md). Les vérifications suivantes sont effectuées au cours de ce test :
* **Extensions de fichiers et protocoles**  
Votre application peut déclarer les types de fichier auxquels elle peut être associée. L’expérience utilisateur est plus médiocre si une déclaration contient un grand nombre de types de fichier inhabituels. Ce test limite le nombre d’extensions de fichier auxquelles une application peut être associée.
* **Règle de dépendance de framework**  
Ce test applique la spécification selon laquelle les applications déclarent des dépendances appropriées sur la plateforme Windows universelle (UWP). En cas de dépendance inappropriée, ce test échoue. En cas d’incompatibilité entre la version du système d’exploitation ciblée par l’application et les dépendances de framework établies, le test échoue. Le test échoue également si l’application fait référence à des versions « d’évaluation » des DLL de framework.
* **Vérification de la communication entre processus (IPC)**  
Ce test applique la spécification selon laquelle les applications Pont du bureau ne communiquent pas en dehors du conteneur d’application avec des composants de bureau. La communication entre processus ne concerne que les applications chargées latéralement. Les applications qui spécifient l’attribut [**ActivatableClassAttribute**](/uwp/schemas/appxpackage/appxmanifestschema/element-activatableclassattribute) avec `DesktopApplicationPath` comme nom échouent à ce test.  

**Action corrective**  
Confrontez le manifeste de l’application aux exigences décrites dans [Exigences relatives aux packages d’applications](../publish/app-package-requirements.md).


#### <a name="32-application-count"></a>3.2 Nombre d'applications
Ce test vérifie qu’un package d’application (.appx, ensemble d’applications) contient une application. 

**Arrière-plan**  
Ce test est implémenté conformément à la stratégie du Store. 

**Détails du test**  
Ce test vérifie que le nombre total de packages .appx contenus dans l’ensemble d’applications est inférieur à 512, et qu’il n’existe qu’un seul package « principal » dans cet ensemble d’applications. Il vérifie également que le numéro de révision de la version de l’ensemble d’applications est défini sur 0. 

**Actions correctives**  
Assurez-vous que le package et l’ensemble d’applications satisfont aux exigences décrites dans **Détails du test**.


#### <a name="33-registry-checks"></a>3.3 Vérifications du Registre
**Arrière-plan**  
Ce test vérifie si l’application installe ou met à jour de nouveaux pilotes ou services.

**Détails du test**  
Le test recherche dans le fichier registry.dat des mises à jour pour des emplacements de Registre spécifiques qui indiquent un nouveau service ou un nouveau pilote enregistré. Si l’application tente d’installer un pilote ou un service, le test échoue.  

**Actions correctives**  
Passez en revue les échecs et supprimez les services ou pilotes en question s’ils sont inutiles. Si l’application dépend de ces derniers, vous devez la réviser si vous souhaitez l’intégrer au Store.


### <a name="4-platform-appropriate-files-test"></a>4. Test des fichiers appropriés à la plateforme
Les applications qui installent des fichiers binaires mixtes peuvent se bloquer ou ne pas s’exécuter correctement sur l’architecture du processeur de l’utilisateur. 

**Arrière-plan**  
Ce test analyse les fichiers binaires stockés dans un package d’application pour y rechercher d’éventuels conflits d’architecture. Un package d’application ne doit pas inclure des fichiers binaires qui ne peuvent pas être utilisés sur l’architecture de processeur spécifiée dans le manifeste. Inclure des fichiers binaires non pris en charge peut entraîner le blocage de votre application ou une augmentation inutile de la taille de son package. 

**Détails du test**  
Le test vérifie que le nombre de bits figurant dans l’en-tête exécutable portable de chaque fichier est approprié en cas de référence croisée avec la déclaration de l’architecture de processeur du package d’application. 

**Actions correctives**  
Suivez les recommandations suivantes pour vous assurer que votre package d’application contient uniquement des fichiers pris en charge par l’architecture spécifiée dans le manifeste d’application : 
* Si l’architecture du processeur cible de votre application a un type de processeur Neutre, le package d’application ne peut pas contenir des fichiers binaires x86, x64 ou ARM, ni de fichiers de types d’images.
* Si l’architecture du processeur cible de votre application a un type de processeur x86, le package d’application doit uniquement contenir des fichiers binaires x86 ou des fichiers de types d’images. Si le package contient des fichiers binaires x64 ou ARM, ou des fichiers de types d’images, il échouera au test.
* Si l’architecture du processeur cible de votre application a un type de processeur x64, le package d’application doit contenir des fichiers binaires x64 ou des fichiers de types d’images. Notez que, dans ce cas, le package peut également inclure des fichiers x86, mais l’expérience d’application principale doit utiliser le fichier binaire x64. Si le package contient des fichiers binaires ARM ou des fichiers de type image, ou s’il contient *uniquement* des fichiers binaires x86 ou des fichiers de type image, il échoue au test.
* Si l’architecture du processeur cible de votre application a un type de processeur ARM, le package d’application doit uniquement contenir des fichiers binaires ARM ou des fichiers de types d’images. Si le package contient des fichiers binaires x64 ou x86, ou des fichiers de types d’images, il échouera au test. 

### <a name="5-supported-api-test"></a>5. Test des API prises en charge
Ce test vérifie si l’application utilise des API non conformes. 

**Arrière-plan**  
Les applications Pont du bureau peuvent tirer parti de certaines API Win32 héritées et d’API modernes (composants UWP). Ce test identifie les fichiers binaires managés qui utilisent des API non prises en charge.
 
**Détails du test**  
Ce test vérifie tous les composants UWP de l’application :
* S’assure que chaque fichier binaire managé dans le package d’application n’est pas dépendant d’une API Win32 non prise en charge pour le développement d’applications UPW en vérifiant la table des adresses d’importation du fichier binaire.
* Vérifie que chaque fichier binaire managé dans le package d’application n’est pas dépendant d’une fonction en dehors du profil approuvé. 

**Actions correctives**  
Ce peut être corrigé en vous assurant que l’application a été compilée comme une version commerciale et non comme une version de débogage. 

> [!NOTE]
> La version de débogage d’une application échoue à ce test même si l’application utilise uniquement des [API pour applications UPW](/uwp/). Passez en revue les messages d’erreur pour identifier l’API présente qui n’est pas autorisée pour les applications UWP. 

> [!NOTE]
> Les applications C++ générées dans une configuration de débogage échouent à ce test même si la configuration utilise uniquement des API du SDK Windows pour les applications UPW. Pour plus d’informations, consultez [Alternatives aux API Windows dans les applications UWP](/uwp/win32-and-com/win32-and-com-for-uwp-apps).

### <a name="6-user-account-control-uac-test"></a>6. Test du contrôle de compte d’utilisateur (UAC)  

**Arrière-plan**  
Le test s’assure que l’application ne demande pas de contrôle de compte d’utilisateur lors de l’exécution.

**Détails du test**  
Une application ne peut pas demander une élévation des droits d’administrateur ni UIAccess conformément à la politique du Microsoft Store. Les autorisations de sécurité élevées ne sont pas prises en charge. 

**Actions correctives**  
Les applications doivent s’exécuter en tant qu’utilisateur interactif. Pour plus d’informations, voir [Vue d’ensemble de la sécurité UI Automation](/dotnet/framework/ui-automation/ui-automation-security-overview).

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Validation des métadonnées Windows Runtime
**Arrière-plan**  
S’assure que les composants fournis avec une application sont conformes au système de type UWP.

**Détails du test**  
Ce test lève un certain nombre d’indicateurs liés à une utilisation du type approprié.

**Actions correctives**  
* **Attribut ExclusiveTo**  
Vérifiez que les classes UWP n’implémentent pas d’interfaces marquées comme étant des interfaces exclusives d’une autre classe.
* **Exactitude des métadonnées générales**  
Vérifiez que le compilateur que vous utilisez pour générer vos types est conforme aux dernières spécifications UWP.
* **Propriétés**  
Assurez-vous que toutes les propriétés d’une classe UWP disposent d’une méthode `get` (les méthodes `set` sont facultatives). Pour toutes les propriétés, vérifiez que le type retourné par la méthode `get` correspond au type du paramètre d’entrée de la méthode `set`.
* **Emplacement du type**  
Vérifiez que les métadonnées de tous les types UWP se trouvent dans le fichier .winmd dont le nom correspondant à l’espace de noms est le plus long du package d’application.
* **Respect de la casse du nom du type**  
Vérifiez que tous les types UWP de votre package d’application ont un nom unique qui ne respecte pas la casse. S’assure également qu’aucun nom de type UWP n’est utilisé comme nom d’espace de noms dans votre package d’application.
* **Exactitude du nom du type**  
Vérifiez qu’aucun type UWP ne se trouve dans l’espace de noms global ni dans l’espace de noms Windows de niveau supérieur.
 

### <a name="8-windows-security-features-tests"></a>8. Test des fonctionnalités de sécurité Windows
La modification des protections de sécurité Windows par défaut peut exposer les clients à des risques accrus. 

#### <a name="81-banned-file-analyzer"></a>8.1 Analyseur de fichiers non autorisés
**Arrière-plan**  
Certains fichiers ont été mis à jour avec l’apport d’améliorations importantes en matière de sécurité et de fiabilité, entre autres. Les applications Pont du bureau Windows doivent contenir les toutes dernières versions de ces fichiers, car les versions obsolètes présentent un risque. Le Kit de certification des applications Windows bloque ces fichiers afin de garantir que toutes les applications utilisent la version actuelle.

**Détails du test**  
L’analyseur de fichiers non autorisés du Kit de certification des applications Windows vérifie actuellement les fichiers suivants :
* *Bing.Maps.JavaScript\js\veapicore.js*  
Cette vérification échoue généralement si une application utilise une version « Release Preview » du fichier et non la dernière version officielle. 

**Actions correctives**  
Pour résoudre ce problème, utilisez la dernière version du [Kit de développement logiciel (SDK) Bing Maps](https://www.bingmapsportal.com/) pour les applications UWP.

#### <a name="82-private-code-signing"></a>8.2 Code de signature privé
Teste l’existence de fichiers binaires de signature de code privé dans le package de l’application. 

**Arrière-plan**  
Les fichiers de signature de code privé doivent demeurer privés car ils peuvent être utilisés à des fins malveillantes s’ils sont compromis. 

**Détails du test**  
Recherche dans le package de l’application les fichiers portant l’extension .pfx ou .snk qui indiquent la présence de clés de signature privées. 

**Actions correctives**  
Supprimez du package toute clé de code de signature privé (par exemple, les fichiers .pfx et .snk).


## <a name="related-topics"></a>Rubriques connexes

* [Politiques du Microsoft Store](/legal/windows/agreements/store-policies)
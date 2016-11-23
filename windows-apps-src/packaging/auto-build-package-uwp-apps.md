---
author: rmpablos
title: "Configuration de builds automatisées pour votre application UWP"
description: "Configuration de builds automatisées pour produire des packages de chargement indépendant et/ou de stockage."
translationtype: Human Translation
ms.sourcegitcommit: 1e30c20b38b9e6b00cd4f56bf9ffb3752ceed6a9
ms.openlocfilehash: 681019d708b859d117992cde6d2ad3d37fa4833a

---
# Configuration de builds automatisées pour votre application UWP

Vous pouvez utiliser Visual Studio Team Services (VSTS) pour créer des builds automatisées pour les projetsUWP. Cet article vous présente différentes méthodes d’utilisation.  Il vous explique également comment effectuer ces tâches à l’aide de la ligne de commande afin de procéder à l’intégration avec d’autres systèmes de génération tels que AppVeyor. 

## Sélection du type approprié d’agent de build

Choisissez le type d’agent de build que vous souhaitez que VSTS utilise lors de l’exécution du processus de génération. Un agent de build hébergé est déployé avec les outils et Kits de développement logiciel (SDK) les plus courants. Il fonctionne dans la plupart des cas. Consultez l’article [Logiciels sur le serveur de build hébergé](https://www.visualstudio.com/en-us/docs/build/admin/agents/hosted-pool#software). Toutefois, si vous souhaitez contrôler davantage les étapes de génération, vous pouvez créer un agent de build personnalisé. Vous pouvez utiliser le tableau suivant pour vous aider à prendre une décision.

|**Scénario**|**Agent personnalisé**|**Agent de build hébergé**|
-------------|----------------|----------------------|
|Build UWP de base (y compris native .NET)|:white_check_mark:|:white_check_mark:|
|Génération de packages pour le chargement indépendant|:white_check_mark:|:white_check_mark:|
|Génération de packages pour la soumission au Windows Store|:white_check_mark:|:white_check_mark:|
|Utilisation de certificats personnalisés|:white_check_mark:||
|Build ciblant un Kit de développement logiciel (SDK) Windows personnalisé|:white_check_mark:||
|Exécution de tests unitaires|:white_check_mark:||
|Utilisation de builds incrémentielles|:white_check_mark:||

>Remarque: si vous prévoyez de cibler le Kit de développement logiciel (SDK) de la mise à jour anniversaire Windows (build14393), vous devez configurer votre agent de build personnalisé, dans la mesure où le pool de build hébergé prend seulement en charge les SDK10586 et 10240. Plus d’informations sur le [choix d’une versionUWP](https://msdn.microsoft.com/en-us/windows/uwp/updates-and-versions/choose-a-uwp-version)

#### Création d’un agent de build personnalisé (facultatif)

Si vous choisissez de créer une agent de build personnalisé, vous devez utiliser les outils de plateforme Windows universelle. Ces outils font partie de Visual Studio. Vous pouvez utiliser Visual Studio Community Edition.

Pour en savoir plus, consultez [Déployer un agent sur Windows.](https://www.visualstudio.com/en-us/docs/build/admin/agents/v2-windows) 

Pour exécuter des tests unitaires UWP, vous devez effectuer les opérations suivantes: • Déployer et démarrer votre application. • Exécuter l’agent VSTS en mode interactif. • Configurer votre agent pour qu’ils e connecte automatiquement après un redémarrage.

À présent, voyons comment configurer une build automatisée.

## Configuration d’une build automatisée
Nous allons commencer par la définition de la build UWP par défaut disponible dans VSTS, puis vous expliquer comment configurer cette définition afin d’effectuer des tâches de génération plus avancées.

**Ajout du certificat de votre projet à un référentiel de code source**

VSTS fonctionne avec les référentiels de code basés sur TFS et sur GIT.  
Si vous utilisez un référentiel Git, ajoutez le fichier de certificat de votre projet au référentiel pour que l’agent de build puisse signer le package appx. Si vous ne procédez pas ainsi, le référentiel Git ignore le fichier de certificat. Pour ajouter le fichier de certificat à votre référentiel, cliquez avec le bouton droit sur le fichier de certificat dans l’Explorateur de solutions puis, dans le menu contextuel, choisissez la commande Ajouter le fichier ignoré au contrôle de code source. 

![inclusion d’un certificat](images/building-screen1.png)

Nous évoquons la [gestion avancée des certificats](#certificates-best-practices) plus loin dans ce guide. 

Pour créer votre première définition de build dans VSTS, accédez à l’onglet Builds, puis sélectionnez le bouton+.

![création d’une définition de build](images/building-screen2.png)

Dans la liste des modèles de définition de build, choisissez le modèle *Plateforme Windows universelle*.

![sélection d’une build UWP](images/building-screen3.png)

Cette définition de build contient les tâches de génération suivantes:

- Restauration NuGet **\*.sln
- Générer la solution **\*.sln
- Publier les symboles
- Publier l’artefact: déposer

#### Configuration de la tâche Restauration NuGet

Cette tâche restaure les packages NuGet définis dans votre projet. Certains packages exigent une version personnalisée de NuGet.exe. Si vous utilisez un package pour lequel c’est le cas, référencez cette version de NuGet.exe dans votre référentiel, puis référencez-la dans la propriété avancée *Chemin d’accès à NuGet.exe*.

![Définition de build par défaut](images/building-screen4.png)

#### Configuration de la tâche Générer la solution

Cette tâche compile toutes les solutions solution se trouvant dans le dossier de travail en fichiers binaires et produit un fichier de sortie AppX. Cette tâche utilise des arguments MSbuild.  Vous devez spécifier la valeur de ces arguments. Inspirez-vous du tableau suivant. 

|**Argument MSBuild**|**Valeur**|**Description**|
|--------------------|---------|---------------|
|AppxPackageDir|$(Build.ArtifactStagingDirectory)\AppxPackages|Définit le dossier de stockage des artefacts générés.|
|AppxBundlePlatforms|$(Build.BuildPlatform)|Vous permet de définir les plateformes à inclure dans l’offre groupée.|
|AppxBundle|Always|Crée un fichier appxbundle avec les fichiers appx pour la plateforme spécifiée.|
|**UapAppxPackageBuildMode**|StoreUpload|Définit le type de package appx à générer. (Non inclus par défaut)|


Si vous souhaitez générer votre solution à l’aide de la ligne de commande ou à l’aide d’un autre système de génération, exécutez msbuild avec ces arguments.

```
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"  
/p:UapAppxPackageBuildMode=StoreUpload 
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Les paramètres définis avec la syntaxe $() sont des variables définies dans la définition de build. Ils changent dans d’autres systèmes de génération.

![variables par défaut](images/building-screen5.png)

Pour afficher toutes les variables prédéfinies, consultez [Utiliser les variables de build](https://www.visualstudio.com/docs/build/define/variables).

#### Configurer la tâche Publier l’artefact 
Cette tâche stocke les artefacts générés dans VSTS. Vous pouvez les voir dans l’onglet Artefacts de la page de résultats de la génération. VSTS utilise le dossier `$Build.ArtifactStagingDirectory)\AppxPackages` que nous avons défini précédemment.

![artefacts](images/building-screen6.png)

Étant donné que nous avons défini la propriété `UapAppxPackageBuildMode` sur `StoreUpload`, le dossier d’artefacts inclut le package que vous chargez dans le Windows Store (appxupload), ainsi que les packages qui permettent le chargement indépendant (appxbundle).


>Remarque: par défaut, l’agent VSTS conserve les derniers packages appx générés. Si vous voulez stocker uniquement les artefacts de la version actuelle, configurez la build pour nettoyer le répertoire des fichiers binaires. Pour ce faire, ajoutez une variable nommée `Build.Clean` et définissez-la sur la valeur `all`. Pour plus d’informations, consultez [Spécifier le référentiel.](https://www.visualstudio.com/en-us/docs/build/define/repository#how-can-i-clean-the-repository-in-a-different-way)

#### Types de builds automatisées
Ensuite, vous devez générer la définition de build pour créer une build automatisée. Le tableau suivant décrit chaque type de build automatisée que vous pouvez créer. 

|**Type de build**|**Artefact**|**Fréquence recommandée**|**Description**|
|-----------------|------------|-------------------------|---------------|
|Intégration continue|Journal de génération, Résultats des tests|Chaque validation|Ce type de build est rapide et s’exécute plusieurs fois par jour.|
|Build de déploiement continu pour le chargement indépendant|Packages de déploiement|Quotidienne |Ce type de build peut inclure des tests unitaires, mais cela prend un peu plus de temps. Cela permet d’effectuer des tests manuels et vous pouvez l’intégrer avec d’autres outils comme HockeyApp.|
|Build de déploiement continu qui soumet un package au Windows Store|Packages de publication|À la demande|Ce type de build crée un package que vous pouvez publier dans le Windows store.|

Voyons comment configurer chacun d’eux.


## Configuration d’une build d’intégration continue (CI) 
Ce type de build vous aide à diagnostiquer rapidement les problèmes liés au code. Elles sont généralement exécutées pour une seule plateforme, et elles n’ont pas besoin d’être traitées par la chaîne d’outils native .NET. En outre, avec les builds CI, vous pouvez exécuter des tests unitaires qui produisent un rapport des résultats des tests.  

Si vous voulez exécuter des tests unitaires UWP dans le cadre de votre build CI, vous devez utiliser un agent de build personnalisé à la place de l’agent de build hébergé.

>Remarque: si vous regroupez plusieurs applications dans une même solution, il se peut que vous receviez une erreur. Consultez la rubrique suivante pour vous aider à résoudre l’erreur: [Traiter les erreurs qui s’affichent lorsque vous regroupez plusieurs applications dans une même solution.](#bundle-errors) 


### Configuration d’une définition de build CI
Utilisez le modèle UWP par défaut pour créer une définition de build. Ensuite, configurez le déclencheur pour qu’il s’exécute sur chaque enregistrement.  

![Déclencheur CI](images/building-screen7.png)

Dans la mesure où la build CI n’est pas déployée pour les utilisateurs, il est judicieux de conserver les différents numéros de contrôle de version afin d’éviter toute confusion avec les builds CD. Par exemple: `$(BuildDefinitionName)_0.0.$(DayOfYear)$(Rev:.r)`.


#### Configurer un agent de build personnalisé pour les tests unitaires

1.D’abord, activez le Mode développeur sur votre ordinateur. Consultez Activer votre appareil pour le développement. 2.Activez le service pour qu’il s’exécute comme un processus interactif. Consultez Déployer un agent sur Windows. 3.Déployez le certificat de signature vers l’agent.

Pour ce faire, double-cliquez sur le fichier .cer, choisissez Ordinateur local, puis choisissez Magasin de personnes autorisées.

<span id="uwp-unit-tests" />
### Configuration de la définition de build pour exécuter des tests unitaires UWP
Pour exécuter un test unitaire, utilisez l’étape de génération Test de Visual Studio.


![ajout de tests unitaires](images/building-screen8.png)

Les tests unitaires UWP sont exécutés dans le contexte d’un fichier appx donné pour que vous ne puissiez pas utiliser l’offre groupée générée. Vous devez également spécifier le chemin d’accès au fichier appx de plateforme concrète. Exemple:

```
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp.UnitTest\x86\MyUWPApp.UnitTest_$(AppxVersion)_x86.appx
```

>Remarque: utilisez la commande suivante pour exécuter les tests unitaires localement à partir de la ligne de commande:
`"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe"`

#### Accès aux résultats des tests
Dans VSTS, la page Résumé de la build présente les résultats des tests pour chaque build qui exécute des tests unitaires.  À partir de là, vous pouvez ouvrir la page Résultats des tests qui affiche des informations supplémentaires sur les résultats des tests. 

![résultats des tests](images/building-screen9.png)

#### Augmentation de la rapidité d’une build CI
Si vous voulez utiliser votre build CI seulement pour surveiller la qualité de vos enregistrements, vous pouvez réduire les durées de génération.

#### Pour augmenter la rapidité d’une build CI
1.  Générez pour une seule plateforme.
2.  Modifiez la variable BuildPlatform pour utiliser seulement x86. ![configuration CI](images/building-screen10.png) 
3.  À l’étape de génération, ajoutez /p:AppxBundle = jamais à la propriété Arguments MSBuild, puis définissez la propriété Plateforme. ![configuration de la plateforme](images/building-screen11.png)
4.  Dans le projet de test unitaire, désactivez Native .NET. 

Pour ce faire, ouvrez le fichier de projet et dans les propriétés du projet, définissez la propriété `UseDotNetNativeToolchain` sur `false`.

>Remarque: l’utilisation de la chaîne d’outil native .NET reste une partie importante du flux. Vous devez donc toujours l’utiliser pour tester les versions Release. 

<span id="bundle-errors" />
#### Traiter les erreurs qui s’affichent lorsque vous regroupez plusieurs applications dans une même solution 
Si vous ajoutez plusieurs projets UWP à votre solution puis tentez de créer une offre groupée, il se peut que vous receviez une erreur comme celle-ci: 

```
MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle
```

Cette erreur s’affiche car l’application qui doit apparaître dans l’offre groupée n’est pas clairement définie au niveau de la solution. Pour résoudre ce problème, ouvrez chaque fichier de projet et ajoutez les propriétés suivantes à la fin du premier élément `<PropertyGroup>`:

|**Projet**|**Propriétés**|
|-------|----------|
|App|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

Ensuite, supprimez l’argument msbuild `AppxBundle` de l’étape de génération.

## Configuration d’une build de déploiement continu pour le chargement indépendant
Une fois ce type de build terminée, les utilisateurs peuvent télécharger le fichier appxbundle à partir de la section artefacts de la page des résultats de build. Si vous voulez effectuer un test bêta de l’application en créant une distribution plus complète, vous pouvez utiliser le service HockeyApp. Ce service offre des fonctionnalités améliorées de test bêta, d’analyse des utilisateurs et de diagnostic d’incidents.


### Application des numéros de version à vos builds

Le fichier manifeste contient le numéro de version de l’application.  Mettez à jour le fichier manifeste dans votre référentiel de contrôle de code source pour changer le numéro de version. Pour mettre à jour le numéro de version de votre application, vous pouvez également utiliser le numéro de build généré par VSTS, puis modifier le manifeste de l’application juste avant de la compiler. Ne validez simplement pas ces modifications dans le référentiel de code source.

Vous devez définir votre format de numéro de build de contrôle de version dans la définition de build, puis utiliser le numéro de version obtenu pour mettre à jour le fichier AppxManifest et éventuellement les fichiers AssemblyInfo.cs avant de compiler.

Définissez le format de numéro de build dans l’onglet *Général* de la définition de build.:

![version de la build](images/building-screen12.png) 

Par exemple, si vous définissez le format de numéro de build sur la valeur suivante:  
``` 
$(BuildDefinitionName)_1.1.$(DayOfYear)$(Rev:r).0 
```

VSTS génère un numéro de version comme:
```
CI_MyUWPApp_1.1.2501.0
```

>Remarque: le Windows Store exige que le dernier numéro de la version soit0.

Pour pouvoir extraire le numéro de version et l’appliquer au manifeste et/ou aux fichiers `AssemblyInfo`, utilisez un script PowerShell personnalisé (disponible [ici](https://go.microsoft.com/fwlink/?prd=12560&pver=14&plcid=0x409&clcid=0x9&ar=DevCenter&sar=docs)). Ce script lit le numéro de version à partir de la variable d’environnement `BUILD_BUILDNUMBER`, puis modifie les fichiers AssemblyInfo et AppxManifest. Veillez à ajouter ce script à votre référentiel de code source, puis à configurer une tâche de génération PowerShell comme indiqué ici:


![mise à jour de version](images/building-screen13.png) 

La variable `$(AppxVersion)` contient le numéro de version. Vous pouvez utiliser ce numéro dans d’autres étapes de génération. 


#### Facultatif: Intégration avec HockeyApp
D’abord, installez l’extension Visual Studio [HockeyApp](https://marketplace.visualstudio.com/items?itemName=ms.hockeyapp). 

>Remarque: vous devez installer cette extension en tant qu’administrateur VSTS. 


![application hockey](images/building-screen14.png) 

Ensuite, configurez la connexion HockeyApp en vous aidant de ce guide: [Utilisation de HockeyApp avec Visual Studio Team Services (VSTS) ou Team Foundation Server (TFS).](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs) Vous pouvez utiliser votre compte Microsoft, votre compte de réseau social ou simplement une adresse e-mail pour configurer votre compte HockeyApp. Le mode Gratuit inclut deux applications, un propriétaire et aucune restriction de données.

Ensuite, vous pouvez créer  une application HockeyApp manuellement ou en chargeant un fichier de package appx existant. Pour plus d’informations, consultez [Création d’une nouvelle application.](https://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app)  

Pour utiliser un fichier de package appx existant, ajoutez une étape de génération et définissez le paramètre de chemin d’accès au fichier binaire de l’étape de génération. 

![configuration de l’application hockey](images/building-screen15.png) 

Pour définir ce paramètre, combinez le nom de l’application, la variable AppxVersion et les plateformes prises en charge en une seule chaîne comme celle-ci:

``` 
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp_$(AppxVersion)_Test\MyUWPApp_$(AppxVersion)_x86_x64_ARM.appxbundle
```

>Remarque: même si la tâche HockeyApp vous permet de spécifier le chemin d’accès au fichier de symboles, il est recommandé d’inclure les symboles (fichiers appxsym) avec l’offre groupée.

Nous allons vous aider à installer et exécuter un package chargé indépendamment [ultérieurement](#sideloading-best-practices) dans ce guide. 

## Configuration d’une build de déploiement continu qui soumet un package au Windows Store 

Pour générer des packages de soumission au Windows Store, associez votre application au Windows Store à l’aide de l’Assistant Association au Windows Store dans Visual Studio.

![association au Windows Store](images/building-screen16.png) 

>Remarque: cet Assistant génère un fichier nommé Package.StoreAssociation.xml qui contient les informations d’association au Windows Store. Si vous stockez votre code source dans un référentiel public tel que GitHub, ce fichier contient tous les noms réservés de l’application pour ce compte. Vous pouvez exclure ou supprimer ce fichier avant de le rendre publique.

Si vous n’avez pas accès au compte DevCenter qui a été utilisé pour publier l’application, vous pouvez suivre les instructions fournies dans ce document: [Vous créez une application pour un tiers? Création du package de leur application pour le Windows Store.](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/#e35YzR5aRG6uaBqK.97) 

Vous devez ensuite vérifier que l’étape de génération inclut le paramètre suivant:

```
/p:UapAppxPackageBuildMode=StoreUpload 
```

Cette commande génère le fichier appxupload qui peut être soumis au Windows Store.


#### Configuration de la soumission automatique au Windows Store

Utilisez l’extension Visual Studio Team Services pour le Windows Store pour l’intégration avec l’API Windows Store, et envoyez votre package appxupload au Windows Store.

Vous devez connecter votre compte DevCenter avec Azure Active Directory (AD), puis créer une application dans votre AD pour authentifier les demandes. Pour ce faire, vous pouvez suivre les recommandations de la page d’extension. 

Une fois que vous avez configuré l’extension, vous pouvez ajouter la tâche de génération et la configurer avec votre ID d’application et l’emplacement du fichier appxupload.

![configuration du compte DevCenter](images/building-screen17.png) 

Où la valeur du paramètre `Package File` est:

```
$(Build.ArtifactStagingDirectory)\
AppxPackages\MyUWPApp__$(AppxVersion)_x86_x64_ARM_bundle.appxupload
```

>Remarque: Vous devez activer manuellement cette build. Vous pouvez l’utiliser pour mettre à jour des applications existantes, mais vous ne pouvez pas l’utiliser pour votre première soumission au Windows Store. Pour plus d’informations, consultez [Créer et gérer des soumissions au Windows Store à l’aide de Windows Store Services.](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)

## Meilleures pratiques

<span id="sideloading-best-practices"/>
### Meilleures pratiques de chargement indépendant des applications

Si vous souhaitez distribuer votre application sans la publier dans le Windows Store, vous pouvez charger indépendamment votre application directement sur les appareils, dans la mesure où ces appareils approuvent le certificat utilisé pour signer le package de l’application. 

Utilisez le script PowerShell `Add-AppDevPackage.ps1` pour installer des applications. Ce script ajoute le certificat à la section Certification racine de confiance pour l’ordinateur local, puis installe ou met à jour le fichier appx.

#### Chargement indépendant de votre application avec la mise à jour anniversaire de Windows10
Dans la mise à jour anniversaire de Windows10, vous pouvez double-cliquer sur le fichier appxbundle et installer votre application en choisissant le bouton Installer dans une boîte de dialogue. 


![chargement indépendant dans rs1](images/building-screen18.png) 

>Remarque: cette méthode n’installe pas le certificat ou les dépendances associées.

Si vous souhaitez distribuer vos packages appx à partir d’un site Web comme VSTS ou HockeyApp, vous devez ajouter ce site à la liste des sites de confiance dans votre navigateur. Sinon, Windows marque le fichier comme verrouillé. 

<span id="certificates-best-practices"/>
### Meilleures pratiques pour les certificats de signature 
Visual Studio génère un certificat pour chaque projet. Pour cette raison, il est difficile de conserver une liste des certificats valides. Si vous prévoyez de créer plusieurs applications, vous pouvez créer un certificat unique pour la signature de toutes vos applications. Ensuite, chaque appareil approuvant votre certificat peut charger vos applications de manière indépendante sans avoir à installer un autre certificat. Pour en savoir plus, consultez [Création d’un certificat de signature de package d’application.](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)


#### Création d’un certificat de signature
Utilisez l’outil [MakeCert.exe](https://msdn.microsoft.com/en-us/library/windows/desktop/ff548309(%09v=vs.85).aspx) pour créer un certificat. L’exemple suivant crée un certificat à l’aide de l’outil MakeCert.exe.

```
MakeCert /n publisherName /r /h 0 /eku "1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.10.3.13" /e expirationDate /sv MyKey.pvk MyKey.cer
```

Vous pouvez utiliser l’outil Pvk2Pfx pour générer un fichier PFX contenant la clé privée protégée par un mot de passe.

Fournissez ces certificats pour chaque rôle d’ordinateur:

|**Ordinateur**|**Utilisation**|**Certificat**|**Magasin de certificats**|
|-----------|---------|---------------|---------------------|
|Ordinateur de développement/génération|Signature des builds|MyCert.PFX|Utilisateur actuel/Personnel|
|Ordinateur de développement/génération|Exécution|MyCert.cer|Ordinateur local/Personnes autorisées|
|Utilisateur|Exécution|MyCert.cer|Ordinateur local/Personnes autorisées|

>Remarque: vous pouvez également utiliser un certificat d’entreprise déjà approuvé par vos utilisateurs.

#### Connexion de votre application UWP
Visual Studio et MSBuild offrent différentes options pour gérer le certificat que vous utilisez pour signer l’application:

L’une des options consiste à inclure le certificat avec la clé privée (normalement sous la forme d’un fichier .PFX) dans votre solution, puis à référencer le pfx dans le fichier de projet. Vous pouvez gérer cela à l’aide de l’onglet Package de l’éditeur de manifeste.


![création de certificats](images/building-screen19.png) 

L’autre option consiste à installer le certificat sur l’ordinateur de build (Utilisateur actuel/Personnel), puis à utiliser l’option Choisir à partir du Magasin de certificats. Ainsi, vous spécifiez l’empreinte numérique du certificat dans le fichier de projet pour que le certificat soit installé sur tous les ordinateurs qui seront utilisés pour générer le projet.

#### Approbation du certificat de signature dans les appareils cibles
Un appareil cible doit approuver le certificat avant que l’application y soit installée. 

Enregistrez la clé publique du certificat dans l’emplacement Personnes autorisées ou Racine de confiance dans le magasin de certificats Ordinateur Local.

Pour enregistrer votre certificat, il vous suffit de double-cliquer sur le fichier .cer, puis de suivre les étapes de l’Assistant afin d’enregistrer le certificat dans le magasin Ordinateur local et Personnes autorisées.

## Rubriques connexes
* [Créer votre application .NET pour Windows](https://www.visualstudio.com/en-us/docs/build/get-started/dot-net) 
* [Création de packages d’applicationsUWP](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
* [Chargement indépendant d’applications métier dans Windows10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
* [Création d’un certificat de signature de package d’application](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)



<!--HONumber=Nov16_HO1-->


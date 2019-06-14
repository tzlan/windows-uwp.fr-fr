---
Description: Vous pouvez utiliser des extensions pour intégrer votre application de bureau empaquetée avec Windows 10 de manière prédéfinie.
title: Intégrer votre application de bureau empaquetée avec Windows 10 et UWP (pont du bureau)
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 814d8c04943e32ff4d2f0c81bd847e78becd5ebb
ms.sourcegitcommit: a4fe508e62827a10471e2359e81e82132dc2ac5a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66468327"
---
# <a name="integrate-your-packaged-desktop-app-with-windows-10-and-uwp"></a>Intégrer votre application de bureau empaquetée avec Windows 10 et UWP

Si vous [empaqueter votre application de bureau dans un conteneur MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root), vous pouvez utiliser des extensions pour intégrer votre application de bureau empaquetée avec Windows 10 à l’aide des extensions prédéfinies dans le [manifeste de package d’application](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Par exemple, utiliser une extension pour créer une exception de pare-feu, que votre application à l’application par défaut pour un type de fichier ou pointer les vignettes de début vers la version empaquetée de votre application. Pour utiliser une extension, il suffit d’ajouter un peu de XML au fichier manifeste du package de votre application. Aucun code n’est nécessaire.

Cet article décrit ces extensions et les tâches que vous pouvez effectuer en les utilisant.

> [!NOTE]
> Les fonctionnalités décrites dans cet article nécessitent la création d’un package d’application Windows pour votre application de bureau. Si vous n’avez pas encore fait, consultez [empaqueter des applications de bureau](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root).

## <a name="transition-users-to-your-app"></a>Migrer les utilisateurs vers votre application

Aidez les utilisateurs à migrer vers votre application empaquetée.

* [Pointez les vignettes de démarrage existantes et les boutons de barre des tâches sur votre application empaquetée](#point)
* [Rendre votre application empaquetée ouvrir des fichiers au lieu de votre application de bureau](#make)
* [Associer votre application empaquetée avec un ensemble de types de fichiers](#associate)
* [Ajouter des options pour les menus contextuels des fichiers qui ont un certain type de fichier](#add)
* [Ouvrir certains types de fichiers directement à l’aide d’une URL](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Pointer les vignettes existantes de l’écran de démarrage et les boutons de barre des tâches vers votre application empaquetée

Vos utilisateurs peuvent avoir épinglé votre application de bureau à la barre des tâches ou au menu Démarrer. Vous pouvez pointer ces raccourcis vers votre nouvelle application empaquetée.

#### <a name="xml-namespace"></a>Espace de noms XML

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Nom | Description |
|-------|-------------|
|Category |Toujours ``windows.desktopAppMigration``.
|AumID |L’ID de modèle utilisateur de l’application de votre application empaquetée. |
|ShortcutPath |Le chemin d’accès aux fichiers .lnk qui démarrent la version bureau de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition et migration/la désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Rendre votre application empaquetée ouvrir des fichiers au lieu de votre application de bureau

Pour vous assurer que les utilisateurs d’ouvrir votre nouvelle application empaquetée par défaut pour certains types de fichiers au lieu d’ouvrir la version de bureau de votre application.

Pour ce faire, vous devez spécifier l’[identificateur programmatique (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) de chaque application à partir de laquelle vous souhaitez hériter des associations de fichiers.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. Cet ID est utilisé en interne pour générer un [identificateur programmatique (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) associé à votre association de type de fichier. Vous pouvez utiliser cet ID pour gérer les modifications dans les futures versions de votre application. |
|MigrationProgId |Le [identificateur programmatique (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) qui décrit l’application, le composant et la version de l’application de bureau à partir de laquelle vous voulez hériter des associations de fichiers.|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition et migration/la désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Associer votre application empaquetée avec un ensemble de types de fichiers

Vous pouvez associé à votre application empaquetée avec les extensions de type de fichier. Si un utilisateur clique sur un fichier, puis sélectionne le **ouvrir avec** option, votre application s’affiche dans la liste de suggestions.

#### <a name="xml-namespace"></a>Espace de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. Cet ID est utilisé en interne pour générer un [identificateur programmatique (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) associé à votre association de type de fichier. Vous pouvez utiliser cet ID pour gérer les modifications dans les futures versions de votre application.   |
|FileType |L’extension de fichier prise en charge par votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition et migration/la désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Ajouter des options aux menus contextuels de fichiers d'un certain type

Dans la plupart des cas, les utilisateurs double-cliquent sur les fichiers pour les ouvrir. Si les utilisateurs cliquent sur un fichier avec le bouton droit, plusieurs options s’affichent.

Vous pouvez ajouter des options à ce menu. Ces options donnent aux utilisateurs d’autres moyens d’interagir avec votre fichier, par exemple l’impression, la modification ou l’aperçu du fichier.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category | Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|Verb |Le nom qui apparaît dans le menu contextuel Explorateur de fichiers. Cette chaîne est localisable en utilisant ```ms-resource```.|
|Id |L’ID unique du verbe. Si votre application est une application UWP, celui-ci est transmis à votre application dans le cadre de ses arguments d’événement d’activation afin de pouvoir traiter la sélection utilisateur en conséquence. Si votre application est une application empaquetée de confiance totale, il reçoit des paramètres à la place (voir la puce suivante). |
|Paramètres |La liste des paramètres et des valeurs d’arguments associés au verbe. Si votre application est une application empaquetée de confiance totale, ces paramètres sont passés à l’application en tant qu’arguments d’événement lorsque l’application est activée. Vous pouvez personnaliser le comportement de votre application basée sur les verbes d’activation différentes. Si une variable peut contenir un chemin d’accès de fichier, placez la valeur du paramètre entre guillemets. Vous éviterez les problèmes qui se produisent dans les cas où le chemin d’accès contient des espaces. Si votre application est une application UWP, vous ne pouvez pas transmettre des paramètres. L’application reçoit l’ID à la place (voir la puce précédente).|
|Extended |Indique que le verbe doit seulement apparaître, si l’utilisateur maintient enfoncée la touche **Maj** avant de cliquer avec le bouton droit sur le fichier pour afficher le menu contextuel. Cet attribut est facultatif et est défini par défaut sur la valeur **False** (c’est-à-dire que le verbe est toujours affiché) s’il n’est pas répertorié. Vous devez spécifier ce comportement individuellement pour chaque verbe (à l’exception de « Ouvrir », qui est toujours défini sur **False**).|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemple connexe

[Visionneuse d’images WPF avec transition et migration/la désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Ouvrir certains types de fichiers directement à l’aide d’une URL

Pour vous assurer que les utilisateurs d’ouvrir votre nouvelle application empaquetée par défaut pour certains types de fichiers au lieu d’ouvrir la version de bureau de votre application.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|UseUrl |Indique s’il faut ouvrir les fichiers directement à partir d’une cible URL. Si vous ne définissez pas cette valeur, tentatives par votre application pour ouvrir un fichier à l’aide une cause de l’URL du système pour le premier téléchargement le fichier localement. |
|Paramètres |Paramètres facultatifs. |
|FileType |Les extensions de fichier appropriées. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>Effectuer des tâches de configuration

* [Créer l’exception de pare-feu pour votre application](#rules)
* [Placez vos fichiers DLL dans n’importe quel dossier du package](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>Créer une exception de pare-feu pour votre application

Si votre application nécessite la communication via un port, vous pouvez ajouter votre application à la liste des exceptions de pare-feu.

#### <a name="xml-namespace"></a>Espace de noms XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.firewallRules``|
|Exécutable |Le nom du fichier exécutable que vous souhaitez ajouter à la liste des exceptions de pare-feu |
|Direction |Indique si la règle est une règle entrante ou sortante |
|IPProtocol |Le protocole de communication |
|LocalPortMin |Le numéro de port le moins élevé d’une plage de numéros de ports locaux. |
|LocalPortMax |Le numéro de port le plus élevé d’une plage de numéros de ports locaux. |
|RemotePortMax |Le numéro de port le moins élevé d’une plage de numéros de ports distants. |
|RemotePortMax |Le numéro de port le plus élevé d’une plage de numéros de ports distants. |
|Profil |Le type de réseau |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Placer vos fichiers DLL dans n'importe quel dossier du package

Utilisez une extension pour identifier ces dossiers. Ainsi, le système est en mesure de trouver et de charger les fichiers que vous avez placés dans ces dossiers. Considérez cette extension comme le remplacement de la variable d'environnement _%PATH%_ .

Si vous n'utilisez pas cette extension, le système recherche le graphique de dépendance de package de ce processus, le dossier racine du package, puis le répertoire système ( _%SystemRoot%\system32_) dans cet ordre. Pour plus d’informations, consultez [Ordre de recherche des applications Windows](https://docs.microsoft.com/windows/desktop/Dlls/dynamic-link-library-search-order).

Chaque package peut contenir uniquement l'une de ces extensions. En d'autres termes, vous pouvez ajouter l'une d'elles à votre package principale, puis en ajouter une à chacun de vos [packages facultatifs et ensembles relatifs](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

#### <a name="xml-namespace"></a>Espace de noms XML

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

Déclarez cette extension au niveau du package de votre manifeste d'application.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|Nom | Description |
|-------|-------------|
|Category |Toujours ``windows.loaderSearchPathOverride``.
|FolderPath | Le chemin du dossier contenant vos fichiers dll. Spécifiez un chemin relatif au dossier racine du package. Vous pouvez spécifier un maximum de cinq chemins dans une extension. Si vous souhaitez que le système recherche les fichiers dans le dossier racine du package, utilisez une chaîne vide pour l'un de ces chemins. N'incluez aucun chemin dupliqué et assurez-vous que vos chemins ne commencent ni ne se terminent par aucune barre oblique ou barre oblique inverse. <br><br> Le système ne recherchera pas les sous-dossiers. Veillez donc à répertorier explicitement chacun des dossiers contenant des fichiers DLL que le système doit charger.|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>Intégration avec l’Explorateur de fichiers

Aidez les utilisateurs à organiser vos fichiers et à interagir avec eux naturellement.

* [Définir le comportement de votre application lorsque les utilisateurs sélectionnent et ouvrent plusieurs fichiers en même temps](#define)
* [Afficher le contenu du fichier dans une image miniature dans l’Explorateur de fichiers](#show)
* [Afficher le contenu du fichier dans un volet de visualisation de l’Explorateur de fichiers](#preview)
* [Permettre aux utilisateurs de regrouper des fichiers à l’aide de la colonne type dans l’Explorateur de fichiers](#enable)
* [Mise à disposition des propriétés du fichier de recherche, les index, les boîtes de dialogue de propriété et le volet d’informations](#make-file-properties)
* [Spécifiez un gestionnaire de menu contextuel pour un type de fichier](#context-menu)
* [Rendre les fichiers à partir de votre service cloud s’affichent dans l’Explorateur de fichiers](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Définir le comportement de votre application lorsque les utilisateurs sélectionnent et ouvrent plusieurs fichiers en même temps

Spécifiez la façon dont votre application se comporte lorsqu’un utilisateur ouvre plusieurs fichiers simultanément.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|MultiSelectModel |Voir ci-dessous |
|FileType |Les extensions de fichier appropriées. |

**MultSelectModel**

Les applications de bureau empaquetées présentent les trois mêmes options que les applications de bureau standard.

* ``Player``: Votre application est activée une fois. Tous les fichiers sélectionnés sont transmises à votre application en tant que paramètres de l’argument.
* ``Single``: Votre application est activée une fois pour le premier fichier sélectionné. Les autres fichiers sont ignorés.
* ``Document``: Une instance séparée de votre application est activée pour chaque fichier sélectionné.

 Vous pouvez définir des préférences spécifiques pour les différents types de fichiers et d’actions. Par exemple, il se peut que vous souhaitiez ouvrir les *documents* en mode *Document* et les *images* en mode *Player*.

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Si l’utilisateur ouvre jusqu’à 15 fichiers, le choix par défaut pour l’attribut **MultiSelectModel** est *Player*. Autrement, la valeur par défaut est *Document*. Les applications UWP sont toujours démarrées en mode *Player*.

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Afficher le contenu du fichier dans une image miniature dans l’Explorateur de fichiers

Permettez aux utilisateurs d’afficher une image miniature du contenu du fichier lorsque l’icône du fichier s’affiche en taille moyenne, grande ou très grande.

#### <a name="xml-namespace"></a>Espace de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|FileType |Les extensions de fichier appropriées. |
|Clsid   |L’ID de classe de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Afficher le contenu du fichier dans le volet de visualisation de l’Explorateur de fichiers

Permettez aux utilisateurs d’afficher un aperçu du contenu d’un fichier dans le volet de visualisation de l’Explorateur de fichiers.

#### <a name="xml-namespace"></a>Espace de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|FileType |Les extensions de fichier appropriées. |
|Clsid   |L’ID de classe de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Permettre aux utilisateurs de grouper des fichiers à l’aide de la colonne Type dans l’Explorateur de fichiers

Vous pouvez associer une ou plusieurs valeurs prédéfinies pour vos types de fichiers avec le champ **Type**.

Dans l’Explorateur de fichiers, les utilisateurs peuvent regrouper ces fichiers à l’aide de ce champ. Les composants du système utilisent également ce champ à différentes fins, telles que l’indexation.

Pour plus d’informations sur le champ **Type** et les valeurs que vous pouvez utiliser pour ce champ, consultez [Utilisation des noms de type](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|FileType |Les extensions de fichier appropriées. |
|valeur |Une [Valeur de type](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) valide. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="Contoso">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Mettre à disposition les propriétés du fichier pour la recherche, les index, les boîtes de dialogue des propriétés et le volet d’informations

#### <a name="xml-namespace"></a>Espace de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fileTypeAssociation``.
|Nom |Un ID unique pour votre application. |
|FileType |Les extensions de fichier appropriées. |
|Clsid  |L’ID de classe de votre application. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="context-menu" />

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>Spécifiez un gestionnaire de menu contextuel pour un type de fichier

Si votre application de bureau définit un [Gestionnaire de menu contextuel](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers), utilisez cette extension pour inscrire le Gestionnaire de menu.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/foundation/windows10
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/4

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

Rechercher la référence de schéma complet ici : [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) et [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus).

#### <a name="instructions"></a>Instructions

Pour inscrire votre gestionnaire de menu contextuel, suivez ces instructions.

1. Dans votre application de bureau, mettre en œuvre un [Gestionnaire de menu contextuel](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers) en implémentant la [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) ou [IExplorerCommandState](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate) interface. Pour obtenir un exemple, consultez le [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb) exemple de code. Assurez-vous que vous définissez une classe GUID pour chacun de vos objets de mise en œuvre. Par exemple, le code suivant définit un ID de classe pour une implémentation de [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand).

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. Dans votre manifeste de package, spécifiez un [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) extension d’application qui inscrit un serveur de substitution COM avec l’ID de classe de votre implémentation de gestionnaire de menu contextuel.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. Dans votre manifeste de package, spécifiez un [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) extension d’application qui inscrit votre implémentation de gestionnaire de menu contextuel.

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Permettre l'affichage des fichiers de votre service cloud dans l'explorateur de fichiers

Inscrivez les gestionnaires que vous souhaitez implémenter dans votre application. Vous pouvez également ajouter des options de menu local qui apparaîtront lorsque les utilisateur cliquent avec le bouton droit sur vos fichiers cloud dans l'explorateur de fichier.

#### <a name="xml-namespace"></a>Espace de noms XML

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.cloudfiles``.
|iconResource |L'icône représentant votre service fournisseur de fichier cloud. L'icône apparaît dans le volet Navigation de l'explorateur de fichier.  Les utilisateurs sélectionnent cette icône pour afficher les fichiers provenant de votre service cloud. |
|CustomStateHandler Clsid |L’ID de classe de l’application qui implémente le CustomStateHandler. Le système utilise cet ID de classe pour demander les états et colonnes personnalisés des fichiers cloud. |
|ThumbnailProviderHandler Clsid |L’ID de classe de l’application qui implémente le ThumbnailProviderHandler. Le système utilise cet ID de classe pour demander les images miniatures des fichiers cloud. |
|ExtendedPropertyHandler Clsid |L’ID de classe de l’application qui implémente le ExtendedPropertyHandler.  Le système utilise cet ID de classe pour demander les propriétés étendues d'un fichier cloud. |
|Verb |Le nom qui apparaît dans le menu local de l'explorateur de fichier pour les fichiers fournis par votre service cloud. |
|Id |L’ID unique du verbe. |

#### <a name="example"></a>Exemple

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>Démarrez votre application de différentes façons

* [Démarrez votre application à l’aide d’un protocole](#protocol)
* [Démarrez votre application en utilisant un alias](#alias)
* [Démarrer un fichier exécutable quand les utilisateurs se connectent à Windows](#executable)
* [Permettre aux utilisateurs de démarrer votre application lorsqu’ils connectent à un appareil à leur PC](#autoplay)
* [Redémarrer automatiquement après la réception d’une mise à jour à partir du Microsoft Store](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>Démarrez votre application à l’aide d’un protocole

Les associations de protocole permettent l’interopérabilité entre votre application empaquetée et les autres programmes et composants système. Lorsque votre application empaquetée est démarrée à l’aide d’un protocole, vous pouvez spécifier des paramètres spécifiques à transmettre à ses arguments d’événement d’activation afin qu’il peut se comporter en conséquence. Les paramètres sont pris en charge uniquement pour les applications empaquetées et de confiance totale. Les applications UWP ne peuvent pas utiliser de paramètres.

#### <a name="xml-namespace"></a>Espace de noms XML

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.protocol``.
|Nom |Le nom du protocole. |
|Paramètres |La liste des paramètres et valeurs à transmettre à votre application en tant qu’arguments d’événement lorsque l’application est activée. Si une variable peut contenir un chemin d’accès de fichier, placez la valeur du paramètre entre guillemets. Vous éviterez les problèmes qui se produisent dans les cas où le chemin d’accès contient des espaces. |

### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>Démarrez votre application en utilisant un alias

Les utilisateurs et autres processus peuvent utiliser un alias pour démarrer votre application sans avoir à spécifier le chemin d’accès complet à votre application. Vous pouvez spécifier ce nom d’alias.

#### <a name="xml-namespaces"></a>Espaces de noms XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.appExecutionAlias``.
|Exécutable |Le chemin d’accès relatif du fichier exécutable à démarrer lorsque l’alias est appelé. |
|Alias |Le nom court de votre application. Il doit toujours se terminer par l’extension « .exe ». Vous ne pouvez spécifier qu’un seul alias d’exécution d’application pour chaque application du package. Si plusieurs applications présentent le même alias, le système appellera la dernière inscrite. Prenez donc soin de choisir un alias peu susceptible d’être remplacé par d’autres applications.
|

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
 
...
</Package>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Démarrer un fichier exécutable lorsque les utilisateurs se connectent à Windows

Tâches de démarrage permettent à votre application exécuter automatiquement un fichier exécutable chaque fois qu’un utilisateur ouvre une session.

> [!NOTE]
> L’utilisateur dispose démarrer votre application au moins une fois pour inscrire cette tâche de démarrage.

Votre application peut déclarer plusieurs tâches de démarrage. Chaque tâche démarre indépendamment. Toutes les tâches de démarrage apparaissent dans le Gestionnaire des tâches sous l’onglet **Démarrage** avec le nom que vous spécifiez dans le manifeste de votre application et l’icône de votre application. Le Gestionnaire des tâches analyse automatiquement l’impact de vos tâches sur le démarrage.

Les utilisateurs peuvent désactiver manuellement la tâche de démarrage de votre application à l’aide du Gestionnaire des tâches. Si un utilisateur désactive une tâche, vous ne pouvez pas la réactiver par programme.

#### <a name="xml-namespace"></a>Espace de noms XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.startupTask``.|
|Exécutable |Le chemin d’accès relatif au fichier exécutable à démarrer. |
|TaskId |Un identificateur unique pour votre tâche. À l’aide de cet identificateur, votre application peut appeler les API le [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) classe pour activer ou désactiver une tâche de démarrage par programme. |
|Enabled |Indique si la tâche est activée ou désactivée au démarrage. Les tâches activées seront exécutées la prochaine fois que l’utilisateur se connecte (sauf si celui-ci les désactive). |
|DisplayName |Le nom de la tâche qui s’affiche dans le Gestionnaire des tâches. Vous pouvez localiser cette chaîne à l’aide de ```ms-resource```. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permettre aux utilisateurs de démarrer votre application lorsqu’ils connectent à un appareil à leur PC

Lecture automatique peut présenter votre application en tant qu’option lorsqu’un utilisateur connecte à un appareil à leur PC.

#### <a name="xml-namespace"></a>Espace de noms XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.autoPlayHandler``.
|ActionDisplayName |Chaîne qui représente l’action par les utilisateurs peuvent effectuer avec un appareil qu’ils se connectent à un PC (par exemple : « Importation de fichiers » ou « Lire la vidéo »). |
|ProviderDisplayName | Chaîne qui représente votre application ou service (par exemple : « Contoso lecteur vidéo »). |
|ContentEvent |Le nom d’un événement de contenu qui envoie une invite aux utilisateurs avec vos éléments ``ActionDisplayName`` et ``ProviderDisplayName``. Les événements de contenu se déclenchent lorsqu’un périphérique de volume, tel que la carte mémoire d’un appareil photo, une clé USB ou un DVD, est inséré dans le PC. Vous trouverez la liste complète de ces événements [ici](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verb |Le paramètre verbe identifie une valeur qui est passée à votre application pour l’option sélectionnée. Vous pouvez spécifier plusieurs options de lancement pour un événement de lecture automatique et utiliser le paramètre Verbe pour déterminer l’option sélectionnée par l’utilisateur pour votre application. Vous pouvez vérifier quelle option a été sélectionnée par l’utilisateur par le biais de la propriété verb des arguments d’événement de démarrage transmis à votre application. Vous pouvez attribuer n’importe quelle valeur au paramètre Verbe, sauf la valeur Open qui est réservée. |
|DropTargetHandler |L’ID de classe de l’application qui implémente le [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) interface. Les fichiers du média amovibles sont transmis à la méthode [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) de votre implémentation [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017).  |
|Paramètres |Vous n'êtes pas obligé d'implémenter l'interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) pour tous les événements de contenu. Pour tous les événements de contenu, vous avez la possibilité de fournir des paramètres de ligne de commande au lieu d'implémenter l'interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017). Pour ces événements, lecture automatique démarre votre application à l’aide de ces paramètres de ligne de commande. Vous pouvez analyser ces paramètres dans le code d'initialisation de votre application afin de déterminer s'il a été démarré par la lecture automatique, puis fournir votre implémentation par défaut. |
|DeviceEvent |Le nom d’un événement d'appareil qui envoie une invite aux utilisateurs avec vos éléments ``ActionDisplayName`` et ``ProviderDisplayName``. Un événement d'appareil est déclenché lorsqu’un appareil est connecté au PC. Les événements d'appareil commencent par la chaîne ``WPD``. Ils sont répertoriés [ici](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |L’ID de classe de l’application qui implémente le [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) interface. |
|InitCmdLine |Le paramètre de chaîne que vous souhaitez transmettre dans la méthode [Initialiser](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) de l'interface [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |

### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Redémarrer automatiquement après avoir reçu une mise à jour à partir du Microsoft Store

Si votre application est ouverte quand les utilisateurs installent une mise à jour à ce dernier, l’application se ferme.

Si vous souhaitez que cette application à redémarrer après la mise à jour est terminée, appelez le [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) fonction dans chaque processus que vous souhaitez redémarrer.

Chaque fenêtre active dans votre application reçoit un [WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession) message. À ce stade, votre application peut appeler le [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) fonction à nouveau pour mettre à jour la ligne de commande si nécessaire.

Quand chaque fenêtre active dans votre application reçoit le [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) message, votre application doit enregistrer les données et les arrêter.

>[!NOTE]
Vos fenêtres actives également recevoir le [WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close) message au cas où l’application ne gère pas la [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) message.

À ce stade, votre application doit le fermer son propre processus de 30 secondes ou la plateforme procède à leur arrêt forcé.

Une fois la mise à jour est terminée, votre application redémarre.

## <a name="work-with-other-applications"></a>Utiliser d’autres applications

Intégration avec d’autres applications, démarrer d’autres processus ou partager des informations.

* [Rendre votre application apparaît comme l’impression cible dans les applications qui prennent en charge l’impression](#printing)
* [Polices de partage avec d’autres applications Windows](#fonts)
* [Démarrer un processus Win32 à partir d’une application de plateforme universelle Windows (UWP)](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Rendre votre application apparaît comme l’impression cible dans les applications qui prennent en charge l’impression

Lorsque les utilisateurs souhaitent imprimer des données à partir d’une autre application tel que le bloc-notes, vous pouvez rendre votre application apparaissent sous la forme d’une cible d’impression dans la liste de l’application des cibles d’impression disponibles.

Vous devrez modifier votre application pour qu’il reçoive des données d’impression au format de la spécification XPS (XML Paper).

#### <a name="xml-namespaces"></a>Espaces de noms XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Vous trouverez la référence de schéma complète [ici](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.appPrinter``.
|DisplayName |Le nom que vous souhaitez voir apparaître dans la liste des cibles d’impression pour une application. |
|Paramètres |Tous les paramètres requis par votre application pour gérer correctement la demande. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

Trouver un exemple qui utilise cette extension [ici](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>Partager des polices avec d’autres applications Windows

Partagez vos polices personnalisées avec d’autres applications Windows.

#### <a name="xml-namespaces"></a>Espaces de noms XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

Vous trouverez la référence de schéma complète [ici](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.sharedFonts``.
|Fichier |Le fichier qui contient les polices que vous souhaitez partager. |

#### <a name="example"></a>Exemple

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Démarrer un processus Win32 à partir d’une application de plateforme Windows universelle (UWP)

Démarrer un processus Win32 qui s’exécute en mode de confiance totale.

#### <a name="xml-namespaces"></a>Espaces de noms XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Éléments et attributs de cette extension

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|Nom |Description |
|-------|-------------|
|Category |Toujours ``windows.fullTrustProcess``.
|GroupID |Une chaîne qui identifie un ensemble de paramètres que vous souhaitez transmettre au fichier exécutable. |
|Paramètres |Paramètres que vous souhaitez transmettre au fichier exécutable. |

#### <a name="example"></a>Exemple

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Cette extension peut être utile si vous souhaitez créer une interface utilisateur de plateforme Windows universelle qui s’exécute sur tous les appareils, mais vous souhaitez que les composants de votre application Win32 à poursuivre son exécution en confiance totale.

Créez simplement un package d’application Windows pour votre application Win32. Ensuite, ajoutez cette extension au fichier de package de votre application UWP. Cette extensions indique que vous souhaitez démarrer un fichier exécutable dans le package d’application Windows.  Si vous souhaitez communiquer entre votre application UWP et votre application Win32, vous pouvez configurer un ou plusieurs [services d’application](/windows/uwp/launch-resume/app-services.md) pour ce faire. Pour en savoir plus sur ce scénario, reportez-vous [ici](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions ? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Donner votre avis ou faire des suggestions de fonctionnalité**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
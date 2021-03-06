---
title: Empaqueter votre jeu de plateforme Windows universelle (UWP) DirectX
description: Certains jeux de plateforme Windows universelle (UWP) qui prennent notamment en charge plusieurs langues et comprennent des ressources spécifiques à la région ou des ressources haute définition facultatives peuvent devenir facilement très volumineux.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, jeux, DirectX, package
ms.localizationpriority: medium
ms.openlocfilehash: 8a1e255a5219e39d866915bce25778dc8f1dfdba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175243"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>Empaqueter votre jeu de plateforme Windows universelle (UWP) DirectX

Certains jeux de plateforme Windows universelle (UWP) qui prennent notamment en charge plusieurs langues et comprennent des ressources spécifiques à la région ou des ressources haute définition facultatives peuvent devenir facilement très volumineux. Dans cette rubrique, découvrez comment utiliser les packages et ensembles d’applications pour personnaliser votre application afin que vos clients ne reçoivent que les ressources dont ils ont réellement besoin.

En complément du modèle de package d’application, Windows 10 prend en charge les lots d’applications qui groupes deux types de packages :

-   Les packages d’application qui contiennent des fichiers exécutables et de bibliothèques spécifiques à la plateforme. Un jeu UWP peut compter jusqu’à trois packages d’application : un par architecture d’UC x86, x64 et ARM. L’ensemble du code et des données spécifiques à cette plateforme matérielle doit être inclus dans son package d’application. Ce dernier doit également contenir toutes les ressources principales pour que le jeu s’exécute avec un niveau de fidélité et de performance de base.
-   Les packs de ressources contiennent des données non spécifiques d’une plateforme étendues ou facultatives telles que les ressources de jeu (textures, maillages, son et texte). Un jeu UWP peut comporter un ou plusieurs packs de ressources, notamment pour les textures ou les ressources haute définition, les ressources de niveau de fonctionnalité DirectX 11 ou supérieur, ou les ressources spécifiques d’une langue.

Pour plus d’informations sur les ensembles d’applications et sur les packages d’application, voir [Définition des ressources d’application](/previous-versions/windows/apps/hh965321(v=win.10)).

Même si vous pouvez placer la totalité du contenu dans vos packages d’application, cette approche est inefficace et redondante. Pourquoi recourir au même fichier de texture de grande taille répliqué trois fois pour chaque plateforme, notamment pour les plateformes ARM qui ne l’utiliseront peut-être pas ? Votre objectif va consister à limiter le volume du contenu à télécharger. Les utilisateurs peuvent ainsi commencer plus rapidement à jouer à votre jeu, en gagnant de l’espace sur leur appareil et en économisant d’éventuels frais de bande passante limitée.

Pour utiliser cette fonctionnalité du programme d’installation d’une application UWP, tenez compte de la disposition du répertoire et des conventions d’affectation de nom pour la création de package d’application et de ressources dès le début du développement du jeu. Vos outils et vos sources peuvent ainsi correctement générer des sorties et simplifier la création de package. Suivez les règles énoncées dans ce document quand vous développez ou configurez des scripts et des outils de création et de gestion de ressources ou quand vous créez du code qui charge ou référence des ressources.

## <a name="why-create-resource-packs"></a>Pourquoi créer des packs de ressources ?


Quand vous créez une application, notamment une application de jeu que vous pouvez vendre dans de nombreux pays ou dans un large éventail de plateformes matérielles UWP, vous devez souvent ajouter plusieurs versions de nombreux fichiers pour prendre en charge ces paramètres régionaux ou plateformes. Par exemple, si vous publiez votre jeu aux États-Unis et au Japon, vous pouvez avoir besoin d’un jeu de fichiers audio en anglais pour les paramètres régionaux en-us, et d’un autre en japonais pour le paramètre régional jp-jp. Sinon, si vous voulez utiliser une image dans votre jeu pour les appareils ARM, ainsi que les plateformes x86 et x64, vous devez charger vers le serveur les mêmes ressources d’image 3 fois, une par architecture d’UC.

En outre, si votre jeu est constitué de nombreuses ressources haute définition qui ne s’appliquent pas aux plateformes dotées de niveaux de fonctionnalités DirectX inférieurs, pourquoi les inclure dans le package d’application de base et obliger votre utilisateur à télécharger un volume important de composants inutilisables par son appareil ? En plaçant ces ressources haute définition dans un pack de ressources facultatif, vous permettez aux utilisateurs équipés d’appareils prenant en charge ces ressources haute définition de les obtenir au coût de la bande passante (éventuellement limitée). Ceux dont les appareils ne prennent pas en charge la haute définition peuvent ainsi obtenir leur jeu plus rapidement et à un coût d’utilisation du réseau inférieur.

Contenu susceptible d’intégrer des packs de ressources de jeu :

-   Ressources spécifiques de paramètres internationaux (texte, audio ou images localisés)
-   Ressources haute résolution pour différents facteurs d’échelle d’appareil (1.0x, 1.4x et 1.8x)
-   Ressources haute définition pour des niveaux de fonctionnalités DirectX plus élevés (9, 10 et 11)

Tout cela est défini dans le fichier package.appxmanifest qui fait partie de votre projet UWP, ainsi que dans la structure de répertoires de votre package final. Avec cette nouvelle interface utilisateur Visual Studio, en suivant la procédure figurant dans ce document, vous n’avez pas besoin d’effectuer des modifications manuelles.

> **Important**    Le chargement et la gestion de ces ressources sont contrôlés par le biais des API **Windows. ApplicationModel. Resources** \* . Si vous utilisez ces API de ressources de modèle d’applications pour charger le fichier qui convient pour un paramètre régional, un facteur d’échelle ou un niveau de fonctionnalité DirectX spécifiques, vous n’avez pas besoin de charger vos ressources à l’aide de chemins d’accès de fichiers explicites. Vous fournissez à la place les API de ressources avec simplement le nom du fichier généralisé de la ressource souhaitée. Le système de gestion des ressources se charge ensuite d’obtenir la variante correcte de la ressource de la plateforme active et de la configuration régionale de l’utilisateur (que vous pouvez directement spécifier comme avec ces mêmes API).

 

Les ressources utilisées pour la création de packs de ressources sont spécifiées de l’une de ces deux manières de base suivantes :

-   Les fichiers de ressources portent le même nom de fichier et les versions spécifiques du pack de ressources sont placées dans des répertoires nommés particuliers. Ces noms de répertoires sont réservés par le système. Par exemple, en \\ -US, \\ scale-140, \\ dxfl-DX11.
-   Les fichiers de ressources sont stockés dans des dossiers avec des noms arbitraires. Toutefois, les fichiers sont nommés avec une étiquette commune qui est ajoutée à l’aide des chaînes réservées par le système pour indiquer la langue ou d’autres qualificateurs. Plus précisément, les chaînes de qualificateur sont apposées sur le nom de fichier généralisé après un trait de soulignement (« \_ »). Par exemple, \\ le \\ menu composants \_ option1 \_lang-en-us.png, le \\ \\ menu ressources \_ option1 \_scale-140.png, \\ Assets \\ CoolSign \_ dxfl-DX11. DDS. Vous pouvez également combiner ces chaînes. Par exemple, \\ \\ le menu ressources \_ option1 \_ Scale-140 \_lang-en-us.png.
    > **Remarque**    Lorsqu’il est utilisé dans un nom de fichier plutôt que seul dans un nom de répertoire, un qualificateur de langue doit prendre la forme « lang- <tag> », par exemple « lang-fr-US », comme décrit dans [adapter vos ressources à la langue, à l’échelle et à d’autres qualificateurs](../app-resources/tailor-resources-lang-scale-contrast.md).

     

Les noms de répertoires peuvent être combinés pour afficher une spécificité supplémentaire au cours de la création de pack de ressources. Toutefois, ils ne peuvent pas être redondants. Par exemple, \\ le menu en-US \\ \_ option1 \_lang-en-us.png est redondant.

Vous pouvez spécifier tous les noms de sous-répertoires non réservés dont vous avez besoin sous le répertoire d’une ressource. Il suffit que la structure du répertoire soit identique dans chaque répertoire de ressource. Par exemple, \\ dxfl-facilement \\ Assets \\ textures \\ CoolSign. DDS. Quand vous chargez ou référencez une ressource, le chemin d’accès doit être généralisé par la suppression des qualificateurs de la langue, de l’échelle ou du niveau de fonctionnalité DirectX, que ce soit dans les nœuds de dossier ou dans les noms de fichier. Par exemple, pour faire référence au code d’un élément multimédia pour lequel l’une des variantes est \\ dxfl-facilement \\ \\ , textures de ressources \\ CoolSign. DDS, utilisez les \\ \\ textures de ressources \\ CoolSign. DDS. De même, pour faire référence à un élément multimédia avec un \\ \\scale-140.png d’arrière-plan d’images variant \_ , utilisez des \\ images \\background.png.

Voici les noms de répertoires réservés et les suffixes de nom de fichier derrière le trait de soulignement suivants :

| Type de ressource                   | Nom de répertoire du pack de ressources                                                                                                                  | Suffixe de nom de fichier du pack de ressources                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Ressources localisées             | Toutes les langues possibles ou les combinaisons de langues et de paramètres régionaux possibles pour Windows 10. (Le préfixe qualificateur « lang- » n’est pas nécessaire dans un nom de dossier.) | Un « \_ » suivi du spécificateur Language, locale ou Language-locale. Par exemple, « \_ fr », « \_ US » ou « \_ fr-fr », respectivement. |
| Ressources de facteur d’échelle        | scale-100, scale-140 ou scale-180. Ces valeurs conviennent pour les facteurs d’échelle d’interface utilisateur 1.0x, 1.4x et 1.8x respectivement.                                     | « \_ » Suivi de « Scale-100 », « Scale-140 » ou « Scale-180 ».                                                                    |
| Ressources de niveau de fonctionnalité DirectX | dxfl-dx9, dxfl-dx10 et dxfl-dx11. Ces valeurs conviennent pour les niveaux de fonctionnalités DirectX 9, 10 et 11, respectivement.                                     | « \_ » Suivi de « dxfl-virtuel DX9 », « dxfl-facilement » ou « dxfl-DX11 ».                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>Définition de packs de ressources de langues localisées


Les fichiers propres aux paramètres régionaux sont placés dans les répertoires de projets nommés selon la langue (par exemple, « en »)

Lorsque vous configurez votre application pour prendre en charge les ressources localisées pour plusieurs langues, vous devez effectuer les opérations suivantes :

-   Créez un sous-répertoire d’application (ou version de fichier) pour chaque langue et paramètre régional pris en charge (comme en-us, jp-jp, zh-cn, fr-fr, etc.).
-   Pendant le développement, placez des copies de TOUTES les ressources (telles que les fichiers audio, les textures et les graphiques de menus localisés) dans le sous-répertoire du paramètre régional de la langue correspondant, même si elles sont similaires entre les langues et les paramètres régionaux. Pour garantir la meilleure expérience utilisateur possible, veillez à ce que l’utilisateur soit alerté s’il n’obtient pas le pack de ressources de langue qui convient à ses paramètres régionaux, s’il existe (ou si les packs de ressources ont été accidentellement supprimés après le téléchargement et l’installation).
-   Veillez à ce que chaque ressource ou fichier de ressource de chaîne (.resw) porte le même nom dans chaque répertoire. Par exemple, les \_option1.png de menu doivent avoir le même nom dans les \\ répertoires en-US et \\ JP-JP même si le contenu du fichier est destiné à une langue différente. Dans ce cas, vous pouvez les afficher en tant que \\ menu en-us \\ \_option1.png et en \\option1.png de menu JP-JP \\ \_ .
    > **Remarque**    Vous pouvez éventuellement ajouter les paramètres régionaux au nom de fichier et les stocker dans le même répertoire ; par exemple, \\ le \\ menu composants \_ option1 \_lang-en-us.png \\ , \\ menu ressources \_ option1 \_lang-jp-jp.png.

     

-   Utilisez les API dans [**Windows. ApplicationModel. Resources**](/uwp/api/Windows.ApplicationModel.Resources) et [**Windows. ApplicationModel. resources. Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) pour spécifier et charger les ressources spécifiques aux paramètres régionaux pour votre application. En outre, utilisez des références de ressources qui n’incluent pas les paramètres régionaux spécifiques, car ces API déterminent les paramètres régionaux appropriés en fonction des paramètres de l’utilisateur, puis récupèrent la ressource appropriée pour l’utilisateur.
-   Dans Microsoft Visual Studio 2015, sélectionnez **PROJET-&gt;Windows Store-&gt;Créer un package d’application**, puis créez le package.

## <a name="defining-scaling-factor-resource-packs"></a>Définition des packs de ressources du facteur d’échelle


Windows 10 fournit trois facteurs d’échelle d’interface utilisateur : 1.0x, 1.4x et 1.8x. Les valeurs d’échelle pour chaque affichage sont définies pendant l’installation en fonction de la combinaison de plusieurs facteurs : la taille et la résolution de l’écran et la distance moyenne supposée qui sépare l’utilisateur de l’écran. L’utilisateur peut également modifier les facteurs d’échelle pour améliorer la lisibilité. Votre jeu doit assurer la prise en charge DPI et des facteurs d’échelle pour offrir la meilleure expérience possible. Vous devez donc créer des versions de ressources visuelles critiques pour chacun de ces trois facteurs d’échelle. Cela inclut également l’interaction avec le pointeur et le test de résultats !

Quand vous configurez votre application pour prendre en charge des packs de ressources de différents facteurs d’échelle d’applications UWP, vous devez effectuer les actions suivantes :

-   Créez un sous-répertoire d’application (ou version de fichier) pour chaque facteur d’échelle pris en charge (scale-100, scale-140, et scale-180).
-   Pendant le développement, placez des copies appropriées en fonction du facteur d’échelle de TOUTES les ressources dans chaque répertoire de ressources de facteur d’échelle, même si elles sont similaires entre les différents facteurs d’échelle.
-   Veillez à ce que chaque ressource porte le même nom dans chaque répertoire. Par exemple, les \_option1.png de menu doivent avoir le même nom dans les \\ répertoires scale-100 et \\ Scale-180, même si le contenu du fichier est différent. Dans ce cas, vous pouvez les voir sous la forme du menu \\ Scale-100 \\ \_option1.png et de \\ menu Scale-140 \\ \_option1.png.
    > **Remarque**    Là encore, vous pouvez éventuellement ajouter le suffixe du facteur d’échelle au nom du fichier et le stocker dans le même répertoire ; par exemple, \\ le \\ menu composants \_ option1 \_scale-100.png \\ , \\ menu ressources \_ option1 \_scale-140.png.

     

-   Utilisez les API de [**Windows. ApplicationModel. resources. Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) pour charger les ressources. Les références de ressources doivent être généralisées (sans suffixe), en laissant de côté la variation d’échelle spécifique. Le système récupère la ressource d’échelle appropriée pour l’affichage et les paramètres de l’utilisateur.
-   Dans Visual Studio 2015, sélectionnez **PROJET-&gt;Windows Store-&gt;Créer un package d’application**, puis créez le package.

## <a name="defining-directx-feature-level-resource-packs"></a>Définition des packs de ressources de niveau de fonctionnalité DirectX


Les niveaux de fonctionnalités DirectX correspondent aux jeux de fonctionnalités de l’unité de traitement graphique (GPU) relatifs aux versions précédentes et actuelles de DirectX (en particulier, Direct3D). Cela inclut les spécifications et les fonctionnalités du modèle de nuanceur, la prise en charge de la langue du nuanceur, la prise en charge de la compression de la texture et les fonctionnalités de pipeline graphique globales.

Votre package d’application de base doit utiliser les formats de compression de texture de base : BC1, BC2 ou BC3. Ces formats peuvent être consommés par tout appareil UWP, depuis les plateformes ARM à faible résolution jusqu’aux stations de travail multi-GPU et ordinateurs multimédias.

La prise en charge du format de texture au niveau de fonctionnalité DirectX 10 ou supérieur doit être ajouté dans un pack de ressources pour conserver de l’espace disque en local et de la bande passante de téléchargement. Cela permet d’utiliser des schémas de compression avancés pour la version 11, comme BC6H et BC7. (Pour plus d’informations, voir [Compression de bloc de texture dans Direct3D 11](/windows/desktop/direct3d11/texture-block-compression-in-direct3d-11).) Ces formats sont plus efficaces pour les ressources de texture haute résolution prises en charge par les GPU modernes. Leur utilisation améliore l’apparence, les performances et les exigences en termes d’espace de votre jeu sur des plateformes de haute qualité.

| Niveau de fonctionnalité DirectX | Compression de texture prise en charge |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

Chaque niveau de fonctionnalité DirectX prend en charge différentes versions du modèle de nuanceur. Vous pouvez créer les ressources du nuanceur compilées par niveau de fonctionnalité, puis les inclure dans les packs de ressources de niveau de fonctionnalité DirectX. En outre, certains modèles de nuanceur récents peuvent utiliser des ressources comme les cartes normales, alors que des modèles de nuanceur plus anciens ne le peuvent pas. Vous pouvez également inclure ces ressources spécifiques de modèle de nuanceur dans un pack de ressources de niveau de fonctionnalité DirectX.

Le mécanisme de ressource se concentre principalement sur les formats de texture pris en charge pour les ressources. Ainsi, il ne prend en charge que les trois niveaux de fonctionnalités globaux. Si vous devez avoir des nuanceurs distincts pour les sous-niveaux (versions de points) comme virtuel DX9 \_ 1 vs virtuel DX9 \_ 3, votre code de gestion et de rendu des actifs doit les gérer explicitement.

Quand vous configurez votre application pour prendre en charge des packs de ressources de différents niveaux de fonctionnalités DirectX, vous devez effectuer les actions suivantes :

-   Créez un sous-répertoire d’application (ou version de fichier) pour chaque niveau de fonctionnalité DirectX pris en charge (dxfl-dx9, dxfl-dx10 et dxfl-dx11).
-   Pendant le développement, placez les ressources spécifiques de niveau de fonctionnalité dans chaque répertoire de ressource de niveau de fonctionnalité. Contrairement aux paramètres régionaux et aux facteurs d’échelle, vous pouvez disposer de différentes ramifications de code de rendu pour chaque niveau de fonctionnalité dans votre jeu. Si vous utilisez des textures, des nuanceurs compilés ou d’autres ressources qui ne sont utilisées dans un niveau de fonctionnalité ou un sous-ensemble comprenant tous les niveaux de fonctionnalités, ne placez les ressources correspondantes que dans les répertoires relatifs aux niveaux de fonctionnalités qui les utilisent. Si des ressources sont chargées dans tous les niveaux de fonctionnalités, veillez à ce que chaque répertoire de ressource de niveau de fonctionnalité dispose d’une version du même nom. Par exemple, pour une texture indépendante au niveau de la fonctionnalité nommée « CoolSign. DDS », placez la version compressée BC3 dans le \\ répertoire dxfl-virtuel DX9 et la version compressée Bc7 dans le \\ répertoire dxfl-DX11.
-   Veillez à ce que chaque ressource (si elle est disponible dans plusieurs niveaux de fonctionnalités) porte le même nom dans chaque répertoire. Par exemple, CoolSign. DDS doit avoir le même nom dans les \\ répertoires dxfl-virtuel DX9 et \\ dxfl-DX11, même si le contenu du fichier est différent. Dans ce cas, vous pouvez les voir comme \\ dxfl-virtuel DX9 \\ CoolSign. DDS et \\ dxfl-DX11 \\ CoolSign. DDS.
    > **Remarque**    Là encore, vous pouvez éventuellement ajouter le suffixe du niveau de fonctionnalité au nom de fichier et les stocker dans le même répertoire ; par exemple, les \\ textures \\ CoolSign \_ dxfl-DX9. DDS, \\ textures \\ CoolSign \_ dxfl-DX11. DDS.

     

-   Déclarez les niveaux de fonctionnalités DirectX pris en charge quand vous configurez vos ressources graphiques.
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   Utilisez les API de [**Windows. ApplicationModel. resources. Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) pour charger les ressources. Les références de ressources doivent être généralisées (sans suffixe), en laissant de côté le niveau de fonctionnalité. Cependant, contrairement à la langue et à l’échelle, le système ne détermine pas automatiquement le niveau de fonctionnalité optimal pour un affichage donné. C’est à vous de le déterminer via la logique du code. Une fois cette opération effectuée, utilisez les API pour indiquer au système d’exploitation le niveau de fonctionnalité préféré. Le système pourra ensuite récupérer la ressource appropriée en fonction de cette préférence. Voici un exemple de code qui montre comment indiquer à votre application le niveau de fonctionnalité DirectX actuel de la plateforme :
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **Remarque**    Dans votre code, chargez la texture directement par nom (ou chemin d’accès sous le répertoire de niveau de fonctionnalité). N’incluez pas le nom du répertoire du niveau de fonctionnalité ni le suffixe. Par exemple, Load « textures \\ CoolSign. DDS », et non pas « dxfl-DX11 \\ textures \\ CoolSign. DDS » ou « textures \\ CoolSign \_ dxfl-DX11. DDS ».

     

-   À présent, utilisez [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) pour localiser le fichier qui correspond au niveau de fonctionnalité actuel de DirectX. **ResourceManager** retourne [**ResourceMap**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap), que vous interrogez avec [**ResourceMap::GetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getvalue) (ou [**ResourceMap::TryGetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.trygetvalue)) et un [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext) fourni. Cela retourne un [**ResourceCandidate**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceCandidate) qui correspond le mieux au niveau de fonctionnalité DirectX spécifié en appelant [**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue).
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   Dans Visual Studio 2015, sélectionnez **PROJET-&gt;Windows Store-&gt;Créer un package d’application**, puis créez le package.
-   Veillez à activer les ensembles d’applications dans les paramètres du manifeste package.appxmanifest.

## <a name="related-topics"></a>Rubriques connexes


* [Définition des ressources d’application](/previous-versions/windows/apps/hh965321(v=win.10))
* [Empaquetage d’applications](../packaging/index.md)
* [Package d’application (MakeAppx.exe)](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)

 

 
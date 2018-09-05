---
author: stevewhims
description: C++ / WinRT fournit des fonctions et des classes de base qui vous faire gagner beaucoup de temps et d’efforts lorsque vous souhaitez implémenter ou passer des collections.
title: Collections avec C++ / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, standard, c++, cpp, winrt, projection, collection
ms.localizationpriority: medium
ms.openlocfilehash: 5495649a6b7fad633e24e244aa3f6efbcc05e441
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3379828"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Collections avec [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.**

En interne, une collection Windows Runtime a un grand nombre d’éléments mobiles complexes. Toutefois, lorsque vous voulez passer un objet de collection à une fonction Windows Runtime, ou pour implémenter vos propres propriétés de collection et les types de collection, il existe des fonctions et des classes de base en C++ / WinRT pour prendre en charge vous. Ces fonctionnalités prennent la complexité en dehors de vos mains et vous faire gagner un grand nombre de la surcharge de temps et d’efforts.

> [!IMPORTANT]
> Les fonctionnalités décrites dans cette rubrique sont disponibles si vous avez installé la [Windows 10 SDK version d’évaluation 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)ou ultérieure.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) est l’interface de Windows Runtime implémentée par n’importe quelle collection à accès aléatoire des éléments. Si vous devez implémenter **IVector** vous-même, vous devrez également implémenter [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)et [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Même si vous *avez besoin* une collection personnalisée de type, qui est un grand nombre de travail. Toutefois, si vous avez des données dans un **std::vector** (ou un **std::map**ou un **std::unordered_map**) et tout ce que vous voulez effectuer est la transmettre à une API Windows Runtime, puis vous souhaiteriez éviter d’exécuter ce niveau de travail, si possible. Et en évitant qu’il *est* possible, étant donné que C++ / WinRT vous aide à créer des collections efficacement et avec peu d’effort.

## <a name="helper-functions-for-collections"></a>Fonctions d’assistance pour les collections

### <a name="general-purpose-collection-empty"></a>Collection à usage général, vide

Pour récupérer un nouvel objet d’un type qui implémente une collection à usage général, vous pouvez appeler le modèle de fonction [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . L’objet est retourné comme une [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), et c’est l’interface par l’intermédiaire de laquelle vous appelez des fonctions et propriétés de l’objet renvoyé.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

Comme vous pouvez le voir dans l’exemple de code ci-dessus, après avoir créé la collection vous pouvez ajouter des éléments, itérer au sein de leur et traitent généralement l’objet comme vous le feriez pour n’importe quel objet de collection Windows Runtime que vous avez peut-être reçu à partir d’une API. Si vous avez besoin d’une vue immuable au-dessus de la collection, vous pouvez appeler [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), comme indiqué. Le modèle présenté ci-dessus&mdash;de création et l’utilisation d’une collection&mdash;est appropriée pour les scénarios simples où vous souhaitez passer des données dans ou à recevoir des données en dehors d’une API.

### <a name="general-purpose-collection-primed-from-data"></a>Collection à usage général, amorcée à partir des données

Vous pouvez également éviter la surcharge liée à des appels à **Append** que vous pouvez voir dans l’exemple de code ci-dessus. Vous disposez peut-être déjà la source de données, ou vous pouvez préférer remplir avant la création de l’objet de collection Windows Runtime. Voici la procédure à suivre.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Vous pouvez transmettre un objet temporaire contenant vos données à **winrt::single_threaded_vector**, comme avec `coll1`ci-dessus. Ou vous pouvez déplacer un **std::vector** (en supposant que vous n’y accéder à nouveau) dans la fonction. Dans les deux cas, vous êtes en passant une *rvalue* dans la fonction. Cela permet le compilateur être efficace et éviter la copie des données. Si vous souhaitez en savoir plus sur *rvalues*, voir les [catégories de valeur et des références associées](cpp-value-categories.md).

Si vous souhaitez lier un contrôle d’éléments XAML à votre collection, vous pouvez ensuite. Mais n’oubliez pas que pour définir correctement la propriété [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , vous devez lui affecter une valeur de type **IVector** de **IInspectable** (ou d’un type d’interopérabilité tels que [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Voici un exemple de code qui produit une collection d’un type approprié pour la liaison et ajoute un élément à celui-ci.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

La collection ci-dessus *peut* être liée à un contrôle d’éléments XAML; Toutefois, la collection n’est pas observable.

### <a name="observable-collection"></a>Collection observable

Pour récupérer un nouvel objet d’un type qui implémente une collection *observable* , appelez le modèle de fonction [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) avec n’importe quel type d’élément. Toutefois, pour rendre une collection observable pouvant être lié à un contrôle d’éléments XAML, utilisez **IInspectable** comme type d’élément.

L’objet est retourné comme une [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), et c’est l’interface par l’intermédiaire de laquelle vous (ou le contrôle à laquelle elle est liée) appeler fonctions et propriétés de l’objet renvoyé.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Pour plus d’informations et des exemples de code, sur la liaison de votre utilisateur (UI) contrôles d’interface à une collection observable, voir [contrôles d’éléments XAML; liaison à C++ / WinRT collection](binding-collection.md).

### <a name="associative-collection-map"></a>Collection associatif (mapper)

Il existe des versions de collection associatifs des deux fonctions que nous avons examinées.

- Le modèle de fonction [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) renvoie une collection associatif non observables sous la forme d’une [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- Le modèle de fonction [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) renvoie une collection observable associatif comme un [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Vous pouvez éventuellement prime ces collections avec des données en transmettant à la fonction une *rvalue* de type **std::map** ou **std::unordered_map**.

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>Thread unique

Le «single-threaded» dans les noms de ces fonctions indique qu’ils ne fournissent pas n’importe quel concurrency&mdash;en d’autres termes, ils ne sont pas thread-safe. La mention de threads n’est pas liée à compartiments, dans la mesure où les objets renvoyées à partir de ces fonctions sont tous les agiles (voir [des objets agiles en C++ / WinRT](agile-objects.md)). Il est simplement que les objets sont à thread unique. Et c’est tout à fait approprié si vous souhaitez simplement transmettre des données d’une façon ou l’autre sur l’interface binaire d’application (ABI).

## <a name="base-classes-for-collections"></a>Classes de base pour les collections

Si, pour une souplesse totale, que vous souhaitez implémenter votre propre collection personnalisée, vous devrez éviter d’exécuter qui dépens. Par exemple, voici comment se présenterait une vue personnalisée vecteur *sans l’aide de C++ / classes de base de WinRT*.

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

Au lieu de cela, il est beaucoup plus facile de dériver de votre affichage vectoriel personnalisée à partir du modèle de structure [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) et simplement implémenter la fonction **get_container** pour exposer le conteneur contenant vos données.

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

Le conteneur renvoyé par **get_container** doit fournir à l’interface de **début** et **fin** cette **winrt::vector_view_base** attend. Comme indiqué dans l’exemple ci-dessus, **std::vector** prévoit que. Toutefois, vous pouvez revenir à n’importe quel conteneur qui remplit le même contrat, y compris votre propre conteneur personnalisé.

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

Il s’agit de la base de classes that + C++ / WinRT fournit pour vous aider à implémenter des collections personnalisées.

### [<a name="winrtvectorviewbase"></a>WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Consultez les exemples de code ci-dessus.

### [<a name="winrtvectorbase"></a>WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### [<a name="winrtobservablevectorbase"></a>WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### [<a name="winrtmapviewbase"></a>WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### [<a name="winrtmapbase"></a>WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### [<a name="winrtobservablemapbase"></a>WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>API importantes
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [WinRT::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [WinRT::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [WinRT::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [WinRT::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [WinRT::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [WinRT::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [WinRT::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [WinRT::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [WinRT::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [WinRT::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Rubriques associées
* [Les catégories de valeur et des références associées](cpp-value-categories.md)
* [Contrôles d’éléments XAML; liaison à une collection C++/WinRT](binding-collection.md)
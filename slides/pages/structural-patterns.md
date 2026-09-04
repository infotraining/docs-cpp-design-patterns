---
layout: cover
background: /img/header-bg.svg
---

# Wzorce strukturalne

---
layout: center
---

<div class="no-bullets text" style="font-size: 1.5em;">

<v-clicks>

* Adapter
* Decorator
* Composite
* Proxy
* Facade
* Bridge
* Flyweight

</v-clicks>

</div>

---

# Wzorce strukturalne

* Wzorce strukturalne rozwiązują problem poprzez złożenie klas i obiektów w większe struktury.

---
layout: cover
background: /img/header-bg.svg 
---

# Adapter

---

# Adapter - Przeznaczenie

* Dokonuje konwersji interfejsu danej klasy do postaci zgodnej z oczekiwaniami klienta
* Pozwala na wzajemną współpracę klas, które ze względu na niekompatybilne interfejsy wcześniej nie mogły ze sobą współpracować

---

# Adapter - Kontekst / Problem

* Kontekst
    - Interfejs wymagany przez klienta i interfejs klasy dostarczającej implementację nie są ze sobą zgodne

* Problem
    - Chcemy wykorzystać istniejącą klasę, a jej interfejs nie odpowiada temu, którego potrzebujemy

---
class: white-slide
layout: center
---

# Adapter klas

<img src="/img/gof/Adapter-Class.excalidraw.svg" alt="Adapter klas" class="width-60 center" />

---
class: white-slide
layout: center
---

# Adapter obiektów

<div class="span-v-4"/>

<img src="/img/gof/Adapter-Object.excalidraw.svg" alt="Adapter obiektów" class="width-70 center" />

---
layout: two-cols
---

::left::

## Adapter klas

* Adaptuje dostosowując się do klasy konkretnej ``Adaptee`` – nie będzie więc działał wtedy, gdy będziemy chcieli zaadaptować klasę oraz jej wszystkie podklasy
* Umożliwia klasie ``Adapter`` przedefiniowanie części zachowania ``Adaptee``
* Wprowadza tylko jeden obiekt, aby dostać się do adaptowanego

::right::

## Adapter obiektów

* Umożliwia jednemu adapterowi działanie z wieloma adaptowanymi – z samą klasą ``Adaptee`` oraz jej wszystkimi podklasami
* Utrudnia przedefiniowanie zachowania adaptowanego – wymaga w tym celu tworzenia podklas adaptowanego i odwoływania się ``Adaptera`` do nich, a nie do samego adaptowanego (``Adaptee``)

---

# Adapter - Podsumowanie

* Zmienia interfejs istniejącego obiektu i dostosowuje go do oczekiwań klienta
* Klasy nie związane ze sobą mogą współpracować pomimo niezgodnych interfejsów

---
layout: cover
background: /img/header-bg.svg
---

# Decorator

---

# Decorator - Przeznaczenie

* Pozwala na dynamiczne przydzielanie danemu obiektowi nowych zachowań
* Zapewnia elastyczną alternatywę dla tworzenia podklas w celu rozszerzania funkcjonalności

---

# Decorator - Scenariusz

Chcemy rozszerzyć możliwości obiektu Photo i dodać do zdjęcia ramkę oraz dwa tagi

---
class: white-slide
layout: center
---

<img src="/img/dp/ImageDecorator-1.svg" alt="coffee" class="width-60 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/ImageDecorator-2.svg" alt="coffee-decorated" class="width-60 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/ImageDecorator-3.svg" alt="coffee-decorated" class="width-60 center"/>


---
layout: center
---

<div class="text-2">

Dwie możliwe implementacje

</div>

---
layout: center
---

# Rozwiązanie 1 - Dziedziczenie

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-inheritance-1.png" alt="Decorator - Inheritance 1" class="width-80 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-inheritance-2.png" alt="Decorator - Inheritance 2" class="width-80 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-inheritance-3.png" alt="Decorator - Inheritance 3" class="width-80 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-inheritance-4.png" alt="Decorator - Inheritance 4" class="width-80 center"/>

---
layout: center
---


# Rozwiązanie 2 - Dekoracja komponentu

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-1.png" alt="Decorator 1" class="width-80 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-2.png" alt="Decorator 2" class="width-80 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/decorator/decorator-3.png" alt="Decorator 3" class="width-80 center"/>

---

# Dekoracja komponentu

* Umieszczenie komponentu w innym obiekcie, który dodaje ramkę.
* Obiekt będący otoczką komponentu nazywa się **dekoratorem**
* Dekorator dostosowuje się do interfejsu ozdabianego obiektu, dzięki czemu staje się przezroczysty dla klientów
* Dekorator przekazuje żądania do komponentu i może wykonywać dodatkowe akcje

---
class: white-slide
---

# Decorator - Współpraca

* Wzorzec dekoratora umożliwia składanie obiektu `Photo` z dekoratorami `BorderedPhoto` i `TaggedPhoto`

<div class="span-v-4"/>

<img src="/img/dp/decorator_3.png" alt="Decorator - Współpraca" class="width-60 center"/>

---

# Decorator - Kontekst

* Dziedziczenie jest jedną z form rozszerzenia funkcjonalności klasy, ale niekoniecznie musi być najlepszym sposobem na osiągnięcie w pełni elastycznych projektów aplikacji
* Tworząc projekt aplikacji, należy go tak skonstruować, aby możliwe było rozszerzanie zachowań poszczególnych elementów bez konieczności modyfikowania istniejącego kodu
* Wykorzystując kompozycję oraz delegację, można dodawać nowe zachowania podczas działania programu
* Wzorzec Decorator posługuje się zbiorem klas dekorujących (dekoratorów), które są wykorzystywane do dekorowania poszczególnych obiektów (komponentów)

---

# Decorator - Stosowalność

* Wzorzec Decorator powinien być stosowany:
    - aby dynamicznie i w przezroczysty sposób (tzn. nie wpływający na inne obiekty) dodać zobowiązania do pojedynczych obiektów
    - w wypadku zobowiązań, które mogą być cofnięte
    - gdy rozszerzanie funkcjonalności przez definiowanie podklas jest niepraktyczne
    - czasami jest możliwych wiele niezależnych rozszerzeń, które przy próbie uwzględnienia różnych ich kombinacji prowadzą do gwałtownego wzrostu liczby klas

---
class: white-slide
---

# Decorator - Struktura

<img src="/img/gof/Decorator.excalidraw.svg" alt="Decorator - Structure" class="width-70 center"/>

---
class: white-slide
---

# Decorator - Struktura

<div class="span-v-4"/>

<img src="/img/gof/Decorator-Objects.excalidraw.svg" alt="Decorator - Structure" class="width-70 center"/>
---

# Decorator - konsekwencje [1]

* Większa elastyczność niż przy stosowaniu statycznego dziedziczenia
    - mając dekoratory można dodawać i usuwać zobowiązania w czasie wykonywania programu
    - Uwzględnienie różnych klas Decorator dla określonej klasy komponentu umożliwia mieszanie i dopasowywanie zobowiązań
    - dekoratory ułatwiają także dwukrotne dołączanie właściwości (np. fotografia z podwójną ramką)

---

# Decorator - konsekwencje [2]

* Unikanie przeładowania właściwościami klas na szczycie hierarchii
    - możliwe jest zdefiniowanie prostej klasy i przyrostowe rozszerzanie jej funkcjonalności za pomocą obiektów dekoratora
    - nowe rodzaje dekoratorów są łatwe do zdefiniowania
* Dekorator i jego komponent nie są identyczne
    - dekorator działa jak przezroczysta otoczka, jednak z punktu widzenia identyczności obiektów udekorowany komponent nie jest taki sam jak ten wyjściowy

---

# Decorator - konsekwencje [3]

* Wiele małych obiektów
    - projekty wykorzystujące dekoratory prowadzą często do powstawanie systemów z dużą liczbą małych, podobnych do siebie obiektów

---

# Decorator - implementacja

* Zgodność interfejsów
    - interfejs obiektu będącego dekoratorem musi odpowiadać interfejsowi dekorowanego przez niego komponentu
    - klasy ConcreteDecorator muszą dziedziczyć po wspólnej klasie
* Pomijanie klasy abstrakcyjnej ``Decorator``
    - gdy zależy nam na dodaniu tylko jednego zobowiązania, nie musimy definiować klasy abstrakcyjnej Decorator
* Utrzymanie klas ``Component`` w wadze lekkiej
    - ekstrakcja interfejsu dla klasy komponentu

---

# Decorator - pokrewne wzorce

* Composite
    - wzorzec Decorator można uważać za zdegenerowany kompozyt, z jednym komponentem. Decorator jednak dodaje dodatkowe zobowiązania, nie jest przeznaczony do agregacji obiektów
* Strategy
    - wzorzec Decorator umożliwia zmianę skóry obiektu, a Strategy zmianę jego wnętrza
* Builder
    - ułatwia tworzenie łańcuchów dekoratorów

---

# Decorator - podsumowanie

* Umożliwia dynamiczne dodawanie zobowiązań do obiektu
* Posługuje się zbiorem klas dekorujących (dekoratorów), które są wykorzystywane do dekorowania poszczególnych obiektów (składników)
* Dekoratory mają ten sam interfejs, co obiekty dekorowane
* Dekoratory zmieniają zachowania obiektów dekorowanych (składników), dodając nowe zachowania przed wywołaniami metod danego składnika i (lub) po nich lub nawet pomiędzy nimi
* Każdy składnik może być "otoczony" dowolną ilością dekoratorów

---
layout: cover
background: /img/header-bg.svg
---

# Composite

---

# Composite - Przeznaczenie

* Składa obiekty w struktury drzewiaste reprezentujące hierarchie typu część-całość
* Umożliwia klientom jednakowe traktowanie pojedynczych obiektów i ich agregatów

---

# Composite - Kontekst / Problem

* Kontekst
    - chcemy utworzyć reprezentację dla hierarchii obiektów
    - hierarchia obiektów ma wspólną klasę bazową (klasę abstrakcyjną lub interfejs)
* Problem
    - chcemy, aby klienci mogli ignorować różnicę między agregatami obiektów a pojedynczymi obiektami
    – klienci będą wtedy jednakowo traktować wszystkie obiekty występujące w strukturze

---
class: white-slide
layout: center
---

# Composite - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Composite_A.png" alt="Composite - Struktura" class="width-80 center"/>

---
class: white-slide
layout: center
---

# Composite - Struktura (Alternative Take)

<div class="span-v-4"/>

<img src="/img/dp/Composite_B.png" alt="Composite - Struktura (Alternative Take)" class="width-80 center"/>

---

# Composite - implementacja

<div class="text-code-09">

```cpp
class ShapeGroup : public Shape
{
    using ShapePtr = std::shared_ptr<Shape>;
    std::vector<ShapePtr> shapes_;
public:
    ShapeGroup() = default;

    void draw() const
    {
        for(const auto& shp : shapes_)
            shp->draw();
    }

    void add(ShapePtr shp)
    {
        shapes_.push_back(shp);
    }
};
```
</div>

---

# Composite - Współpraca

* Klienci używają interfejsu z klasy Component w celu komunikowania się z obiektami występującymi w składanej strukturze
    - jeśli odbiorca jest liściem, to żądania są realizowane bezpośrednio
    - jeśli odbiorca jest kompozytem (Composite), to zwykle przekazuje swoje żądania komponentom-dzieciom, wykonując ewentualnie przed i/ lub po przekazaniu dodatkowe operacje

---

# Composite - Konsekwencje

* Wzorzec Composite definiuje hierarchie klas grupujących obiekty pierwotne i agregaty
* Uproszczenie budowy klienta
    – klienci mogą jednakowo traktować struktury złożone i pojedyncze obiekty
* Umieszczenie operacji dodawania nowych komponentów do agregatów w klasie bazowej Component może naruszać zasadę podstawiania Liskov

---

# Composite - Implementacja (1)

* Jawne odwołania do rodziców
    - przechowywanie odwołań z komponentów-dzieci do ich rodziców może ułatwiać poruszanie się po strukturze kompozytu i zarządzanie nią
    - typowym miejscem do definiowania odwołania do rodzica jest klasa ``Component``
    - klasy ``Leaf`` i ``Composite`` mogą dziedziczyć to odwołanie i zarządzające nim operacje

---

# Composite - Implementacja (2)

* Iteracja po elementach agregatu
    - implementacja wzorca Iterator dla struktury kompozytowej
* Współdzielenie komponentów
    - bardzo często opłacalne jest współdzielenie komponentów, na przykład w celu ograniczenia wymagań pamięciowych
    - implementacja obiektów struktury jako ``const`` (*immutable*)

---

# Composite - Implementacja (3)

* Kto powinien usuwać komponenty?
    - czy obiekt kompozytu posiada prawo własności (*ownership*) do obiektów podrzędnych
* Jaka struktura danych jest najlepsza do przechowywania komponentów?
    - ``std::vector<T>``
    - ``std::list<T>``
    - ``std::unordered_map<K, T>``

---

# Composite - Pokrewne wzorce

* Wzorzec Composite może być użyty do reprezentacji grupy obiektów we wzorcach
    - Chain Of Responsibility
    - Visitor
* Flyweight
    - umożliwia implementację współdzielenia komponentów
* Iterator
    - umożliwia iterację po całej hierarchii obiektów

---
layout: cover
background: /img/header-bg.svg
---

# Proxy

---

# Proxy - Przeznaczenie

* Wzorzec Proxy zapewnia substytut lub reprezentanta innego obiektu w celu sterowania dostępem do niego

---

# Proxy - Kontekst / Problem

* Kontekst
    - tworzenie obiektów i ich inicjalizacja w trakcie działania programu jest kosztowne
    - potrzebna jest kontrola dostępu do obiektu
* Problem
    - optymalizacja kosztownych procesów lub kontrola dostępu powinna być przezroczysta dla klienta

---

# Proxy - Scenariusz

* Edytor dokumentów, który umożliwia osadzanie obiektów graficznych
    - otwieranie dokumentów powinno być szybkie
    - optymalizacja nie powinna mieć wpływu na części programu związane z wyświetlaniem czy formatowaniem
* Rozwiązanie
    - użycie innego obiektu, pełnomocnika rysunku, który będzie zastępował prawdziwy rysunek
    - obiekt proxy zachowuje się jak rysunek i zajmuje się jego utworzeniem, gdy będzie to konieczne (**lazy loading**)

---
class: white-slide
# layout: center
---

# Proxy - Scenariusz - UML

<img src="/img/gof/Proxy - DocumentEditor - 1.excalidraw.svg" alt="Proxy - Scenariusz - UML 1" class="width-90 center"/>

---
class: white-slide
# layout: center
---

# Proxy - Scenariusz - UML

<img src="/img/gof/Proxy - DocumentEditor - 2.excalidraw.svg" alt="Proxy - Scenariusz - UML 2" class="width-90 center"/>

---
class: white-slide
# layout: center
---

# Proxy - Scenariusz - UML

<img src="/img/dp/Proxy_Image_Sekwencja.png" alt="Proxy - Scenariusz - UML 3" class="width-40 center"/>

---

# Proxy - Stosowalność

* Proxy ma zastosowanie zawsze wtedy, gdy potrzeba bardziej uniwersalnego lub bardziej wyrafinowanego odwołania do obiektu

---

# Rodzaje proxy

* Rodzaje obiektów Proxy
    - remote proxy – jest lokalnym reprezentantem obiektu znajdującego się w innej przestrzeni adresowej (RPC)
    - virtual proxy – tworzy kosztowne obiekty na żądanie
    - protection proxy – kontroluje dostęp do oryginalnego obiektu
    - smart proxy – modyfikuje żądanie przed przesłaniem go do oryginalnego obiektu

---
class: white-slide
# layout: center
---

# Proxy - Struktura

<img src="/img/gof/Proxy.excalidraw.svg" alt="Proxy - Struktura" class="width-80 center"/>

---
class: white-slide
# layout: center
---

# Proxy - Współpraca

* Obiekt pełnomocnika (proxy), jeśli trzeba, przekazuje żądania do prawdziwego obiektu (subject)

<div class="span-v-4"/>

<img src="/img/gof/Proxy - Objects.excalidraw.svg" alt="Proxy - Współpraca" class="width-60 center"/>

---

# Proxy - Konsekwencje

* Wzorzec Proxy wprowadza dodatkowy poziom pośredniości przy dostępie do obiektu
    * Remote Proxy – może ukryć fakt, że obiekt znajduje się w innej przestrzeni adresowej
    * Virtual Proxy – może wykonywać optymalizacje, np. tworzenie obiektu na żądanie, kopiowanie-przy-zapisaniu
    * Protection Proxy i Smart Proxy – umożliwiają wykonywanie dodatkowych czynności porządkowych przy dostępie do obiektu

---

# Proxy - Pokrewne wzorce

* Decorator – pomimo podobnej implementacji, przeznaczenie wzorca Proxy jest inne:
    - Dekorator dodaje zobowiązania do obiektu
    - Proxy zarządza dostępem do obiektu

---

# Proxy - Podsumowanie

* Zapewnia obiekt pośrednika, dzięki któremu możemy optymalizować wywołanie kosztownych operacji lub kontrolować dostęp do oryginału
* Interfejs obiektu Proxy jest taki sam jak interfejs oryginału

---
layout: cover
background: /img/header-bg.svg
---

# Façade

---

# Façade

* Przeznaczenie
    - zapewnia jeden, zunifikowany interfejs dla całego zestawu interfejsów określonego podsystemu
    - tworzy nowy interfejs wysokiego poziomu, który powoduje, że korzystanie z całego podsystemu staje się zdecydowanie łatwiejsze
    - odseparowuje klienta od złożonych podsystemów

---
class: white-slide
---

# Façade

<div class="span-v-4"/>

<img src="/img/dp/Facade1.png" alt="Façade" class="width-80 center"/>

---
class: white-slide
---

# Façade - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Facade.png" alt="Façade - Struktura" class="width-50 center"/>

---

# Façade - Współpraca

* Klienci komunikują się z podsystemem, wysyłając żądania do fasady (Façade), która przekazuje je do odpowiednich obiektów podsystemu
* Klienci wykorzystujący fasadę nie muszą mieć bezpośredniego dostępu do obiektów z jej podsystemu

---

# Façade - Konsekwencje

* Oddziela klientów od komponentów podsystemu, dzięki czemu zmniejsza się liczba obiektów, z którymi klienci mają do czynienia – podsystem staje się łatwiejszy do użycia
* Sprzyja słabemu powiązaniu podsystemu z jego klientami – umożliwia zmianę komponentów w sposób niewidoczny dla jego klientów
* Ułatwia ułożenie warstwami systemu i zależności między obiektami
* Nie uniemożliwia aplikacjom bezpośredniego dostępu do podsystemu, jeśli go potrzebują

---

# Façade - Pokrewne wzorce

* Singleton
    - zwykle potrzeby jest jeden obiekt będący fasadą, dlatego fasady często są implementowane jako singletony
* Abstract Factory
    - wzorca Façade można użyć ze wzorcem AbstractFactory
    - zapewniamy w ten sposób interfejs do tworzenia określonej rodziny obiektów podsystemu

---

# Façade - Podsumowanie

* Udostępnia interfejs pozwalający ukryć przed klientem złożoność podsystemu
* Sprzyja słabemu powiązaniu klientów z podsystemem

---
layout: cover
background: /img/header-bg.svg
---

# Bridge

---

# Bridge

* Przeznaczenie
    - umożliwia różnicowanie implementacji i abstrakcji przez umieszczenie obu elementów w osobnych hierarchiach klas
    - bardziej elastyczny sposób separacji abstrakcji od implementacji niż stosowanie dziedziczenia

---

# Bridge - Kontekst

* Istnieje wiele implementacji, które muszą być uwzględnione w projekcie
* Klient korzysta z abstrakcyjnych klas w celu ujednolicenia interfejsu

---

# Bridge - Problem

* Chcemy uniknąć stałego powiązania abstrakcji z jej implementacją
    – implementacja może być wybierana lub zmieniana w czasie wykonywania programu
* Oczekujemy zmian zarówno w interfejsie abstrakcji jak i w implementacjach
    - zmiany w implementacji abstrakcji nie powinny mieć wpływu na klientów
* Chcemy całkowicie ukryć implementację abstrakcji przed klientami

---

# Bridge - Scenariusz

---
layout: center
---

<div class="text-2">

Projekt z użyciem dziedziczenia w celu uwzględnienia wielu implementacji

</div>

---
class: white-slide
layout: center
---

<img src="/img/gof/Bridge - Shapes - 1.excalidraw.svg" alt="Bridge - DrawingAPI Before" class="center"/>

---
class: white-slide
layout: center
---

<img src="/img/gof/Bridge - Shapes - 2.excalidraw.svg" alt="Bridge - DrawingAPI Before" class="center"/>

---
class: white-slide
layout: center
---

<img src="/img/gof/Bridge - Shapes - 3.excalidraw.svg" alt="Bridge - DrawingAPI Before" class="center"/>

---
layout: center
---

<div class="text-2">

Projekt z użyciem wzorca Bridge

</div>

---
class: white-slide
layout: center
---

<img src="/img/dp/Bridge_DrawingAPI_After.png" alt="Bridge - DrawingAPI After" class="width-70 center"/>

---
class: white-slide
---

# Bridge - Structure

<img src="/img/gof/Bridge.excalidraw.svg" alt="Bridge - Structure" class="width-90 center"/>

---

# Bridge - Konsekwencje

* Separuje implementację, usuwając jej trwałe powiązanie z interfejsem
    - implementacja abstrakcji może być ustalana w czasie wykonywania programu
    - eliminuje zależność w czasie kompilacji od określonej implementacji
* Ułatwiona rozszerzalność
    – można niezależnie rozbudowywać hierarchie Abstrakcji i Implementacji
    - zmiany w klasach konkretnych abstrakcji nie wpływają na klienta
* Przydatny w systemach graficznych i okienkowych, które muszą pracować na różnych platformach
* Zwiększa złożoność projektu

---

# Bridge - przypadek szczególny PIMPL

bitmap.hpp

```cpp
class Bitmap
{
    struct BitmapImpl;

    std::unique_ptr<BitmapImpl> impl_;
public:
    explicit Bitmap(const std::string& file_name);
    ~Bitmap();
    Bitmap(Bitmap&&) = default;
    Bitmap& operator=(Bitmap&&) = default;
    void draw() const;
};
```

---

bitmap.cpp

<div class="text-code-08">

```cpp
struct Bitmap::BitmapImpl
{
    std::vector<std::byte> bmp;
};

Bitmap::Bitmap(const std::string& file_name) : impl_{std::make_unique<BitmapImpl>()}
{
    impl_->bmp.reserve(1024);
    //...
}

Bitamp::~Bitmap() = default;

void Bitmap::draw() const
{
    for(const auto& pixel : impl_->bmp)
    {
        //...
    }
}
```

</div>

---
layout: cover
background: /img/header-bg.svg
---

# Flyweight

---

# Flyweight - Scenariusz

---
class: white-slide
---

<img src="/img/dp/flyweight-map-1.svg" alt="Flyweight - Map 1" class="width-80 center"/>

---
class: white-slide
---

<img src="/img/dp/flyweight-map-2.svg" alt="Flyweight - Map 2" class="width-80 center"/>

---
class: white-slide
---

<img src="/img/dp/flyweight-map-3a.svg" alt="Flyweight - Map 3a" class="width-80 center"/>

---
class: white-slide
---


<img src="/img/dp/flyweight-map-3b.svg" alt="Flyweight - Map 3b" class="width-80 center"/>

---
class: white-slide
---

<img src="/img/dp/flyweight-map-4.svg" alt="Flyweight - Map 4" class="width-80 center"/>

---
class: white-slide
---

<img src="/img/dp/flyweight-map-5.svg" alt="Flyweight - Map 5" class="width-80 center"/>

---
class: white-slide
---

<img src="/img/dp/flyweight-map-6.svg" alt="Flyweight - Map 6" class="width-80 center"/>

---

# Flyweight - Kontekst / Problem

* Kontekst
    - w projekcie istnieje olbrzymia liczba obiektów
    - koszt związany z przechowywaniem tych obiektów w pamięci jest znaczący

* Problem
    - chcemy ograniczyć koszty, związane z przechowywaniem obiektów w pamięci  współdzieląc obiekty w postaci pyłków

---

# Obiekt flyweight

* Obiekt Flyweight (Pyłek)
    - współdzielony obiekt, który może być używany jednocześnie w wielu kontekstach
    - działa jako obiekt niezależny w każdym kontekście dzięki rozróżnieniu stanu na **wewnętrzny** i **zewnętrzny**

---

# Obiekt flyweight

* **Stan wewnętrzny**
    – jest przechowywany w pyłku, składa się z informacji, które są niezależne od kontekstu
* **Stan zewnętrzny**
    – zależy od kontekstu i zmienia się w zależności od niego, nie może być współdzielony
* Pyłki modelują pojęcia lub byty, które zwykle są zbyt liczne, żeby przedstawiać je za pomocą w pełni samodzielnych obiektów

---

# Flyweight - wpółdzielenie obiektów

* Po usunięciu stanu zewnętrznego wiele grup obiektów można zastąpić stosunkowo niewielką liczbą współdzielonych obiektów

---
class: white-slide
---

# Flyweight - Struktura

<img src="/img/gof/Flyweight.excalidraw.svg" alt="Flyweight - Struktura" class="width-80 center"/>

---

# Flyweight - Konsekwencje

* Zmniejszenie zużycia pamięci kosztem wydłużenia czasu wykonywania
* Oszczędności pamięci zależą od kilku czynników:
    - zmniejszenia łącznej liczby egzemplarzy, wynikającego ze współdzielenia
    - wielkości stanu wewnętrznego przypadającego na obiekt
    - tego, czy stan zewnętrzny jest wyliczany, czy przechowywany

---

# Flyweight - Podsumowanie

* Wykorzystuje współdzielenie obiektów w celu efektywnej obsługi wielkiej liczby obiektów
* Zmniejsza zużycie pamięci kosztem wydłużenia czasu wykonywania
* Skuteczność pyłku zależy od tego, ile informacji uda się współdzielić jako stan wewnętrzny pyłku

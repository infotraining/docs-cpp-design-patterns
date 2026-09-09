---
layout: cover
background: /img/header-bg.svg
---

# Wzorce kreacyjne

---
layout: center
---

<div class="no-bullets text-2">

<v-clicks>

* Factory Method
* Abstract Factory
* Prototype
* Builder 
* Singleton

</v-clicks>

</div>

---

# Wzorce kreacyjne - wprowadzenie 

* Prowadzą do uabstrakcyjnienia procesu tworzenia obiektów
* Ułatwiają budowę systemu niezależnego od sposobu tworzenia, składania i reprezentowania stosowanych w nim obiektów
* Hermetyzują wiedzę o tym, jakich klas konkretnych używa system i w jaki sposób egzemplarze tych klas są tworzone, konfigurowane i łączone ze sobą

---

# Wzorce kreacyjne - wprowadzenie

* Umożliwiają konfigurowanie systemu z obiektami-produktami, które w znacznym stopniu różnią się strukturą i funkcjonalnością
    - Konfiguracja statyczna (podczas kompilacji)
    - Konfiguracja dynamiczna (w trakcie wykonywania programu)

---
layout: center
---

# Fabryki

<div class="text-xl">

> Preferuj luźne powiązania między klasami
>
> *— Zasada dobrego projektowania OOP*

</div>

---

Kod łamiący tą zasadę:

<div class="text-code-07">

```cpp
class MusicApp
{
public:
    //...

    void play(const std::string& track_title)
    {
        // creation of the object
        SpotifyService music_service("spotify_user", "rjdaslf276%2", 45);

        // usage of the object
        std::optional<Track> track = music_service.get_track(track_title);

    }

    //...
};
```
</div>


<div class="text-09">
<v-clicks>

* Klasy są silnie ze sobą związane (**strong coupling**)
* Klasa ``MusicApp`` jest trudna do przetestowania jednostkowego

</v-clicks>
</div>


---

# Fabryki

* Chcąc utworzyć obiekt, musimy dokładnie wiedzieć jaki jest jego typ
* Jednak czasami:
  - Chcemy tę dokładną wiedzę pozostawić w gestii kogoś innego
  - Dysponujemy informacją o typie obiektu, ale w postaci identyfikatora np. typu `std::string`
  - O typie tworzonego obiektu decyduje typ innego obiektu

---
layout: cover
background: /img/header-bg.svg 
---
# Factory Method

---

# Factory Method

* Przeznaczenie
  - definiuje interfejs pozwalający na tworzenie obiektów, ale odpowiedzialność za tworzenie obiektów jest delegowana do klas pochodnych
  - Wykorzystuje mechanizm dziedziczenia pozwala klasom pochodnym decydować, jakiej klasy obiekt zostanie utworzony

---

# Factory Method - Scenariusz

Jak poprawić poniższy kod:

<div class="text-code-08">

```cpp
class MusicApp
{
public:
    //...

    void play(const std::string& track_title)
    {
        // creation of the object
        SpotifyService music_service("spotify_user", "rjdaslf276%2", 45);

        // usage of the object
        std::optional<Track> track = music_service.get_track(track_title);

    }

    //...
};
```

</div>

---

## Krok #1

* Ekstrakcja interfejsu dla serwisu:

```cpp
class MusicService
{
public:
    virtual std::optional<Track> get_track(const std::string& title) = 0;
    virtual ~MusicService() = default;
};
```

---

## Krok #2

* Uabstrakcyjnienie procesu tworzenia instancji klasy ``MusicService``:

```cpp
class MusicServiceCreator
{
public:
    virtual std::unique_ptr<MusicService> create_music_service() = 0;
    virtual ~MusicServiceCreator() = default;
};
```

---

## Krok #3

* Wstrzyknięcie zależności od fabryki do klasy ``MusicApp``:

```cpp
class MusicApp
{
    std::shared_ptr<MusicServiceCreator> music_service_creator_;
public:
    MusicApp(std::shared_ptr<MusicServiceCreator> music_service_creator)
        : music_service_creator_(music_service_creator)
    {
    }

    //...
};
```

---

## Krok #4

* Oddelegowanie procesu kreacji do obiektu fabryki:

```cpp
void play(const std::string& track_title)
{
    // creation of the object
    std::unique_ptr<MusicService> music_service =
        music_service_creator_->create_music_service();

    // usage of the object
    std::optional<Track> track = music_service->get_track(track_title);

    //...
}
```

---

# Factory Method - Kontekst

* Chcemy wprowadzić nową funkcjonalność poprzez napisanie nowej klasy i utworzenie instancji tej klasy

---

# Factory Method - Problem

* Chcemy tworzyć instancje konkretnych klas za pomocą interfejsu
* Dana klasa nie może przewidzieć typu obiektu, który ma być utworzony
* Informacja o typie obiektu, który ma być utworzony jest dostępna dopiero w run-time'ie

---
class: white-slide
layout: center
---

# Factory Method - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Factory.png" alt="Factory Method" class="width-80 center"/>


---

# Factory Method - Współpraca

* Obiekt klasy ``Creator`` deleguje do swoich klas pochodnych odpowiedzialność za takie zdefiniowanie metody wytwórczej, by towrzyła egzemplarz odpowiedniej klasy ``ConcreteProduct`` (podklasy klasy abstrakcyjnej ``Product``)

---

# Factory Method - Konsekwencje

* Eliminuje potrzebę wstawiania klas konkretnych w kod aplikacji.
* Tworzenie obiektów wewnątrz klasy za pomocą metody wytwórczej jest bardziej elastyczne niż tworzenie ich bezpośrednio.
* Promuje luźne powiązania między obiektami, ponieważ redukuje zależność  kodu aplikacji od konkretnych klas.

---
class: white-slide
layout: center
---

## Równoległe hierarchie klas

<div class="span-v-4"/>

<img src="/img/dp/Factory-ShapeManipulator.png" alt="Factory Method - Shape Manipulator" class="width-80 center"/>

---

## Równoległe hierarchie klas

* Równoległe hierarchie klas powstają wtedy, gdy klasa przekazuje niektóre ze swych zobowiązań odrębnej klasie
* Wzorzec Factory Method umożliwia łączenie równoległych hierarchii klas

```cpp
std::shared_ptr<Shape> selected_shape = get_clicked_shape();

std::shared_ptr<Manipulator> manipulator = selected_shape->create_manipulator();

manipulator->on_drag(100, 200);
```

---

# Factory Method - Implementacja

* Implementacja klasy fabryki jako:
   - klasa abstrakcyjna (interfejs), która nie dostarcza implementacji dla metod, które deklaruje
   - klasa konkretna, która dostarcza domyślną implementację metody wytwórczej
* W prostym przypadku interfejs fabryki można zastąpić typem

```cpp
using MusicServiceCreator = std::function<std::unique_ptr<MusicService>()>;
```

---

# Sparametryzowane fabryki

* Metoda wytwórcza przyjmuje parametr, który identyfikuje typ tworzonego obiektu
* W rezultacie pojedyncza instancja fabryki może tworzyć obiekty różnych typów

---

# Sparametryzowane fabryki

```cpp
using MusicServiceCreator = std::function<std::unique_ptr<MusicService>()>;

class MusicServiceFactory
{
    std::unordered_map<std::string, MusicServiceCreator> creators_;
public:
    std::unique_ptr<MusicService> create(const std::string& id) const
    {
        auto& creator = creators_.at(id);
        return creator();
    }

    void register_creator(const std::string& id, const MusicServiceCreator& creator);
};
```

---

# Sparametryzowane fabryki

* Użycie fabryki:

```cpp
MusicServiceFactory music_service_factory;

music_service_factory.register_creator("Tidal",
    [] { return std::make_unique<TidalService>("tidal_user", "KJH8324d&df"); });

//...

std::string id_from_config = "Tidal";
MusicApp app(music_service_factory.at(id_from_config));
```

---

# Factory Method - Pokrewne wzorce

* Iterator
* Abstract Factory
    – fabrykę abstrakcyjną często implementuje się za pomocą metod wytwórczych

---

# Factory Method - Podsumowanie

* Umożliwia inicjowanie przez jeden obiekt procesu tworzenia innego obiektu w sytuacji, gdy nie jest znana klasa tworzonego obiektu
* Kod klienta jest nakierowany na interfejsy
* Umożliwia łączenie równoległych hierarchii klas

---
layout: cover
background: /img/header-bg.svg 
---

# Abstract Factory

---

# Abstract Factory

* Przeznaczenie
    - dostarcza interfejs do tworzenia całych rodzin spokrewnionych lub zależnych od siebie obiektów bez konieczności określania ich klas rzeczywistych

---

# Abstract Factory - Scenariusz

* Chcemy napisać aplikację współpracującą z wieloma RDBMSami (np. Oracle, SQL Server, itp.)
* Definiujemy podstawowe klasy abstrakcyjne, które współpracują ze sobą:
    - ``Connection`` – obiekt kontrolujący połączenie z bazą danych
    - ``Command`` – obiekt reprezentujący polecenie SQL
    - ``Transaction`` – obiekt reprezentujący transakcję

---

# Abstract Factory - Scenariusz

* Chcemy uniknąć sztywnego tworzenia konkretnych klas w aplikacji i jednocześnie chcemy zachować spójność w ramach rodziny obiektów

---
class: white-slide
layout: center
---

<img src="/img/dp/abstract-factory-db-1.png" alt="Abstract Factory - Database - 1" class="width-40 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/abstract-factory-db-2.png" alt="Abstract Factory - Database - 2" class="width-70 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/abstract-factory-db-3.png" alt="Abstract Factory - Database - 3" class="width-70 center"/>

---
class: white-slide
layout: center
---

<img src="/img/dp/abstract-factory-db-4.png" alt="Abstract Factory - Database - 4" class="width-70 center"/>

---

# Abstract Factory - Kontekst
* W systemie istnieją rodziny powiązanych ze sobą obiektów-produktów, zaprojektowane tak, by obiekty były używane razem i ograniczenie to powinno być zachowane
* System powinien być niezależny od tego, jak jego produkty są tworzone, składowane i reprezentowane

---

# Abstract Factory - Problem

* Chcemy umożliwić łatwą konfigurację systemu przy użyciu jednej z wielu rodzin produktów
* Kod powinien być zależny od interfejsów lub klas abstrakcyjnych
* Chcemy dostarczyć bibliotekę klas produktów, ujawniając tylko ich interfejsy, a nie implementacje

---
class: white-slide
layout: center
---

## Abstract Factory - Struktura

<div class="span-v-2"/>

<img src="/img/dp/Abstract.png" alt="Abstract Factory" class="width-50 center"/>

---

# Abstract Factory - Implementacja

* Najczęstsza implementacja

```cpp
class AbstractFactory
{
public:
    virtual ~AbstractFactory() = default;f
    virtual std::unique_ptr<ProductA> create_product_a() = 0;
    virtual std::unique_ptr<ProductB> create_product_b(Param param) = 0;
    virtual std::unique_ptr<ProductC> create_product_c() = 0;
};
```

---

# Abstract Factory - Konsekwencje

* Odseparowanie klas konkretnych
* Łatwiejsza wymiana rodzin produktów
    - najczęściej klasa konkretnej fabryki pojawia się w aplikacji tylko raz
    - umożliwia to łatwą zmianę fabryki konkretnej używanej przez aplikację, a tym samym wymianę rodziny produktów
* Spójność produktów
    - współpraca produktów wymaga, by aplikacja używała za jednym razem obiektów tylko z danej rodziny
* Utrudnione dołączenie nowych produktów

---

# Abstract Factory - Pokrewne wzorce

* Factory Method
    - klasy AbstractFactory są definiowane z wykorzystaniem metod wytwórczych
* Singleton
    - konkretna instancja fabryki może być implementowana jako singleton

---

# Abstract Factory - Podsumowanie

* Zapewnia interfejs umożliwiający tworzenie rodzin powiązanych ze sobą lub zależnych od siebie obiektów bez specyfikowania ich klas konkretnych
* Rodziny produktów mogą być łatwo wymieniane bez ingerencji w kod klienta
* Spełnia regułę odwracania zależności (DIP – Dependency Inversion Principle)
* Najczęstsza implementacja wzorca: interfejs fabryki abstrakcyjnej zdefiniowany  jako kolekcja metod wytwórczych

---

# Fabryki - podsumowanie

* Uabstrakcyjniają proces tworzenia obiektów
* Potężne techniki programistyczne pozwalające na tworzenie kodu, który będzie uzależniony od abstrakcji
* Promują tworzenie luźnych zależności poprzez redukcję zależności kodu aplikacji od implementacji klas rzeczywistych

---
layout: cover
background: /img/header-bg.svg 
---

# Singleton

---

# Singleton

* Przeznaczenie
    - Gwarantuje, że dana klasa będzie miała tylko i wyłącznie jedną instancję obiektu i zapewnia globalny punkt dostępu do tej instancji
    - Wszystkie obiekty korzystające z danej klasy używają tego samego egzemplarza

---

# Singleton - Kontekst / Problem

* Kontekst
    - w systemie istnieją obiekty, które z różnych powodów powinny występować tylko w jednym egzemplarzu
* Problem
    - chcemy utworzyć klasę, która może mieć tylko jeden egzemplarz instancji, dostępny dla klientów

---

# Singleton - Implementacja

* Zagwarantowanie unikatowego egzemplarza
    - prywatny konstruktor oraz usunięte z interfejsu operacje kopiowania
    - prywatna statyczna właściwość inicjalizowana jedyną instancją klasy
    - publiczna statyczna metoda oferująca dostęp do jedynego egzemplarza obiektu klasy Singleton
* Leniwa inicjalizacja instancji klasy
* Singleton wielowątkowy
    – obsługa jednoczesnych wywołań metody instance

---

# Singleton - Implementacja C++

```cpp
class Singleton
{
    Singleton() = default;
    ~Singleton() = default;
public:
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    static Singleton& instance()
    {
        static Singleton unique_instance;
        return unique_instance;
    }

    void do_something();
};
```

---

## Implementacja - użycie singletona

```cpp
Singleton::instance().do_something();

Singleton& single_object = Singleton::instance();
Single_object.do_something();
```

---

# Generic Singleton

```cpp
template <typename T>
class SingletonHolder
{
    SingletonHolder() = default;
public:
    SingletonHolder(const SingletonHolder&) = delete;
    SingletonHolder& operator=(const SingletonHolder&) = delete;

    static T& instance()
    {
        static T unique_instance;

        return unique_instance;
    }
};
```
---

## Implementacja - użycie singletona w wersji generycznej

```cpp
class Logger
{
public:
    void log(const string&);
    //... rest of impl
};

using SingletonLogger = SingletonHolder<Logger>;

int main()
{
    SingletonLogger::instance().log("Start - main");
    //...
    SingletonLogger::instance().log("End - main");
}

```

---

# Singleton - Podsumowanie

* Singleton gwarantuje, że zostanie utworzony tylko jeden egzemplarz danej klasy
* Wszystkie obiekty korzystające z danej klasy używają tego samego egzemplarza

---
layout: cover
background: /img/header-bg.svg 
---

# Prototype

---

# Prototype

* Przeznaczenie
    - określa rodzaj tworzonych obiektów, używając prototypowego egzemplarza
    - tworzy nowe obiekty, kopiując ten prototyp
    - zapewnia klientowi możliwość generowania obiektów, których typ nie jest klientowi znany

---

# Prototype - Scenariusz

* Chcemy napisać aplikację graficzną umożliwiającą edycję rysunków
* Rysunki są reprezentowane w postaci agregatów typu Shape
* Klasa abstrakcyjna Shape definiuje podstawowe operacje wykonywane przez klienta – kod klienta zależy od tej abstrakcji
* Chcemy dodać możliwość kopiowania rysunków bez odwoływania się do klas konkretnych reprezentujących jego elementy składowe

---
class: white-slide
---

<img src="/img/dp/prototype-shapes-0.png" alt="Prototype - Shapes - 0" class="width-60 center"/>

---
class: white-slide
---

<img src="/img/dp/prototype-shapes-1.png" alt="Prototype - Shapes - 1" class="width-60 center"/>

---
class: white-slide
---

<img src="/img/dp/prototype-shapes-2.png" alt="Prototype - Shapes - 2" class="width-60 center"/>

---

## Kod

```cpp
auto selected_shape = get_selected_shape(x, y);

std::unique_ptr<Shape> shape_copy;

if (selected_shape)
{
    shape_copy = ...;
}
```

---

## Kod 

```cpp
std::unique_ptr<Shape> shape_copy;

if (auto selected_shape = get_selected_shape(x, y); selected_shape)
{
    shape_copy = selected_shape->clone();
}
```

---
class: white-slide
---

# Prototype - Struktura

<img src="/img/gof/Prototype.excalidraw.svg" alt="Prototype" class="width-70 center"/>

---

# Prototype w C++

Definiujemy abstrakcyjną metodę ``clone`` w klasie bazowej:

<div class="text-code-08">
```cpp
class Shape {
public:
    virtual std::unique_ptr<Shape> clone() const = 0;
    virtual ~Shape() = default;

    // rest of methods
};
```
</div>

Każda konkretna klasa pochodna **musi** zaimplementować operację ``clone``:

<div class="text-code-08">
```cpp
class Rectangle : public Shape {
public:
    std::unique_ptr<Shape> clone() const override
    {
        return std::make_unique<Rectangle>(*this);
    }
};
```
</div>

---

# Prototype - CRTP

Aby uniknąć duplikacji kodu możemy zastosować idiom CRTP:

<div class="text-code-08">
```cpp
template <typename ShapeType>
class CloneableShape : public Shape
{
public:
    std::unique_ptr<Shape> clone() const override
    {
        return std::make_unique<ShapeType>(
            static_cast<const ShapeType&>(*this));
    }
};
```
</div>

Klasa pochodna może użyć ``CloneableShape`` jako klasy bazowej:

<div class="text-code-08">
```cpp
class Rectangle : public CloneableShape<Rectangle> {
public:
    //...
};
```
</div>

---

# Prototype - konsekwencje

* Dynamiczne dodawanie i usuwanie produktów w czasie wykonywania programu
    - ułatwia włączanie do systemu nowych produktów konkretnych przez rejestrowanie prototypowych egzemplarzy u klienta
    - bardziej elastyczne rozwiązanie niż w przypadku fabryk
    - zredukowana liczba podklas
* Umożliwia specyfikowanie nowych prototypowych obiektów przez urozmaicanie struktury
    - złożone struktury definiowane w trakcie działania programu mogą być również klonowane

---

# Prototype - Pokrewne wzorce

* Abstract Factory
* Composite, Decorator, Command
    - wzorce te są często używane wraz ze wzorcem Prototype

---

# Prototype - Podsumowanie

* Wzorzec umożliwia tworzenie nowych obiektów poprzez kopiowanie prototypowego egzemplarza
* Klasy, których egzemplarze są klonowane, mogą być specyfikowane w trakcie wykonania programu
* Pozwala uprościć hierarchię klas fabryk, która jest porównywalna z hierarchią klas produktów

---
layout: cover
background: /img/header-bg.svg 
---

# Builder

---

# Builder - Przeznaczenie

* Oddziela konstrukcję złożonych obiektów od ich reprezentacji, umożliwiając tym samym powstawanie w jednym procesie konstrukcyjnym różnych reprezentacji
* Definiuje etapy tworzenia obiektu-produktu
  - etapy te są konfigurowalne z zewnątrz (to odróżnia go od fabryk obiektów)

---

# Builder - Kontekst

* Algorytm konstrukcji obiektu jest wieloetapowy
* Proces konstrukcji złożonego obiektu prowadzi do utworzenia różnych reprezentacji obiektu

---

# Builder - Problem

* Chcemy
  - upublicznić operacje niezbędne do utworzenia złożonego obiektu
  - ukryć wewnętrzną reprezentację produktu przed klientem
* Chcemy mieć możliwość modyfikacji poszczególnych kroków algorytmu

---
class: white-slide
layout: center
---

# Builder - Struktura

<img src="/img/gof/Builder.excalidraw.svg" alt="Builder" class="width-70 center"/>

---
class: white-slide
layout: center
---

<h2 style="font-size: 1.1em; margin-bottom: 0.3em !important;">Builder - Współpraca</h2>

<img src="/img/gof/Builder-Sequence.excalidraw.svg" alt="Builder - Współpraca" class="width-40 center"/>

---

# Builder - Konsekwencje

* Umożliwia zmiany wewnętrznej reprezentacji produktu
* Oddziela kod służący do konstruowania od reprezentacji
* Ulepsza kontrolę procesu konstruowania
    - w odróżnieniu od innych wzorców kreacyjnych, we wzorcu Builder obiekty konstruowane są krok po kroku pod nadzorem obiektu kierownika (Director)
* Proces konstrukcji może trwać w czasie !!!
    - Dopiero po ukończeniu produktu kierownik (Director) odbiera go od budowniczego

---

# Builder - Implementacja

* Interfejs montowania i konstruowania produktów
    - Interfejs Builder musi być na tyle ogólny, żeby było możliwe konstruowanie produktów przez wszystkich budowniczych konkretnych
* Brak klasy abstrakcyjnej produktów – nie jest wymagana

---

# Builder - Pokrewne wzorce

* Abstract Factory
    - podobnie jak Builder może konstruować obiekty złożone
    - różnica – wzorzec Builder kładzie nacisk na tworzenie produktów krok po kroku a Abstract Factory kładzie nacisk na rodziny produktów
* Decorator, Composite
    - Builder jest często używany do budowy łańcuchów obiektów

---

# Builder - Podsumowanie

* Oddziela konstrukcję złożonych obiektów od ich reprezentacji
* Ten sam proces konstrukcyjny może prowadzić do powstania obiektów o różnej reprezentacji
* Jest często używany do budowania obiektów kompozytowych (wzorzec Composite)

---

# Creational Patterns - Podsumowanie

* Factory Method
* Abstract Factory
* Singleton
* Prototype
* Builder
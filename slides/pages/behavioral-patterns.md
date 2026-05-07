---
layout: cover
background: /img/header-bg.svg
---

# Wzorce behawioralne

---
layout: center
---

<div class="no-bullets" style="font-size: 1.3em;">

<v-clicks>

* Template Method
* Strategy
* State
* Chain Of Responsibility
* Observer
* Command
* Memento
* Mediator
* Visitor

</v-clicks>

</div>


---

# Wzorce behawioralne

* Dotyczą algorytmów i przydzielania zobowiązań obiektom
* Charakteryzują złożone przepływy sterowania między obiektami, które są trudne do prześledzenia w czasie wykonywania programu
* Są wykorzystywane do organizowania, zarządzania i łączenia zachowań

---
layout: cover
background: /img/header-bg.svg 
---

# Template Method

---

# Template Method

* Przeznaczenie
    - definiuje szkielet algorytmu, odkładając implementację niektórych kroków algorytmu do klas pochodnych
    - metoda szablonowa ustala kolejność kroków w algorytmie, ale umożliwia klasom pochodnym zmianę implementacji tych kroków zależnie od ich potrzeb

---

# Template Method - Kontekst / Problem

* Kontekst
    - istnieje algorytm wymagający zmiany implementacji poszczególnych kroków
* Problem
    - chcemy jednorazowo zaimplementować stałą część algorytmu i pozostawić klasom pochodnym zaimplementowanie zachowania, które może się zmieniać

---
class: white-slide
layout: center
---

# Template Method - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Template.png" alt="Template Method" class="width-50 center">

---

# Template Method - Konsekwencje

* Użycie metod szablonowych jest podstawową techniką stosowaną w celu zagwarantowania możliwości ponownego wykorzystania kodu
* Metody szablonowe prowadzą do odwróconej struktury sterowania – klasa bazowa wywołuje operacje klasy pochodnej, a nie odwrotnie

---

# Template Method - Konsekwencje

Metody szablonowe wywołują:

- operacje konkretne z ``ConcreteClass``, ``AbstractClass`` lub z klas klienta
- operacje abstrakcyjne
- metody wytwórcze
- **hook methods**, zapewniające zachowanie domyślne, które może być rozszerzane przez klasy pochodne

---

# Template Method - Pokrewne wzorce

* Strategy
    - zarówno wzorzec Strategy jak i Template Method dokonują hermetyzacji algorytmów odpowiednio wykorzystując kompozycję i dziedziczenie
* Factory Method
    - Template Method często wywołuje metody wytwórcze

---

# Template Method

* Definiuje szkielet danego algorytmu w określonej metodzie, przekazując realizację niektórych kroków algorytmu do klas podrzędnych
* Pozwala klasom podrzędnym na redefiniowanie pewnych kroków algorytmu, ale jednocześnie uniemożliwia zmianę jego struktury
* Spełnia tzw. Regułę Hollywood
    - proces podejmowania decyzji powinien być umieszczony w modułach wysokiego poziomu , które mogą samodzielnie decydować, jak i kiedy wywoływać moduły niskiego poziomu

---
layout: cover
background: /img/header-bg.svg 
---

# Strategy

---

# Strategy

* Przeznaczenie
    - definiuje rodziny algorytmów
    - dokonuje ich hermetyzacji i sprawia, że stają się one wymienne
    - pozwala na modyfikację danego algorytmu niezależnie od klienta, który tego algorytmu używa

---

# Strategy - Kontekst / Problem

* Kontekst
    - potrzebne są różne warianty jakiegoś algorytmu
    - wiele powiązanych ze sobą klas różni się tylko zachowaniem
    - w algorytmie są używane dane, o których klient nie powinien wiedzieć
    - klasa definiuje wiele zachowań, które w operacjach są uwzględnione w postaci wielokrotnych instrukcji warunkowych
* Problem
    - chcemy dokonać hermetyzacji algorytmu w klasie i stosując kompozycję umożliwić wymianę implementacji algorytmu

---
class: white-slide
---

# Strategy - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Strategy.png" alt="Strategy" class="width-70 center">

---

# Strategy - Konsekwencje

* Enkapsulacja algorytmu w oddzielnych klasach umożliwia modyfikację jego implementacji niezależnie od kontekstu
* Strategie eliminują instrukcje warunkowe
    - alternatywa dla stosowania instrukcji warunkowych w celu wybrania pożądanego zachowania
* Klienci muszą być świadomi istnienia różnych strategii i różnic pomiędzy nimi – potencjalna wada
* Zwiększona liczba obiektów

---

# Strategy - Implementacja (1)

* Definiowanie interfejsów strategii (Strategy) i kontekstu (Context)
    - Context przesyła dane do operacji  Strategy w argumentach
    - Context przesyła siebie samego jako argument, a strategia jawnie żąda danych od kontekstu
* Klasa kontekstu jest prostsza w użyciu, jeżeli użycie jej bez obiektu strategii ma sens
    - implementacja strategii domyślnej – klienci nie muszą zajmować się obiektami strategii

---

# Strategy - Implementacja (2)

* Jeśli strategie są prostymi metodami, można zrezygnować z enkapsulacji w postaci odrębnej klasy na rzecz ``std::function``

<div class="text-code-08">
```cpp
    using Strategy = std::function<Result (Args)>;

    class Context
    {
        Strategy strategy_;
    public:
        Context(Strategy strategy)
            : strategy_{strategy}
        {}

        void run()
        {
            auto result = strategy_();
            //...
        }
    };
```
</div>

---

# Strategy - Podsumowanie

* Strategia hermetyzuje algorytm w postaci obiektu
* Program wykorzystujący wzorzec Strategy może oferować wiele wersji algorytmu lub zachowania
* Zachowanie obiektów może się dynamicznie zmieniać w czasie wykonywania programu
* Delegowanie zachowania do oddzielnego obiektu z określonym interfejsem prowadzi do powstania klas o wysokiej kohezji
    - lepsza zgodność z SRP

---
layout: cover
background: /img/header-bg.svg 
---

# State

---

# State

* Umożliwia obiektowi zmianę zachowania, gdy zmienia się jego stan wewnętrzny

---

# State - Kontekst / Problem

* Kontekst
    - obiekt musi zmieniać swoje zachowanie w czasie wykonywania programu w zależności od stanu
    - operacje zawierają duże, wieloczęściowe instrukcje warunkowe, które zależą od stanu obiektu - wzorzec State przenosi każde rozgałęzienie warunkowe do oddzielnej klasy
* Problem
    - chcemy umożliwić obiektowi zmianę zachowania w momencie zmiany wewnętrznego stanu obiektu hermetyzując stan w postaci klasy

---
class: white-slide
---

# State - Struktura

<div class="span-v-4"/>

<img src="/img/dp/State.png" alt="State" class="width-70 center">

---

# State - Konsekwencje

* Umiejscowienie zachowania specyficznego dla stanu i rozdzielenie zachowania w wypadku różnych stanów
    - kod dla każdego stanu znajduje się w osobnej klasie – ułatwia to dodawanie nowych stanów (nie wymaga daleko idących modyfikacji istniejącego kodu)
* Eliminuje konieczność dzielenia kodu metod na bloki właściwe dla poszczególnych stanów (bloki switch)
* Jawność przejść między stanami
    - z perspektywy kontekstu przejścia między stanami są atomowe – dochodzi do nich poprzez wymianę pojedynczego obiektu stanu

---

# State - Implementacja

* Możliwość współdzielenia obiektów typu State
    - jeśli obiekty typu State są *immutable* to konteksty mogą je współdzielić
* Który z uczestników definiuje przejścia między stanami?

---

# State - Pokrewne wzorce

* Strategy
    - podobny schemat UML
    - o zmianie zachowanie zawsze decyduje klient
* Flyweight
    – stosowany gdy można współdzielić obiekty reprezentujące stan

---

# State - Podsumowanie

* Wzorzec projektowy State opisuje sytuację, w której zachowanie obiektu jest determinowane przez wewnętrzny stan, który może się zmieniać w reakcji na zachodzące zdarzenia
* Zapewnia lepszą skalowalność logiki zawiązanej z zarządzaniem stanów obiektu
* Poprzez hermetyzację stanów w klasach lokalizujemy przyszłe zmiany w kodzie

---
layout: cover
background: /img/header-bg.svg
---

# Chain Of Responsibility

---

# Chain Of Responsibility

* Umożliwia uniknięcie związania wysyłającego żądanie z odbiorcą żądania przez danie więcej niż jednemu obiektowi szansy obsłużenia tego żądania
* Żądanie jest przesyłane wzdłuż łańcucha obiektów, aż któryś je obsłuży
* Obiekt, który wygenerował żądanie, nie wie, kto je obsłuży – żądanie ma **niejawnego odbiorcę**
* Aby przekazywać żądania wzdłuż łańcucha i zagwarantować, że odbiorcy pozostaną niejawni, każdy obiekt w łańcuchu korzysta ze wspólnego interfejsu obsługi żądań i uzyskiwania dostępu do następnika w łańcuchu

---

# Chain of Responsibility - Kontekst / Problem

* Kontekst
    - więcej niż jeden obiekt może obsłużyć żądanie, a obiekt obsługujący nie jest znany a priori
    - wykonanie żądania nie jest gwarantowane
    - zbiór obiektów, które mogą obsłużyć żądanie, może być określony dynamicznie
* Problem
    - chcemy wysłać żądanie do jednego z kilku obiektów, nie określając jawnie odbiorcy
    - chcemy odseparować nadawcę żądania od jego odbiorców

---
class: white-slide
---

# Chain of Responsibility - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Chain.png" alt="Chain of Responsibility" class="width-50 center">

---

# Chain of Responsibility - Konsekwencje

* Zredukowanie powiązań
    - dany obiekt nie musi wiedzieć, który inny obiekt obsłuży żądanie
* Dodatkowa elastyczność w przydzielaniu obiektom zobowiązań
* Brak gwarancji odebrania żądania
    - ponieważ odbiorca żądania nie jest jawnie znany, nie ma gwarancji, że zostanie ono obsłużone – żądanie może wypaść z łańcucha, nie zostawszy w ogóle obsłużone

---

# Chain of Responsibility - Implementacja

* Implementacja łańcucha następników:
    - zdefiniowanie nowych powiązań
    - użycie istniejących powiązań

---

# Chain of Responsibility - Pokrewne wzorce

* Composite
    - często stosowany w połączeniu z Chain of Responsibility
    - rodzic komponentu może odgrywać rolę jego następnika

---

# Chain of Responsibility - Podsumowanie

* Separuje nadawcę żądania od jego odbiorców
* Wykonanie żądania nie jest gwarantowane
* Powszechnie używany w systemach okienkowych do obsługi zdarzeń
    - np. kliknięcie myszy, wciśnięcie klawisza

---
layout: cover
background: /img/header-bg.svg
---

# Observer

---

# Observer

* Określa zależność jeden-do-wiele między obiektami
* Gdy jeden obiekt zmienia stan, wszystkie obiekty od niego zależne są o tym automatycznie powiadamiane i uaktualniane

---

# Observer - Kontekst

* Kontekst
    - zmiana stanu jednego obiektu wymaga zmiany innych i nie wiadomo, ile obiektów trzeba zmienić
* Problem
    - obiekt powinien być w stanie powiadamiać inne obiekty, nie przyjmując żadnych założeń co do tego, co te obiekty reprezentują – wynikiem są luźniejsze powiązania między obiektami

---
class: white-slide
---

# Observer - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Observer.png" alt="Observer" class="width-50 center">

---

# Observer - Konsekwencje

* Abstrakcyjne powiązanie między **Obserwowanym** a **Obserwatorem**
    - mogą należeć do różnych warstw abstrakcji w systemie
* Wsparcie dla rozsyłania komunikatów
    - powiadomienie jest automatycznie nadawane do wszystkich zainteresowanych obiektów, które je zaprenumerowały
* Nieoczekiwane uaktualnienia
    - pozornie nieszkodliwa operacja zmiana stanu dotycząca **Obserwowanego** może spowodować kaskadę uaktualnień w **Obserwatorach** i obiektach od nich zależnych

---

# Observer - Implementacja

* Obserwowanie więcej niż jednego **Obserwowanego**
    - **Obserwowany** może po prostu przekazać siebie jako argument operacji ``Update()``, informując w ten sposób **Obserwatora**, który obiekt powinien sprawdzić
* Przesyłanie informacji o zmianie do **Obserwatora** – dwa modele:
    - **model push** – `Subject` wysyła szczegółową informację o zmianie (bez względu, czy obiekty obserwatorów tego chcą, czy nie)
    - **model pull** – `Subject` nie wysyła niczego poza powiadomieniem, a obserwatorzy jawnie pytają potem o szczegóły

---

# Observer - Podsumowanie

* Umożliwia obiektom dynamiczne rejestrowanie zależności między obiektami, dzięki czemu obiekty mogą powiadamiać swoje obiekty zależne o istotnych zmianach swoich stanów
* Obiekty obserwujące są luźno powiązane z obiektem obserwowanym
* Informacje o zmianach stanu obiektu obserwowanego mogą być wysyłane lub pobierane

---
layout: cover
background: /img/header-bg.svg
---

# Command

---

# Command

* Hermetyzuje żądania w postaci obiektu
* Usuwa sprzężenie pomiędzy obiektem wystawiającym żądanie, a obiektem, który wie, jak należy je zrealizować
* Umożliwia
    - parametryzowanie klientów różnymi żądaniami
    - kolejkowanie żądań oraz zapisywanie w dziennikach
* Ułatwia implementację anulowania operacji
* Enkapsuluje odbiorcę (obiekt realizujący) z operacją lub szeregiem operacji, które mają być zrealizowane

---

# Command - Kontekst / Problem

* Wzorca Command należy używać, gdy chcemy:
    - sparametryzować obiekt wykonywaną akcją – Command jest obiektową implementacją wywołań zwrotnych (**callbacks**)
    - uwzględnić możliwość anulowania wprowadzonych zmian
    - umożliwić wpisywanie zmian do dziennika, tak by można je było ponownie wykonać, gdy dojdzie do awarii systemu
    - stosować semantykę transakcji - transakcja hermetyzuje zbiór zmian danych w systemie

---
class: white-slide
---

# Command - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Command.png" alt="Command" class="width-70 center">

---

# Command - Konsekwencje

* Wzorzec Command oddziela obiekt, który wywołał operację, od tego, który wie, jak ją wykonać
    – separacja interfejsów wywołującego od odbiorcy
* Polecenia są obiektami
    – mogą być przetwarzane i rozszerzane tak jak inne obiekty
* Można łatwo dodawać nowe polecenia, gdyż nie wymaga to modyfikowania istniejących klas

---

# Command - Implementacja

* Polecenia mogą wspomagać anulowanie i przywracanie operacji, o ile zapewniają odpowiednie narzędzia do tego (np. operację ``Undo`` lub ``Redo``)
* Klasa ConcreteCommand może wymagać w tym celu pamiętania dodatkowego stanu
* Stan ten może zawierać:
    - obiekt odbiorcy (Receiver), który faktycznie wykonuje operacje w odpowiedzi na żądanie
    - argumenty operacji wykonywanych przez odbiorcę
    - wszystkie początkowe wartości z odbiorcy, które mogą zmienić się w wyniku obsługi żądania – odbiorca musi zapewnić operacje, które umożliwiają przywrócenie odbiorcy do poprzedniego stanu

---

# Command - Pokrewne wzorce

* Composite
    – wzorzec ten może zostać użyty do implementacji poleceń typu makro
* Prototype
    – polecenie, które musi być skopiowane przed umieszczeniem go na liście poleceń, działa jak Prototyp
* Memento
    – wzorzec często wykorzystywany do implementacji anulowania operacji

---

# Command - Podsumowanie

* Oddziela obiekt żądający wykonania danej operacji od obiektu, który wie jak tą operację wykonać
* Ułatwia kolejkowanie, selekcję i sterowanie czasem wykonywania poleceń
* Ułatwia wycofywanie i ponowne wykonywanie poleceń
* Ułatwia utrzymywanie trwałej historii wykonanych poleceń

---
layout: cover
background: /img/header-bg.svg
---

# Memento

---

# Memento

* Nie naruszając enkapsulacji, zapamiętuje i udostępnia na zewnątrz stan wewnętrzny obiektu, tak że obiekt może być później przywrócony do zapamiętanego stanu
* Memento jest obiektem przechowującym migawkę wewnętrznego stanu innego obiektu – jej źródła

---

# Memento - Problem / Kontekst

* Istnieje potrzeba zapamiętania migawki stanu obiektu (lub części stanu), tak by można go było potem przywrócić do tego stanu
* Bezpośredni interfejs do uzyskania stanu ujawniałby szczegóły implementacji i naruszał hermetyzację obiektu

---
class: white-slide
---

# Memento - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Memento.png" alt="Memento" class="width-70 center">

---

# Memento - Podsumowanie

* Zapamiętuje i udostępnia bez naruszenia zasad hermetyzacji wewnętrzny stan obiektu
* Dzięki pamiątce obiekt może być przywrócony do poprzedniego stanu

---
layout: cover
background: /img/header-bg.svg
---

# Mediator

---

# Mediator

* Definiuje obiekt enkapsulujący informacje o tym, jak obiekty współdziałają
* Przyczynia się do rozluźnienia powiązań między obiektami, ponieważ obiekty nie odwołują się do siebie wprost
* Zapewnia separację współpracujących obiektów od systemu

---

# Mediator - Stosowalność

* Zbiór obiektów porozumiewa się w dobrze zdefiniowany, lecz skomplikowany sposób
    - wynikające stąd zależności są nieuporządkowane i trudne do zrozumienia
* Ponowne użycie obiektu jest utrudnione, ponieważ odwołuje się i komunikuje się z wieloma innymi obiektami
* Implementacja zachowania jest rozproszona w wielu klasach - zmiana wymaga utworzenia wielu klas pochodnych

---
class: white-slide
---

# Mediator - Struktura

<div class="span-v-4"/>

<img src="/img/dp/Mediator.png" alt="Mediator" class="width-60 center">

---

# Mediator - Podsumowanie

* Hermetyzuje proces komunikacji między obiektami
* Definiuje luźne powiązania między obiektami
* Umożliwia łatwą reimplementację sposobu współpracy obiektów typu ``Coleague``, bez konieczności definiowania klas pochodnych dla nich

---
layout: cover
background: /img/header-bg.svg
---

# Visitor

---

# Visitor

* Określa operację, która ma być wykonana na elementach struktury obiektowej
* Umożliwia definiowanie nowej operacji bez modyfikowania klas elementów, na których ona działa

---
class: white-slide
layout: center
---

<img src="/img/dp/visitor-1.svg" alt="Visitor" class="width-80 center">

---
class: white-slide
layout: center
---

<img src="/img/dp/visitor-2.svg" alt="Visitor" class="width-80 center">

---
class: white-slide
layout: center
---
<img src="/img/dp/visitor-3.svg" alt="Visitor" class="width-80 center">

---
class: white-slide
layout: center
---

<img src="/img/dp/visitor-4.svg" alt="Visitor" class="width-80 center">

---
class: white-slide
layout: center
---

<img src="/img/dp/visitor-5.svg" alt="Visitor" class="width-80 center">

---
class: white-slide
layout: center
---

<img src="/img/dp/visitor-6.svg" alt="Visitor" class="width-80 center">

---
class: white-slide
layout: center
---

<img src="/img/dp/visitor-7.svg" alt="Visitor" class="width-80 center">

---

# Visitor - Kontekst / Problem

* Kontekst
    - klasy definiujące strukturę obiektową rzadko się zmieniają, ale chcemy często definiować nowe operacje w tej strukturze
* Problem
    - chcemy umożliwić łatwe dodawanie nowej operacji do struktury obiektowej bez konieczności otwierania klas tej struktury

---
class: white-slide
---

## Visitor - Struktura

<div class="span-v-2"/>

<img src="/img/dp/Visitor.png" alt="Visitor" class="width-60 center">

---

# Visitor - Konsekwencje

* Łatwe dodawanie nowych operacji
    - odwiedzający ułatwiają dodawanie operacji zależących od komponentów złożonych obiektów
    - nową operację na strukturze obiektowej definiuje się po prostu przez dodanie nowego odwiedzającego (Visitor)
* Zebranie razem powiązanych ze sobą operacji, a rozdzielenie tych niepowiązanych
    - powiązane ze sobą zachowania nie są rozsiane po wszystkich klasach definiujących strukturę obiektową, lecz są umiejscowione w klasie Visitor
    - upraszcza to zarówno klasy definiujące elementy, jak i algorytmy zdefiniowane w odwiedzających
* Trudne dodawanie nowych klas do wizytowanej hierarchii

---

# Visitor - Konsekwencje

* Odwiedzanie całej hierarchii klas
    - odwiedzający może odwiedzać obiekty nie mające wspólnego rodzica
    - do interfejsu Visitor można dodać operacje na obiektach dowolnego typu
* Kumulowanie stanu
    - odwiedzający mogą kumulować stan w miarę odwiedzania poszczególnych elementów struktury obiektowej.
    - bez wzorca ten stan byłby przekazywany jako dodatkowe argumenty do operacji wykonujących przechodzenie lub mógłby pojawić się w postaci zmiennych globalnych

---

# Visitor - Implementacja

* Wykorzystanie idiomu CRTP

```cpp
class Expression
{
public:
    virtual void accept(Visitor& v) = 0;
    virtual ~Expression() = default;
};
```

---

```cpp
template <typename VisitableType>
class VisitableExpression : public Expression
{
public:
    void accept(Visitor& v)
    {
        v.visit(static_cast<VisitableType&>(*this));
    }
};
```

```cpp
class IntExpr : public VisitableExpression<IntExpr>
{
    //...
};
```

---

# Visitor - Pokrewne wzorce

* Composite
    - odwiedzających można użyć do wykonania operacji dla wszystkich elementów struktury obiektowej, zdefiniowanej przez wzorzec Composite
* Interpreter
    - wzorzec Visitor można zastosować do interpretowania AST

---
layout: cover
background: /img/header-bg.svg
---

# Designing for Change

---

# Designing for Change

* A design that doesn't take change into account risks major redesign in the future
* Those changes might involve class redefinition and reimplementation, client modification, and retesting
* Design patterns help you avoid this by ensuring that a system can change in specific ways - each design pattern lets some aspect of system structure vary independently of other aspects

---

### Creating an object by specifying a class explicitly
  
* Factory Method
* Abstract Method
* Prototype

---

### Dependence on specific operations

* Command
* Chain of Responsibility

---

### Dependence on hardware and software platform

* Abstract Factory
* Bridge

---

### Dependence on object representations or implementations

* Abstract Factory
* Bridge
* Memento
* Proxy

---

### Algorithmic dependencies

* Builder
* Iterator
* Template Method
* Strategy
* Visitor

---

### Tight coupling

* Abstract Factory
* Facade
* Chain of Responsibility
* Mediator
* Bridge
  * Observer
  * Command

---

### Extending functionality by subclassing

* Decorator
* Composite
* Strategy


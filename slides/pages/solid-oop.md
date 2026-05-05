# Fundamenty OOP

* Abstrakcja
* Enkapsulacja
* Polimorfizm
* Ponowne wykorzystanie kodu
  - kompozycja & dziedziczenie

---

# Obiekt

* Zawiera zarówno **dane składowe** jak i implementację **funkcji składowych**, które operują na tych danych
* Wykonuje akcję po otrzymaniu **żądania** od klienta
* Wewnętrzny stan jest ukryty przed klientem - zaenkapsulowany

---

# Interfejs

* Zbiór operacji, które mogą być wykonane na obiekcie
* Nic nie mówi o implementacji
  + różne obiekty mające ten sam interfejs mogą różnie go implementować

---

# Klasa

* Definiuje **reprezentację** (dane) oraz **zachowanie** (implementację) dla obiektu
* Obiekty są instancjami danej klasy
* Za pomocą dziedziczenia klas można definiować **nowe klasy** wykorzystując kod klas, które już istnieją

---

# Klasa abstrakcyjna

* Definiuje interfejs dla klientów
* Operacje, które klasa abstrakcyjna deklaruje, ale których nie implementuje, nazywa się operacjami abstrakcyjnymi
  - w C++ są to **metody czysto wirtualne**

---
class: white-slide
---

<img src="/img/oop/abstract-class.svg" alt="abstract class" class="img-lg" />


---

## Klasa abstrakcyjna - kod

```cpp
    class Shape {
        int x_, y_;
    public:
        Shape(int x = 0, int y = 0) : x_{x}, y_{y}
        {}

        virtual ~Shape() = default;

        virtual void move(int dx, int dy) {
            x_ += dx;
            y_ += dy;
        }

        virtual void draw() const = 0;
    };
```

---

### Ekstrakcja interfejsu

```cpp
    class Shape {
    public:
        virtual ~Shape() = default;

        virtual void move(int dx, int dy)  = 0;
        virtual void draw() const = 0;
    };

    class ShapeBase : public Shape {
        Point coord_;
    public:
        void move(int dx, int dy)  override
        {
           coord_.x += dx;
           coord_.y += dy;
        }
    };
```

---
class: white-slide
---

<img src="/img/oop/shape-base.svg" alt="shape base" class="img-lg" />


---

# Polimorfizm

* Zapewnienie tego samego interfejsu dla wielu obiektów różnych typów
* Umożliwia zastępowanie w czasie wykonywania programu jednych obiektów drugimi, jeśli mają identyczny interfejs

---

# Rodzaje polimorfizmu

* Dynamiczny
* Statyczny

---

# Polimorfizm dynamiczny

* Implementacja jest wiązana z wywołaniem w trakcie działania programu - późne wiązanie
  * dziedziczenie publiczne i nadpisywanie metod z klasy bazowej
  * duck typing (np. Python)

---

## Polimorfizm dynamiczny - kod


<div class="text-code-08">

```cpp
class Formatter
{
public:
    virtual std::string format(const std::string& data) = 0;
    virtual ~Formatter() = default;
};

class Logger
{
    std::unique_ptr<Formatter> formatter_;

public:
    Logger(std::unique_ptr<Formatter> formatter)
        : formatter_{std::move(formatter)}
    { }

    void log(const std::string& data)
    {
        std::cout << "LOG: " << formatter_->format(data) << '\n';
    }
};
```
</div>

---

## Polimorfizm dynamiczny - kod

<div class="text-code-08">

```cpp
class UpperCaseFormatter : public Formatter {
public:
    std::string format(const std::string& data) override {
        std::string transformed_data{data};
        std::transform(data.begin(), data.end(), transformed_data.begin(),
            [](char c) { return std::toupper(c); });
        return transformed_data;
    }
};

class LowerCaseFormatter : public Formatter {
public:
    std::string format(const std::string& data) override {
        std::string transformed_data{data};
        std::transform(data.begin(), data.end(), transformed_data.begin(),
            [](char c) { return std::tolower(c); });
        return transformed_data;
    }
};
```

</div>

---

## Polimorfizm dynamiczny - kod

```cpp
Logger logger{std::make_unique<UpperCaseFormatter>()};
logger.log("Hello, World!");

logger = Logger{std::make_unique<LowerCaseFormatter>()};
logger.log("Hello, World!");
```

---

## Polimorfizm dynamiczny - Zalety i Wady

* Zalety:

  * **Elastyczność**: Pozwala na zmianę zachowania obiektu w czasie wykonywania programu. Umożliwia zdefiniowanie wspólnego interfejsu dla grupy klas i używanie ich zamiennie.

  * **Luźne powiązania**: Pozwala na stworzenie luźnych powiązań między klientem oczekującym określonej funkcjonalności a klasami, które tą funkcjonalność implementują.

---

## Polimorfizm dynamiczny - Zalety i Wady

* Wady:

  * **Wydajność**: Używa tablicy metod wirtualnych.
  * **Zużycie pamięci**: Wymaga przechowywania dodatkowych informacji o funkcjach wirtualnych (wskaźnik do tablicy metod wirtualnych w obiekcie).
  * **Semantyka referencji**: Wymaga użycia wskaźników lub referencji do klasy bazowej. Może prowadzić do bardziej złożonego kodu niż kod używający semantyki wartości. Często musimy dynamicznie przydzielać pamięć dla obiektów i używać inteligentnych wskaźników do zarządzania ich czasem życia.
  * **Wymaga dziedziczenia**: Wymaga użycia dziedziczenia. Tworzy silne powiązania między typami.

---

# Polimorfizm statyczny

* Działa na etapie kompilacji
  * szablony

---

## Polimorfizm statyczny - kod

<div class="text-code-08">

```cpp
template <typename TFormatter = UpperCaseFormatter>
class Logger
{
    TFormatter formatter_;

public:
    Logger() = default;

    Logger(TFormatter formatter)
        : formatter_(std::move(formatter))
    {
    }

    void log(const std::string& message)
    {
        std::cout << formatter_.format(message) << std::endl;
    }
};
```
</div>

---

## Polimorfizm statyczny - kod

<div class="text-code-08">

```cpp
struct UpperCaseFormatter
{
    std::string format(const std::string& message) const
    {
        std::string result = message;
        std::transform(result.begin(), result.end(),
            result.begin(), [](char c) { return std::toupper(c); });
        return result;
    }
};

struct CapitalizeFormatter
{
    std::string format(const std::string& message) const
    {
        std::string result = message;
        result[0] = std::toupper(result[0]);
        return result;
    }
};
```

</div>

---

## Polimorfizm statyczny - kod

```cpp
Logger logger{UpperCaseFormatter{}};
logger.log("Hello, World!");

Logger<CapitalizeFormatter> logger2;
logger2.log("hello, world!");
```

---

## Polimorfizm statyczny - Zalety i Wady

* Zalety:

  * **Wydajność**: Wywołania funkcji są wiązane w czasie kompilacji.

  * **Pamięć**: Nie wymaga przechowywania wskaźnika do tablicy metod wirtualnych każdym obiekcie.

  * **Semantyka wartości**: Pozwala na użycie semantyki wartości. Składowe klasy nie wymagają dynamicznej alokacji pamięci i użycia wskaźników.

  * **Brak dziedziczenia**: Polimorfizm statyczny nie wymaga użycia dziedziczenia.

---

## Polimorfizm statyczny - Zalety i Wady

* Wady:

  * **Czas kompilacji**: Polimorfizm statyczny wymaga ustawienia zachowania obiektu w czasie kompilacji. Nie pozwala na zmianę zachowania obiektu w czasie wykonywania.

  * **Składnia**: Polimorfizm statyczny wymaga użycia szablonów. Może prowadzić do bardziej złożonej składni niż polimorfizm dynamiczny.

---

# Podstawowe techniki OOP

* Dziedziczenie
* Kompozycja
* Delegacja

---

# Dziedziczenie

---

## Dziedziczenie implementacji

* Definiuje implementację danego obiektu wykorzystując implementację innego obiektu
* Mechanizm współdzielenia kodu
* C++ - dziedziczenie prywatne

---

<div class="text-code-08">

```cpp
    class Set : private std::set<int> {
        using BaseImpl = std::set<int>;

    public:
        using BaseImpl::BaseImpl;

        size_t size() const { return BaseImpl::size(); }

        const int& operator[](size_t index) const {
            return *std::next(BaseImpl::begin(), index);
        }

        bool add_item(int value) {
            return BaseImpl::insert(value).second;
        }

        bool remove_item(int value) {
            return BaseImpl::erase(value) > 0;
        }
    };
```

</div>

---

## Dziedziczenie interfejsu

* Określa, kiedy jeden obiekt może być używany zamiast drugiego
* C++ - publiczne dziedziczenie po klasie, która ma (czysto) wirtualne funkcje składowe

---

<div class="text-code-07">

```cpp
class Shape
{
public:
    virtual move(int dx, int dy) = 0
    virtual void draw() const = 0;
    virtual ~Shape() = default;
};

class Square : public Shape
{
    Rectangle rect_;
public:
    Square(int x, int y, int size) : rect_(x, y, size, size) {}
    void move(int dx, int dy) override { rect_.move(dx, dy); }
    void draw() const override { rect_.draw(); }
};
```

```cpp
void draw_shapes(const std::vector<std::unique_ptr<Shape>>& shapes)
{
    for (const auto& shape : shapes)
    {
        shape->draw();
    }
}
```
</div>

---

# Dziedziczenie - wady (?/!)

* narusza enkapsulację
  - pola ``protected`` - implementacja typu pochodnego może zależeć od szczegółów implementacji typu bazowego
* jest statyczne
  - zachowanie (implementacja) jest związana z typem

---

# OOP Tip #1

Program to an interface, not an implementation!

---

# Kompozycja

* jest definiowana dynamicznie (runtime)
* nie może naruszyć enkapsulacji
* pozwala tworzyć typy zgodne z SRP

---

# OOP Tip #2

Favor object composition over class inheritance!

---

# Delegacja

* bardziej uniwersalny od dziedziczenia sposób rozszerzania zachowania klasy

---

# Delegowanie żądań

* Dwa obiekty są zaangażowane w obsługę żądania
  * obiekt **przyjmujący żądanie** przekazuje operacje swojemu **delegatowi**

---

# Delegacja vs. Dziedziczenie

---
class: white-slide
layout: image
---

<div class= "br-lg"/>

<img src="/img/oop/delegation-before.svg" alt="Delegation Before" class="img-md center" />


<v-click>
<div class= "br-lg"/>
<center>
Użycie dziedziczenia <span v-mark.underline.orange>statycznie wiąże zachowanie z typem</span>
</center>
</v-click> 

---
class: white-slide
layout: image
---

<div class= "br-lg"/>
<img src="/img/oop/delegation-after.svg" alt="Delegation After" class="img-lg center" />

<v-click>
<div class= "br-lg"/>
<center>
Delegacja umożliwia <span v-mark.underline.green>dynamiczne składanie zachowań w czasie wykonywania programu</span>
</center>
</v-click> 

---

# Delegacja - zalety & wady

<v-clicks>

* Zalety
  - umożliwia składanie zachowań w czasie wykonywania programu – obiekt przyjmujący żądanie może zmieniać swoje zachowanie
* Wady
  - dynamiczne, wysoce sparametryzowane oprogramowanie jest trudniej zrozumieć niż oprogramowanie statyczne

</v-clicks>

---

# Atrybuty dobrego projektu OOP

* Dobre projekty zorientowane obiektowo:
  - Powinny nadawać się do wielokrotnego użytku
  - Być proste do rozbudowy
  - Być łatwe w serwisowaniu i modyfikacji
  - Być łatwe w testowaniu

---
layout: cover
background: /img/petals.svg
---

# S.O.L.I.D. OOP

---
layout: center
---

<v-clicks>

* Single Responsibility Principle

* Opened-Closed Principle

* Liskov Substitution Principle

* Interface Segregation Principle

* Dependency Inversion Principle

</v-clicks>

---

# Single Responsibility Principle

* każdy obiekt w kodzie powinien mieć tylko jedną odpowiedzialność, a wszystkie usługi tego obiektu powinny koncentrować się na jej realizacji

---
class: white-slide
layout: center
---

<div class="slogan">

Każda klasa powinna mieć tylko jeden <v-click><span v-mark.underline.red> powód do modyfikacji!</span></v-click> 

</div>

---
class: white-slide
theme: image
layout: center
---

![SRP](/img/solid/srp.svg)

---

# Open-Closed Principle

<v-clicks>

* Klasy powinny być <span v-mark.underline.green>otwarte na rozbudowę</span> i <span v-mark.underline.red>zamknięte na modyfikacje</span>. 

</v-clicks>

---
class: white-slide
---

<center>

## Naruszenie zasady OCP

</center>

<v-clicks>

<div class="text-code-08">
```cpp
struct Server
{
    void run() { /*implementation*/ }
}

class Client
{
    Server server_;
public:
    void use()
    {
        server_.run();
    }
};
```
</div>

<div class="span-v-2"/>
<img src="/img/solid/ocp-before.png" alt="OCP Before" class="width-50 center" />
<div class="span-v-2"/>

</v-clicks>

---
class: white-slide
layout: center
---

## Rozwiązanie = Interfejs

<center>
<div class="span-v-4"/>
<img src="/img/solid/ocp-after.png" alt="OCP After" class="width-40 center" />
</center>

---

# Liskov Substitution Principle

<v-click>

* Musi istnieć możliwość podstawiania typów pochodnych w miejsce ich typów bazowych

</v-click>

---
class: white-slide
layout: center
---

<div class="slogan">

Jeżeli <span style="color: #dd2222">S</span> jest podtypem <span style="color: #22aa22">T</span>, wtedy obiekty typu <span style="color: #22aa22">T</span>
mogą zostać zastąpione instancjami typu <span style="color: #dd2222">S</span> bez naruszenia istotnych właściwości programu (niezmienników, poprawności, itp.).

</div>
---

## Design by contract

* Pre-conditions cannot be strengthened in a subtype
* Post-conditions cannot be weakened in a subtype
* Invariants of the supertype must be preserved in a subtype

---
class: white-slide
layout: center
---

## Naruszenie zasady LSP

<span class="span-v-4"/>

<v-click>
<img src="/img/solid/lsp.svg" alt="LSP Before" class="img-lg" />
</v-click>

---

# Interface Segregation Principle

<v-click>

* Klient nie powinien być zmuszany do <span v-mark.underline.green>zależności od metod, których nie używa.</span>  

</v-click>

---
class: white-slide
layout: center
---

## Naruszenie ISP

![isp-before](/img/solid/ISP-Before.svg)

---
class: white-slide
layout: center
---

## ISP - lepsze rozwiązanie

<span class="span-v-4"/>

![isp-after](/img/solid/ISP-After.svg)

---

# Dependency Inversion Principle

* <span v-mark.underline.green>Moduły wysokopoziomowe</span> nie powinny zależeć od <span v-mark.underline.red>modułów niskopoziomowych</span>. Obie grupy modułów powinny zależeć od <span v-mark.underline.blue>abstrakcji</span>.
* Abstrakcje nie powinny zależeć od szczegółowych rozwiązań. To szczegółowe rozwiązania powinny zależeć od abstrakcji

---
class: white-slide
layout: center
---

## Naruszenie DIP

<span class="span-v-4"/>

<img src="/img/solid/dip-before.png" alt="DIP Before" class="width-50 center" />

---
class: white-slide
layout: center
---

## DIP

<span class="span-v-4"/>

<img src="/img/solid/dip-after.png" alt="DIP After" class="width-80 center" />

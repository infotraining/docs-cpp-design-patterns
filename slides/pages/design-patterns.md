---
layout: cover
background: /img/header-bg.svg
---

# Wzorce projektowe

---
layout: center
---

<div class="text-xl">

> Wzorzec opisuje **problem** występujący wielokrotnie w danym środowisku, pokazując **podstawowe rozwiązanie** tego problemu dane w taki sposób, aby **można wielokrotnie użyć tego rozwiązania** do wszystkich wystąpień danego problemu, bez konieczności ponownego wykonywania tych samych czynności projektowych
>
> *— Christopher Alexander - "A Pattern Language"*

</div>

---
layout: image-right
image: /img/dp/gof.jpg
background-size: 20em 70%
---

# Gang of Four

* Erich Gamma
* Richard Helm
* Ralph Johnson
* John Vlissides

---

# Wzorce projektowe - zalety

* Sprawdziły się w wielu rzeczywistych aplikacjach systemów zorientowanych obiektowo
* Wskazują sposoby tworzenia całych systemów posiadających cechy charakterystyczne dla dobrych (SOLIDnych) projektów zorientowanych obiektowo
* Nie udostępniają gotowego kodu, a jedynie ogólne sposoby rozwiązywania problemów pojawiających się w fazie projektowania

---

# Wzorce projektowe - zalety

* Wzorce zapewniają rodzaj **wspólnego języka**, który może maksymalizować efektywność komunikacji pomiędzy poszczególnymi członkami zespołu
* Większość wzorców umożliwia modyfikowanie pewnych fragmentów systemu całkowicie niezależnie od pozostałych elementów systemu
* Wzorce zwykle odnoszą się do sytuacji, w których w danym oprogramowaniu muszą zostać dokonane określone zmiany - umożliwiają hermetyzację elementów wykazujących się częstą zmiennością

---

# Wzorzec projektowy

* Wzorzec dostarcza abstrakcyjnego opisu problemu projektowego i tego jak ogólny układ elementów (klas i obiektów) rozwiązuje ten problem.

---

# Wzorzec projektowy


* ma cztery zasadnicze elementy:

  <v-clicks>

  - **Nazwa wzorca** – skrót, którego można użyć do zwięzłego określenia problemu projektowego. Umożliwia projektowanie na wyższym poziomie abstrakcji
  - **Problem** – określa, kiedy stosować dany wzorzec
  - **Rozwiązanie** – ogólny opis elementów składających się na rozwiązanie zdefiniowanego problemu
  - **Konsekwencje** – zalety oraz wady zastosowania wzorca

  </v-clicks>
---

## Klasyfikacja wzorców wg GOF

* Kreacyjne (Creational)
* Strukturalne (Structural)
* Czynnościowe (Behavioral)

---

### Katalog wzorców projektowych

<div text-size="0.8em">

|    Kreacyjne     | Strukturalne |      Behawioralne
| ---------------- | ------------ | -----------------------
| Factory Method   | Adapter      | Interpreter
| Abstract Factory | Decorator    | Template Method
| Prototype        | Composite    | Strategy
| Builder          | Proxy        | State
| Singleton        | Facade       | Chain Of Responsibility
|                  | Bridge       | Observer
|                  | Flyweight    | Command
|                  |              | Memento
|                  |              | Mediator
|                  |              | Iterator
|                  |              | Visitor

</div>
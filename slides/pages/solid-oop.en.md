# Four Tenets of OOP

* Abstraction
* Encapsulation
* Polymorphism
* Composition & inheritance
  * Code reuse

<!-- Four Tenets of OOP 

Abstraction: Focusing on what an object does, not how it does it.
 * Hide complexity behind a simple interface
 * Provide meaningful abstractions (interfaces) to clients

Encapsulation: Bundling the data & behavior (the methods that operate on the data) within a single object, and restricting access to some of the object's components.
  * Provides controlled access to the object's data through methods
  * Protects the internal state of the object from unintended interference
  * Expose a public interface for controlled interaction with the object's data

Polymorphism: Different objects can be used through the same interface
  * Enables code to work with objects of different types through a common interface
  * Supports code extensibility and flexibility by allowing new types to be introduced without modifying existing code

Composition & Inheritance: Mechanisms for code reuse and establishing relationships between classes.
  * Composition: Building complex objects by combining simpler ones
  * Inheritance: Creating new classes based on existing ones, inheriting their behavior and attributes, extending behavior

-->

---

# Object

* Contains both **data members** and implementations of **member functions** that operate on that data
* Performs an action after receiving a **request** from a client
* Its internal state is hidden from the client and encapsulated

---

# Interface

* A set of operations that can be performed on an object
* Says nothing about the implementation
    + different objects with the same interface may implement it differently

---

# Class

* Defines the **representation** (data) and **behavior** (implementation) of an object
* Objects are instances of a class
* Class inheritance can define **new classes** using the code of existing classes

---

# Abstract Class

* Defines an interface for clients
* Operations declared but not implemented by an abstract class are called abstract operations
    - in C++, these are **pure virtual methods**

---
class: white-slide
---

<img src="/img/oop/abstract-class.svg" alt="abstract class" class="img-lg" />


---

## Abstract Class - Code

```cpp {all|2}
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

### Interface Extraction

```cpp {all|1-7|9-17}
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

# Polymorphism

* Providing the same interface for multiple objects of different types
* It's all about substitutability
* Allows one object to be replaced by another

---

# Types of Polymorphism

* Dynamic
* Static

---

# Dynamic Polymorphism

<v-clicks>

* Allows one object to be replaced by another at runtime as long as both share the same interface
* The decision which member function to call is made dynamically through **virtual dispatch** (vtable)
* Uses public inheritance and overriding methods from a base class 
* Alternative takes
  * restricted polymorphism using `std::variant`
  * duck typing (like in Python)
    * polymorphic wrappers using type erasure (like `std::function`)
    * `std::protocol<I>` and `std::protocol_view<I>` proposal for C++29 (v-tables are generated using reflection)
</v-clicks>

---

## Dynamic Polymorphism - Code


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

## Dynamic Polymorphism - Code

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

## Dynamic Polymorphism - Code

```cpp
Logger logger{std::make_unique<UpperCaseFormatter>()};
logger.log("Hello, World!");

logger = Logger{std::make_unique<LowerCaseFormatter>()};
logger.log("Hello, World!");
```

---

## Dynamic Polymorphism - Advantages and Disadvantages

* Advantages:

    * **Flexibility**: Allows an object's behavior to change at runtime. Makes it possible to define a common interface for a group of classes and use them interchangeably.

    * **Loose coupling**: Creates loose coupling between a client expecting specific functionality and the classes that implement it.

---

## Dynamic Polymorphism - Advantages and Disadvantages

* Disadvantages:

    * **Performance**: Uses a virtual method table.
    * **Memory usage**: Requires storing additional information about virtual functions (a pointer to the object's virtual method table).
    * **Reference semantics**: Requires pointers or references to a base class. This can produce more complex code than value semantics. Objects often need dynamic allocation and smart pointers to manage their lifetime.
    * **Requires inheritance**: Requires inheritance, which creates strong coupling between types.

---

# Static Polymorphism

* Works at compile time
    * templates

---

## Static Polymorphism - Code

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

## Static Polymorphism - Code

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

## Static Polymorphism - Code

```cpp
Logger logger{UpperCaseFormatter{}};
logger.log("Hello, World!");

Logger<CapitalizeFormatter> logger2;
logger2.log("hello, world!");
```

---

## Static Polymorphism - Advantages and Disadvantages

* Advantages:

    * **Performance**: Function calls are bound at compile time.

    * **Memory**: Does not require storing a pointer to a virtual method table in every object.

    * **Value semantics**: Allows value semantics. Class members do not require dynamic memory allocation or pointers.

    * **No inheritance**: Static polymorphism does not require inheritance.

---

## Static Polymorphism - Advantages and Disadvantages

* Disadvantages:

    * **Compile time**: Static polymorphism requires an object's behavior to be selected at compile time. It does not allow the behavior to change at runtime.

    * **Syntax**: Static polymorphism requires templates, which can lead to more complex syntax than dynamic polymorphism.

---

# Basic OOP Techniques

* Inheritance
* Composition
* Delegation

---

# Inheritance

* Implementation inheritance
* Interface inheritance

---

## Implementation Inheritance

* Derived class inherits data members and concrete methods from the base class
* No polymorphism required
* No virtual functions involved
* A mechanism for **code reuse**
* C++ - **private inheritance**

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

## Interface Inheritance

* Means inheriting only the **contract** — the set of functions a type must implement — without inheriting any concrete behavior
* Defines when one object can be used instead of another - enforces **substitutability**
* C++ - **public inheritance** from a class with **pure-virtual member functions**

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

## Typical Inheritance

* Has hybrid form
* Combines both implementation and interface inheritance

---

# Inheritance - Disadvantages (?/!)

<v-clicks depth=2>

* Can lead to **tight coupling** between base and derived classes 
  * Changes in the base class can have unintended consequences on derived classes
  * Violates encapsulation
    - `protected` fields allow a derived type's implementation to depend on details of the base type's implementation
* Is static - **behavior is fixed** at compile time
    - A new behavior (implementation) is tied to the type that relies on a base class
    - Cannot easily change behavior at runtime
    - Inheritance hard‑codes behavior into the type system

</v-clicks>

---

# Composition

<v-clicks>

* Composition means building **complex objects by combining simpler objects**.
* Is **defined dynamically** (at runtime) - you can change behavior at runtime by swapping components.
* Cannot violate encapsulation
* Allows the creation of types that comply with **SRP** - leads to cohesive and maintainable code
* Each component is independent and replaceable

</v-clicks>
 
---

# Delegation

* A more general way to extend a class's behavior than inheritance

---

# Delegating Requests

* Two objects are involved in handling a request
    * the **request-receiving** object delegates operations to its **delegate**

---

# Delegation vs. Inheritance

---
class: white-slide
layout: image
---

<div class= "br-lg"/>

<img src="/img/oop/delegation-before.svg" alt="Delegation Before" class="img-md center" />


<v-click>
<div class= "br-lg"/>
<center>
Using inheritance <span v-mark.underline.orange>statically binds behavior to the type</span>
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
Delegation enables <span v-mark.underline.green>dynamic composition of behavior at runtime</span>
</center>
</v-click> 

---

# Delegation - Advantages & Disadvantages

<v-clicks>

* Advantages
    - enables behavior to be composed at runtime; the request-receiving object can change its behavior
* Disadvantages
    - dynamic, highly parameterized software is harder to understand than static software

</v-clicks>

---

# Attributes of Good OOP Design

* Good object-oriented designs:

  - Should be reusable
  - Should be easy to extend
  - Should be easy to maintain and modify
  - Should be easy to test

---

# Tips for Good OOP Design

- Prefer composition over inheritance
- Program to interfaces, not implementations
- Keep classes and methods small and focused
- Encapsulate what varies
- Strive for high cohesion and low coupling

---
layout: cover
background: /img/petals.svg
---

# S.O.L.I.D. OOP

---
layout: center
---

<div class="no-bullets text-2">

<v-clicks>

* Single Responsibility Principle
* Open-Closed Principle
* Liskov Substitution Principle
* Interface Segregation Principle
* Dependency Inversion Principle

</v-clicks>

</div>

---

# Single Responsibility Principle

* A class should be responsible for **one thing**, one well‑defined aspect of the system
* SRP is about **cohesion**, **clarity**, and **maintainability** of classes

---
class: white-slide
layout: center
---

<div class="slogan">

Every class should have only one <v-click><span v-mark.underline.red> reason to change!</span></v-click> 

</div>

---
class: white-slide
theme: image
layout: center
---

<img src="/img/solid/SOLID SRP - Before.excalidraw - 1.svg" alt="SRP Before" class="img-lg center" />

---
class: white-slide
theme: image
layout: center
---

<img src="/img/solid/SOLID SRP - Before.excalidraw - 2.svg" alt="SRP After" class="img-lg center" />

---
class: white-slide
theme: image
layout: center
---

<div class="slogan">
How to refactor a class to adhere to SRP?
</div>

---
class: white-slide
theme: image
layout: center
---

<img src="/img/solid/SOLID SRP - After.excalidraw.svg" alt="SRP Refactored" class="center" />

---


## Why SRP matters

<v-clicks>

* Less coupling
* Easier to understand and maintain
* Easier to test

</v-clicks>

---

# Open-Closed Principle

<v-clicks depth="2">

* A module (class, function, component) should be
  * <span v-mark.underline.green>open for extension</span> and
  * <span v-mark.underline.red>closed for modification</span>.
* OCP is about protecting stable code while still allowing the system to grow.
  
</v-clicks>

---
class: white-slide
theme: image
layout: center
---

## Violation of the Open-Closed Principle

<img src="/img/solid/SOLID OCP - Db - Before.excalidraw.svg" alt="OCP Before" class="img-md center" />

---

## What violates the Open-Closed Principle?

* Big `switch` or `if(type)` statements
* Hard‑coded behavior
* Modifying existing classes every time a new case appears
* Deep inheritance hierarchies that require touching old code

---
class: white-slide
theme: image
layout: center
---

<div class="slogan">
How to refactor a class to adhere to OCP?
</div>

---
class: white-slide
theme: image
layout: center
---

<img src="/img/solid/SOLID OCP - Db - After.excalidraw.svg" alt="OCP After" class="img-lg center" />

---
class: white-slide
theme: image
layout: center
---

## Solution == Interface

<center>
<div class="span-v-4"/>
<img src="/img/solid/SOLID OCP.excalidraw.svg" alt="OCP After" class="width-60 center" />
</center>

---

## Why OCP matters?

* **Stability** — tested code stays untouched.
* **Scalability** — adding the 10th variant is as safe as adding the 2nd.
* **Parallel development** — teams can add features without merge conflicts.
* **Lower regression risk** — only new classes need testing.

---

# Liskov Substitution Principle

<v-clicks>

* A **subclass** must be usable anywhere its base class is expected — **without breaking correctness**.
* The core idea: subtypes must preserve the behavior (the contract) of their supertypes.

</v-clicks>

---
class: white-slide
layout: center
---

<div class="slogan">

If <span style="color: #dd2222">S</span> is a subtype of <span style="color: #22aa22">T</span>, then objects of type <span style="color: #22aa22">T</span>
can be replaced with instances of type <span style="color: #dd2222">S</span> without violating the essential properties of the program (invariants, correctness, etc.).

</div>
---

## Design by contract

<v-clicks>

* Pre-conditions cannot be strengthened in a subtype
  * A subclass cannot demand more from the caller than the base class.
* Post-conditions cannot be weakened in a subtype
  * A subclass cannot guarantee less than the base class promises.
* Invariants of the supertype must be preserved in a subtype

</v-clicks>

---
class: white-slide
---

## Violating the LSP

<img src="/img/solid/SOLID LSP - Before.excalidraw - 1.svg" class="width-80 center" />

---
class: white-slide
---

## Violating the LSP


<img src="/img/solid/SOLID LSP - Before.excalidraw - 2.svg" class="width-80 center" />

---

## Violating the LSP

* Example of LSP violation: testing a Square as a Rectangle

```c++ {all|2-3} 
void test_rectangle_area(Rectangle& r) {
    r.set_width(10);
    r.set_height(20);
    assert(r.area() == 10 * 20); // should be 200
}

Rectangle r;
test_rectangle_area(r);  // should pass

Square sq;
test_rectangle_area(sq); // assertion will fail because Square violates LSP
```
---

## Refactored to LSP

```c++
class Square 
{
    Rectangle rect;  // use composition instead of inheritance
public:
    void set_side(int side) {
        rect.set_width(side);
        rect.set_height(side);
    }

    int area() const {
        return rect.area(); // delegation to the composed Rectangle object
    }
}
```

---

## How to design for LSP

* Keep base class contracts clear and minimal
* Ensure subclasses only extend, never contradict behavior
* Avoid inheritance when behavior diverges → use composition or interfaces instead

---

# Interface Segregation Principle

<v-click>

* A client should not be forced to <span v-mark.underline.green>depend on methods it does not use.</span>
* It’s about small, focused interfaces instead of large, “fat” ones.

</v-click>

---
class: white-slide
---

## Violating the ISP

<span class="span-v-4"/>

<img src="/img/solid/SOLID ISP - Before.excalidraw.svg" alt="ISP Before" class="width-70 center" />

---
class: white-slide
---

## Refactored to ISP

<span class="span-v-4"/>

<img src="/img/solid/SOLID ISP - After.excalidraw.svg" alt="ISP After" class="width-90 center" />

---

## Why ISP Matters?

* Encourages the creation of focused, **cohesive interfaces**
  * Splits large interfaces by responsibility
  * One interface per role, not one per domain
* Client depends only on the methods it actually uses
* Reduces the impact of changes in one part of the system on other parts

---

# Dependency Inversion Principle

* <span v-mark.underline.green>High-level modules</span> should not depend on <span v-mark.underline.red>low-level modules</span>. Both groups of modules should depend on <span v-mark.underline.blue>abstractions</span>.
* Abstractions should not depend on details. Details should depend on abstractions.

---
class: white-slide
layout: center
---

## Violating the DIP

<img src="/img/solid/SOLID DIP - Before.excalidraw - 1.svg" alt="DIP Before" class="width-80 center" />

---
class: white-slide
layout: center
---

## Violating the DIP

<img src="/img/solid/SOLID DIP - Before.excalidraw - 2.svg" alt="DIP Before" class="width-80 center" />


---
class: white-slide
layout: center
---

## Refactored to DIP

<img src="/img/solid/SOLID DIP - After.excalidraw - 1.svg" alt="DIP After" class="width-90 center" />

---
class: white-slide
layout: center
---

## Refactored to DIP

<img src="/img/solid/SOLID DIP - After.excalidraw - 2.svg" alt="DIP After" class="width-90 center" />

---

## Why DIP Matters?

* Promotes decoupling between high-level and low-level modules
* Makes the system more flexible and easier to maintain
* Encourages the use of abstractions, leading to more reusable code
* Reduces the risk of changes in low-level modules affecting high-level modules
* Simplifies testability - allows to use mock implementations for dependencies
# OOP Fundamentals

* Abstraction
* Encapsulation
* Polymorphism
* Composition & inheritance
  * Code reuse

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
        ## Violating the LSP
    };
```

---

### Interface Extraction

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

# Polymorphism

* Providing the same interface for multiple objects of different types
* Allows one object to be replaced by another at runtime when they have identical interfaces

---

# Types of Polymorphism

* Dynamic
* Static

---

# Dynamic Polymorphism

* The implementation is bound to the call while the program is running - late binding
    * public inheritance and overriding methods from a base class
    * duck typing (e.g. Python)

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

---

## Implementation Inheritance

* Defines one object's implementation using another object's implementation
* A mechanism for sharing code
* C++: private inheritance

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

* Defines when one object can be used instead of another
* C++: public inheritance from a class with (pure) virtual member functions

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

# Inheritance - Disadvantages (?/!)

* Violates encapsulation
    - ``protected`` fields allow a derived type's implementation to depend on details of the base type's implementation
* Is static
    - behavior (implementation) is tied to the type

---

# OOP Tip #1

Program to an interface, not an implementation!

---

# Composition

* Is defined dynamically (at runtime)
* Cannot violate encapsulation
* Allows the creation of types that comply with SRP

---

# OOP Tip #2

Favor object composition over class inheritance!

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
* Opened-Closed Principle
* Liskov Substitution Principle
* Interface Segregation Principle
* Dependency Inversion Principle

</v-clicks>

</div>

---

# Single Responsibility Principle

* every object in the code should have only one responsibility, and all of its services should focus on fulfilling it

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

![SRP](/img/solid/srp.svg)

---

# Open-Closed Principle

<v-clicks>

* Classes should be <span v-mark.underline.green>open for extension</span> and <span v-mark.underline.red>closed for modification</span>.

</v-clicks>

---
class: white-slide
---

<center>

## Violating the OCP

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

## Solution = Interface

<center>
<div class="span-v-4"/>
<img src="/img/solid/ocp-after.png" alt="OCP After" class="width-40 center" />
</center>

---

# Liskov Substitution Principle

<v-click>

* It must be possible to substitute derived types for their base types

</v-click>

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

* Pre-conditions cannot be strengthened in a subtype
* Post-conditions cannot be weakened in a subtype
* Invariants of the supertype must be preserved in a subtype

---
class: white-slide
layout: center
---

## Violating the LSP

<span class="span-v-4"/>

<v-click>
<img src="/img/solid/lsp.svg" alt="LSP Before" class="img-lg" />
</v-click>

---

# Interface Segregation Principle

<v-click>

* A client should not be forced to <span v-mark.underline.green>depend on methods it does not use.</span>

</v-click>

---
class: white-slide
layout: center
---

## Violating the ISP

<span class="span-v-4"/>

<img src="/img/solid/ISP-Before.svg" alt="ISP Before" class="width-80 center" />

---
class: white-slide
layout: center
---

## ISP - Better Solution

<span class="span-v-4"/>

<img src="/img/solid/ISP-After.svg" alt="ISP After" class="width-80 center" />

---

# Dependency Inversion Principle

* <span v-mark.underline.green>High-level modules</span> should not depend on <span v-mark.underline.red>low-level modules</span>. Both groups of modules should depend on <span v-mark.underline.blue>abstractions</span>.
* Abstractions should not depend on details. Details should depend on abstractions.

---
class: white-slide
layout: center
---

## Violating the DIP

<span class="span-v-4"/>

<img src="/img/solid/dip-before.png" alt="DIP Before" class="width-50 center" />

---
class: white-slide
layout: center
---

## DIP

<span class="span-v-4"/>

<img src="/img/solid/dip-after.png" alt="DIP After" class="width-80 center" />

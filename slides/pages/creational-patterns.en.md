---
layout: cover
background: /img/header-bg.svg
---

# Creational Patterns

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

# Creational Patterns - Introduction

* Abstract the object creation process
* Facilitate building systems independent of how their objects are created, assembled, and represented
* Encapsulate knowledge of which concrete classes the system uses and how instances of those classes are created, configured, and combined

---

# Creational Patterns - Introduction

* Make it possible to configure a system with product objects that differ significantly in structure and functionality
    - Static configuration (at compile time)
    - Dynamic configuration (at runtime)

---
layout: center
---

# Factories

<div class="text-xl">

> Prefer loose coupling between classes
>
> *— A principle of good OOP design*

</div>

---

Code that violates this principle:

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

* Classes are strongly coupled (**strong coupling**)
* The ``MusicApp`` class is difficult to unit test

</v-clicks>
</div>


---

# Factories

* To create an object, we must know its exact type
* Sometimes, however:
    - We want to leave that precise knowledge to someone else
    - We have the object type as an identifier, such as a `std::string`
    - The type of another object determines the type of the object being created

---
layout: cover
background: /img/header-bg.svg 
---
# Factory Method

---

# Factory Method

* Intent
    - defines an interface for creating objects, while delegating responsibility for object creation to derived classes
    - uses inheritance to let derived classes decide which class of object is created

---

# Factory Method - Scenario

How can we improve the following code?

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

## Step #1

* Extract the service interface:

```cpp
class MusicService
{
public:
    virtual std::optional<Track> get_track(const std::string& title) = 0;
    virtual ~MusicService() = default;
};
```

---

## Step #2

* Abstract the process of creating an instance of ``MusicService``:

```cpp
class MusicServiceCreator
{
public:
    virtual std::unique_ptr<MusicService> create_music_service() = 0;
    virtual ~MusicServiceCreator() = default;
};
```

---

## Step #3

* Inject the factory dependency into ``MusicApp``:

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

## Step #4

* Delegate the creation process to the factory object:

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

# Factory Method - Context

* We want to introduce new functionality by writing a new class and creating an instance of that class

---

# Factory Method - Problem

* We want to create instances of concrete classes through an interface
* A class cannot anticipate the type of object that must be created
* Information about the type of object to create is available only at runtime

---
class: white-slide
layout: center
---

# Factory Method - Structure

<div class="span-v-4"/>

<img src="/img/dp/Factory.png" alt="Factory Method" class="width-80 center"/>


---

# Factory Method - Collaboration

* An object of class ``Creator`` delegates to its derived classes the responsibility for defining the factory method so that it creates an instance of the appropriate ``ConcreteProduct`` class (a subclass of the abstract ``Product`` class)

---

# Factory Method - Consequences

* Eliminates the need to include concrete classes in application code.
* Creating objects inside a class through a factory method is more flexible than creating them directly.
* Promotes loose coupling between objects by reducing application code's dependency on concrete classes.

---
class: white-slide
layout: center
---

## Parallel Class Hierarchies

<div class="span-v-4"/>

<img src="/img/dp/Factory-ShapeManipulator.png" alt="Factory Method - Shape Manipulator" class="width-80 center"/>

---

## Parallel Class Hierarchies

* Parallel class hierarchies arise when a class delegates some of its responsibilities to a separate class
* The Factory Method pattern makes it possible to connect parallel class hierarchies

```cpp
std::shared_ptr<Shape> selected_shape = get_clicked_shape();

std::shared_ptr<Manipulator> manipulator = selected_shape->create_manipulator();

manipulator->on_drag(100, 200);
```

---

# Factory Method - Implementation

* Implement the factory class as:
    - an abstract class (interface) that provides no implementation for the methods it declares
    - a concrete class that provides a default implementation of the factory method
* In a simple case, the factory interface can be replaced with a type

```cpp
using MusicServiceCreator = std::function<std::unique_ptr<MusicService>()>;
```

---

# Parameterized Factories

* The factory method accepts a parameter identifying the type of object to create
* As a result, a single factory instance can create objects of different types

---

# Parameterized Factories

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

# Parameterized Factories

* Using the factory:

```cpp
MusicServiceFactory music_service_factory;

music_service_factory.register_creator("Tidal",
    [] { return std::make_unique<TidalService>("tidal_user", "KJH8324d&df"); });

//...

std::string id_from_config = "Tidal";
MusicApp app(music_service_factory.at(id_from_config));
```

---

# Factory Method - Related Patterns

* Iterator
* Abstract Factory
    – an abstract factory is often implemented using factory methods

---

# Factory Method - Summary

* Allows one object to initiate the creation of another object when the class of the object being created is unknown
* Client code is directed toward interfaces
* Makes it possible to connect parallel class hierarchies

---
layout: cover
background: /img/header-bg.svg 
---

# Abstract Factory

---

# Abstract Factory

* Intent
    - provides an interface for creating families of related or dependent objects without specifying their concrete classes

---

# Abstract Factory - Scenario

* We want to write an application that works with multiple RDBMSs (e.g. Oracle, SQL Server, etc.)
* We define basic abstract classes that work together:
    - ``Connection`` – an object that controls the database connection
    - ``Command`` – an object representing an SQL command
    - ``Transaction`` – an object representing a transaction

---

# Abstract Factory - Scenario

* We want to avoid hard-coding concrete classes in the application while preserving consistency within an object family

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

# Abstract Factory - Context
* The system contains families of related product objects designed to be used together, and this constraint should be preserved
* The system should be independent of how its products are created, stored, and represented

---

## Abstract Factory - Problem

* We want to make it easy to configure the system with one of several product families
* Code should depend on interfaces or abstract classes
* We want to provide a library of product classes while exposing only their interfaces, not their implementations

---
class: white-slide
layout: center
---

## Abstract Factory - Structure

<div class="span-v-2"/>

<img src="/img/dp/Abstract.png" alt="Abstract Factory" class="width-50 center"/>

---

# Abstract Factory - Implementation

* The most common implementation

```cpp
class AbstractFactory
{
public:
    virtual ~AbstractFactory() = default;
    virtual std::unique_ptr<ProductA> create_product_a() = 0;
    virtual std::unique_ptr<ProductB> create_product_b(Param param) = 0;
    virtual std::unique_ptr<ProductC> create_product_c() = 0;
};
```

---

## Abstract Factory - Consequences

* Separates concrete classes
* Makes it easier to replace product families
    - the concrete factory class usually appears in the application only once
    - this makes it easy to change the concrete factory used by the application and therefore replace the product family
* Product consistency
    - product collaboration requires the application to use objects from only one family at a time
* Makes it harder to add new products

---

# Abstract Factory - Related Patterns

* Factory Method
    - AbstractFactory classes are defined using factory methods
* Singleton
    - a concrete factory instance can be implemented as a singleton

---

# Abstract Factory - Summary

* Provides an interface for creating families of related or dependent objects without specifying their concrete classes
* Product families can be exchanged easily without changing client code
* Follows the Dependency Inversion Principle (DIP)
* The most common implementation is an abstract factory interface defined as a collection of factory methods

---

# Factories - Summary

* Abstract the object creation process
* Powerful programming techniques for creating code that depends on abstractions
* Promote loose coupling by reducing application code's dependency on concrete class implementations

---
layout: cover
background: /img/header-bg.svg 
---

# Singleton

---

# Singleton

* Intent
    - guarantees that a class has only one instance and provides a global access point to that instance
    - all objects using the class use the same instance

---

# Singleton - Context / Problem

* Context
    - the system contains objects that should exist as a single instance for various reasons
* Problem
    - we want to create a class that can have only one instance available to clients

---

# Singleton - Implementation

* Guaranteeing a unique instance
    - a private constructor and copy operations removed from the interface
    - a private static member initialized with the class's only instance
    - a public static method providing access to the single instance of the Singleton class
* Lazy initialization of the class instance
* Thread-safe Singleton
    – handling simultaneous calls to the `instance` method

---

# Singleton - C++ Implementation

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

## Implementation - Using the Singleton

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

## Implementation - Using the Generic Singleton

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

# Singleton - Summary

* Singleton guarantees that only one instance of a class is created
* All objects using the class use the same instance

---
layout: cover
background: /img/header-bg.svg 
---

# Prototype

---

# Prototype

* Intent
    - specifies the kinds of objects to create using a prototype instance
    - creates new objects by copying the prototype
    - allows clients to create objects whose types are unknown to them

---

# Prototype - Scenario

* We want to write a graphics application that supports editing drawings
* Drawings are represented as aggregates of type Shape
* The abstract Shape class defines the basic operations performed by the client; client code depends on this abstraction
* We want to add the ability to copy drawings without referring to the concrete classes representing their components

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

## Code

```cpp
auto selected_shape = get_selected_shape(x, y);

std::unique_ptr<Shape> shape_copy;

if (selected_shape)
{
    shape_copy = ...;
}
```

---

## Code

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

# Prototype - Structure

<div class="span-v-4"/>

<img src="/img/dp/Prototype.png" alt="Prototype" class="width-60 center"/>

---

# Prototype in C++

We define an abstract ``clone`` method in the base class:

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

Every concrete derived class **must** implement the ``clone`` operation:

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

To avoid code duplication, we can use the CRTP idiom:

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

A derived class can use ``CloneableShape`` as its base class:

<div class="text-code-08">
```cpp
class Rectangle : public CloneableShape<Rectangle> {
public:
    //...
};
```
</div>

---

# Prototype - Consequences

* Dynamically add and remove products at runtime
    - makes it easier to add new concrete products by registering prototype instances with the client
    - a more flexible solution than factories
    - fewer subclasses
* Makes it possible to specify new prototype objects by varying their structure
    - complex structures defined at runtime can also be cloned

---

# Prototype - Related Patterns

* Abstract Factory
* Composite, Decorator, Command
    - these patterns are often used together with Prototype

---

# Prototype - Summary

* The pattern creates new objects by copying a prototype instance
* Classes whose instances are cloned can be specified at runtime
* Simplifies a factory class hierarchy comparable to the product class hierarchy

---
layout: cover
background: /img/header-bg.svg 
---

# Builder

---

# Builder - Intent

* Separates the construction of a complex object from its representation, allowing different representations to be created by the same construction process
* Defines the steps for creating a product object
    - these steps are configurable from outside (which distinguishes Builder from object factories)

---

# Builder - Context

* The object construction algorithm consists of multiple steps
* The construction process for a complex object produces different representations

---

# Builder - Problem

* We want to
    - expose the operations needed to create a complex object
    - hide the product's internal representation from the client
* We want to be able to modify individual steps of the algorithm

---
class: white-slide
layout: center
---

# Builder - Structure

<div class="span-v-4"/>

<img src="/img/dp/Builder.png" alt="Builder" class="width-70 center"/>

---
class: white-slide
layout: center
---

# Builder - Collaboration

<div class="span-v-4"/>

<img src="/img/dp/Builder_dgr_sekwencji.png" alt="Builder - Collaboration" class="width-60 center"/>

---

# Builder - Consequences

* Allows changes to the product's internal representation
* Separates construction code from representation
* Improves control over the construction process
    - unlike other creational patterns, Builder constructs objects step by step under the supervision of a Director object
* The construction process can span time
    - the Director receives the product from the Builder only after construction is complete

---

# Builder - Implementation

* An interface for assembling and constructing products
    - the Builder interface must be general enough for all concrete builders to construct products
* No abstract product class is required

---

# Builder - Related Patterns

* Abstract Factory
    - like Builder, it can construct complex objects
    - the difference is that Builder focuses on creating products step by step, while Abstract Factory focuses on product families
* Decorator, Composite
    - Builder is often used to build chains of objects

---

# Builder - Summary

* Separates the construction of complex objects from their representation
* The same construction process can produce objects with different representations
* Is often used to build composite objects (the Composite pattern)

---

# Creational Patterns - Summary

* Factory Method
* Abstract Factory
* Singleton
* Prototype
* Builder
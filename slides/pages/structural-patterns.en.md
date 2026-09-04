---
layout: cover
background: /img/header-bg.svg
---

# Structural Patterns

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

# Structural Patterns

* Structural patterns solve problems by composing classes and objects into larger structures.

---
layout: cover
background: /img/header-bg.svg 
---

# Adapter

---

# Adapter - Intent

* Converts a class's interface into an interface expected by the client
* Allows classes that previously could not work together because of incompatible interfaces to collaborate

---

# Adapter - Context / Problem

* Context
    - The interface required by the client and the interface of the class providing the implementation are incompatible

* Problem
    - We want to use an existing class, but its interface does not match the one we need

---
class: white-slide
layout: center
---

# Class Adapter

<img src="/img/gof/Adapter-Class.excalidraw.svg" alt="Class Adapter" class="width-60 center" />

---
class: white-slide
layout: center
---

# Object Adapter

<img src="/img/gof/Adapter-Object.excalidraw.svg" alt="Object Adapter" class="width-80 center" />

---
layout: two-cols
---

::left::

## Class Adapter

* Adapts by deriving from the concrete ``Adaptee`` class, so it will not work when we want to adapt the class and all of its subclasses
* Allows the ``Adapter`` class to redefine part of ``Adaptee``'s behavior
* Introduces only one object to access the adapted object

::right::

## Object Adapter

* Allows one adapter to work with multiple adaptees: the ``Adaptee`` class itself and all of its subclasses
* Makes redefining the adaptee's behavior more difficult: this requires creating subclasses of the adaptee and having the ``Adapter`` refer to them instead of to the adaptee itself (``Adaptee``)

---

# Adapter - Summary

* Changes the interface of an existing object and adapts it to the client's expectations
* Unrelated classes can collaborate despite incompatible interfaces

---
layout: cover
background: /img/header-bg.svg
---

# Decorator

---

# Decorator - Intent

* Allows new behavior to be assigned to an object dynamically
* Provides a flexible alternative to subclassing for extending functionality

---

# Decorator - Scenario

We want to extend the capabilities of a `Photo` object by adding a border and two tags to the photo.

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

Two possible implementations

</div>

---
layout: center
---

# Solution 1 - Inheritance

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


# Solution 2 - Component Decoration

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

# Component Decoration

* Place the component inside another object that adds a border.
* The object surrounding the component is called a **decorator**.
* The decorator conforms to the interface of the decorated object, making it transparent to clients.
* The decorator forwards requests to the component and can perform additional actions.

---
class: white-slide
---

# Decorator - Collaboration

* The Decorator pattern makes it possible to compose a `Photo` object with the `BorderedPhoto` and `TaggedPhoto` decorators.

<div class="span-v-4"/>

<img src="/img/dp/decorator_3.png" alt="Decorator - Collaboration" class="width-60 center"/>

---

# Decorator - Context

* Inheritance is one way to extend a class's functionality, but it is not necessarily the best way to achieve fully flexible application designs.
* An application should be designed so that the behavior of individual elements can be extended without modifying existing code.
* Composition and delegation allow new behavior to be added at runtime.
* The Decorator pattern uses a set of decorating classes (decorators) to decorate individual objects (components).

---

# Decorator - Applicability

* The Decorator pattern should be used:
    - to add responsibilities to individual objects dynamically and transparently (without affecting other objects)
    - when responsibilities can be withdrawn
    - when extending functionality by defining subclasses is impractical
    - when there may be many independent extensions whose combinations would otherwise lead to a rapid increase in the number of classes

---
class: white-slide
layout: center
---

# Decorator - Structure

<img src="/img/gof/Decorator.excalidraw.svg" alt="Decorator - Structure" class="width-70 center"/>

---
class: white-slide
---

# Decorator - Structure

<div class="span-v-4"/>

<img src="/img/gof/Decorator-Objects.excalidraw.svg" alt="Decorator - Structure" class="width-70 center"/>



---

# Decorator - Consequences [1]

* Greater flexibility than with static inheritance
    - decorators allow responsibilities to be added and removed at runtime
    - using different Decorator classes for a particular component class makes it possible to mix and match responsibilities
    - decorators also make it easy to attach a property twice (for example, a photograph with a double border)

---

# Decorator - Consequences [2]

* Avoiding overloaded classes at the top of the hierarchy
    - a simple class can be defined and its functionality extended incrementally using decorator objects
    - new types of decorators are easy to define
* The decorator and its component are not identical
    - the decorator acts as a transparent wrapper, but in terms of object identity the decorated component is not the same as the original one

---

# Decorator - Consequences [3]

* Many small objects
    - designs that use decorators often lead to systems with a large number of small, similar objects

---

# Decorator - Implementation

* Interface conformity
    - the interface of the decorator object must match the interface of the component it decorates
    - ConcreteDecorator classes must inherit from a common class
* Omitting the abstract ``Decorator`` class
    - when we only need to add one responsibility, we do not need to define an abstract Decorator class
* Keeping ``Component`` classes lightweight
    - extract the interface for the component class

---

# Decorator - Related Patterns

* Composite
    - the Decorator pattern can be viewed as a degenerate Composite with one component. Decorator adds responsibilities, however, and is not intended for aggregating objects
* Strategy
    - the Decorator pattern changes an object's skin, while Strategy changes its internals
* Builder
    - facilitates the creation of decorator chains

---

# Decorator - Summary

* Enables responsibilities to be added to an object dynamically
* Uses a set of decorating classes (decorators) to decorate individual objects (components)
* Decorators have the same interface as the objects they decorate
* Decorators change the behavior of decorated objects (components) by adding new behavior before and/or after calls to a component's methods, or even between them
* Each component can be "wrapped" by any number of decorators

---
layout: cover
background: /img/header-bg.svg
---

# Composite

---

# Composite - Intent

* Composes objects into tree structures representing part-whole hierarchies
* Lets clients treat individual objects and their aggregates uniformly

---

# Composite - Context / Problem

* Context
    - we want to represent a hierarchy of objects
    - the object hierarchy has a common base class (an abstract class or an interface)
* Problem
    - we want clients to be able to ignore the difference between aggregates and individual objects
    – clients will then treat all objects in the structure uniformly

---
class: white-slide
layout: center
---

# Composite - Structure

<div class="span-v-4"/>

<img src="/img/dp/Composite_A.png" alt="Composite - Structure" class="width-80 center"/>

---
class: white-slide
layout: center
---

# Composite - Structure (Alternative Take)

<div class="span-v-4"/>

<img src="/img/dp/Composite_B.png" alt="Composite - Structure (Alternative Take)" class="width-80 center"/>

---

# Composite - Implementation

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

# Composite - Collaboration

* Clients use the interface of the Component class to communicate with objects in the composed structure
    - if the receiver is a leaf, requests are handled directly
    - if the receiver is a composite (Composite), it usually forwards its requests to child components, possibly performing additional operations before and/or after forwarding them

---

# Composite - Consequences

* The Composite pattern defines class hierarchies for grouping primitive objects and aggregates
* Simplifying client construction
    – clients can treat composite structures and individual objects uniformly
* Placing operations for adding new components to aggregates in the Component base class may violate the Liskov substitution principle

---

# Composite - Implementation (1)

* Explicit references to parents
    - references from child components to their parents can make it easier to navigate and manage the composite structure
    - the ``Component`` class is a typical place to define a reference to the parent
    - ``Leaf`` and ``Composite`` classes can inherit this reference and the operations that manage it

---

# Composite - Implementation (2)

* Iterating over aggregate elements
    - implement the Iterator pattern for the composite structure
* Sharing components
    - sharing components is often worthwhile, for example to reduce memory requirements
    - implement the structure's objects as ``const`` (*immutable*)

---

# Composite - Implementation (3)

* Who should remove components?
    - does the composite object have ownership of its child objects?
* Which data structure is best for storing components?
    - ``std::vector<T>``
    - ``std::list<T>``
    - ``std::unordered_map<K, T>``

---

# Composite - Related Patterns

* The Composite pattern can be used to represent a group of objects in the following patterns:
    - Chain Of Responsibility
    - Visitor
* Flyweight
    - enables the implementation of component sharing
* Iterator
    - enables iteration over the entire object hierarchy

---
layout: cover
background: /img/header-bg.svg
---

# Proxy

---

# Proxy - Intent

* The Proxy pattern provides a substitute or representative for another object to control access to it

---

# Proxy - Context / Problem

* Context
    - creating and initializing objects at runtime is expensive
    - access to the object must be controlled
* Problem
    - optimization of expensive processes or access control should be transparent to the client

---

# Proxy - Scenario

* A document editor that allows graphical objects to be embedded
    - documents should open quickly
    - optimization should not affect the parts of the program responsible for displaying or formatting
* Solution
    - use another object, a drawing surrogate, to replace the real drawing
    - the proxy behaves like a drawing and creates it when necessary (**lazy loading**)

---
class: white-slide
# layout: center
---

# Proxy - Scenario - UML

<img src="/img/gof/Proxy - DocumentEditor - 1.excalidraw.svg" alt="Proxy - Scenario - UML 1" class="width-90 center"/>

---
class: white-slide
# layout: center
---

# Proxy - Scenario - UML

<img src="/img/gof/Proxy - DocumentEditor - 2.excalidraw.svg" alt="Proxy - Scenario - UML 1" class="width-90 center"/>

---
class: white-slide
# layout: center
---

# Proxy - Scenario - UML

<div class="span-v-4"/>

<img src="/img/dp/Proxy_Image_Sekwencja.png" alt="Proxy - Scenario - UML 3" class="width-40 center"/>

---

# Proxy - Applicability

* Proxy is applicable whenever a more versatile or sophisticated reference to an object is needed

---

# Proxy Kinds

* Kinds of Proxy objects
    - remote proxy – a local representative for an object in a different address space (RPC)
    - virtual proxy – creates expensive objects on demand
    - protection proxy – controls access to the original object
    - smart proxy – modifies a request before forwarding it to the original object

---
class: white-slide
# layout: center
---

# Proxy - Structure

<img src="/img/gof/Proxy.excalidraw.svg" alt="Proxy - Structure" class="width-80 center"/>

---
class: white-slide
# layout: center
---

# Proxy - Collaboration

* The surrogate object (proxy) forwards requests to the real object (subject) when necessary

<div class="span-v-4"/>

<img src="/img/gof/Proxy - Objects.excalidraw.svg" alt="Proxy - Collaboration" class="width-60 center"/>

---

# Proxy - Consequences

* The Proxy pattern introduces an additional level of indirection when accessing an object
    * Remote Proxy – can hide the fact that the object resides in another address space
    * Virtual Proxy – can perform optimizations such as on-demand object creation and copy-on-write
    * Protection Proxy and Smart Proxy – enable additional housekeeping when accessing the object

---

# Proxy - Related Patterns

* Decorator – despite a similar implementation, the purpose of the Proxy pattern is different:
    - Decorator adds responsibilities to an object
    - Proxy manages access to an object

---

# Proxy - Summary

* Provides an intermediary object that lets us optimize calls to expensive operations or control access to the original
* The Proxy object's interface is the same as the original's interface

---
layout: cover
background: /img/header-bg.svg
---

# Façade

---

# Façade

* Intent
    - provides a single, unified interface to a set of interfaces in a subsystem
    - defines a higher-level interface that makes the subsystem much easier to use
    - separates the client from complex subsystems

---
class: white-slide
---

# Façade

<div class="span-v-4"/>

<img src="/img/dp/Facade1.png" alt="Façade" class="width-80 center"/>

---
class: white-slide
---

# Façade - Structure

<div class="span-v-4"/>

<img src="/img/dp/Facade.png" alt="Façade - Structure" class="width-50 center"/>

---

# Façade - Collaboration

* Clients communicate with the subsystem by sending requests to the façade, which forwards them to the appropriate subsystem objects
* Clients using the façade do not need direct access to the objects in its subsystem

---

# Façade - Consequences

* Separates clients from subsystem components, reducing the number of objects clients must deal with and making the subsystem easier to use
* Promotes loose coupling between the subsystem and its clients, allowing components to be changed without the clients noticing
* Makes it easier to organize the system and dependencies between objects in layers
* Does not prevent applications from accessing the subsystem directly when necessary

---

# Façade - Related Patterns

* Singleton
    - usually only one façade object is needed, so façades are often implemented as singletons
* Abstract Factory
    - the Façade pattern can be used with the Abstract Factory pattern
    - this provides an interface for creating a particular family of subsystem objects

---

# Façade - Summary

* Provides an interface that hides the subsystem's complexity from the client
* Promotes loose coupling between clients and the subsystem

---
layout: cover
background: /img/header-bg.svg
---

# Bridge

---

# Bridge

* Intent
    - separates abstraction and implementation into independent class hierarchies so they can vary independently
    - provides a more flexible way to separate abstraction from implementation than inheritance

---

# Bridge - Context

* There are multiple implementations that must be supported by the design
* The client uses abstract classes to provide a uniform interface

---

# Bridge - Problem

* We want to avoid permanently binding an abstraction to its implementation
    – the implementation can be selected or changed at runtime
* We expect changes both in the abstraction's interface and in its implementations
    - changes in the abstraction's implementation should not affect clients
* We want to hide the abstraction's implementation completely from clients

---

# Bridge - Scenario

---
layout: center
---

<div class="text-2">

Design using inheritance to support multiple implementations

</div>

---
class: white-slide
layout: center
---

<img src="/img/gof/Bridge - Shapes - 1.excalidraw.svg" alt="Bridge - DrawingAPI Before - 1" class="center"/>

---
class: white-slide
layout: center
---

<img src="/img/gof/Bridge - Shapes - 2.excalidraw.svg" alt="Bridge - DrawingAPI Before - 2" class="center"/>

---
class: white-slide
layout: center
---

<img src="/img/gof/Bridge - Shapes - 3.excalidraw.svg" alt="Bridge - DrawingAPI Before - 3" class="center"/>

---
layout: center
---

<div class="text-2">

Design using the Bridge pattern

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

# Bridge - Consequences

* Separates the implementation, removing its permanent binding to the interface
    - the abstraction's implementation can be selected at runtime
    - eliminates compile-time dependence on a particular implementation
* Easier extensibility
    – the Abstraction and Implementation hierarchies can be extended independently
    - changes in concrete abstraction classes do not affect the client
* Useful in graphics and windowing systems that must run on different platforms
* Increases design complexity

---

# Bridge - Special Case: PIMPL

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

# Flyweight - Scenario

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

# Flyweight - Context / Problem

* Context
    - the design contains a very large number of objects
    - the cost of storing these objects in memory is significant

* Problem
    - we want to reduce the cost of storing objects in memory by sharing objects as flyweights

---

# Flyweight Object

* Flyweight object
    - a shared object that can be used simultaneously in multiple contexts
    - acts as an independent object in each context by distinguishing between **intrinsic** and **extrinsic** state

---

# Flyweight Object

* **Intrinsic state**
    – is stored in the flyweight and consists of information independent of the context
* **Extrinsic state**
    – depends on the context and changes with it, so it cannot be shared
* Flyweights model concepts or entities that are usually too numerous to represent as fully independent objects

---

# Flyweight - Object Sharing

* Once extrinsic state is removed, many groups of objects can be replaced with a relatively small number of shared objects

---
class: white-slide
---

# Flyweight - Structure

<img src="/img/gof/Flyweight.excalidraw.svg" alt="Flyweight - Structure" class="width-80 center"/>

---

# Flyweight - Consequences

* Reduces memory usage at the cost of increased execution time
* Memory savings depend on several factors:
    - the reduction in the total number of instances resulting from sharing
    - the size of the intrinsic state per object
    - whether the extrinsic state is computed or stored

---

# Flyweight - Summary

* Uses object sharing to handle a large number of objects efficiently
* Reduces memory usage at the cost of increased execution time
* The effectiveness of a flyweight depends on how much information can be shared as the flyweight's intrinsic state

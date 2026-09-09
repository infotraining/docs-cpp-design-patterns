---
layout: cover
background: /img/header-bg.svg
---

# Behavioral Patterns

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

# Behavioral Patterns

* Concern algorithms and the assignment of responsibilities to objects
* Characterize complex control flows between objects that are difficult to trace during program execution
* Are used to organize, manage, and combine behaviors

---
layout: cover
background: /img/header-bg.svg 
---

# Template Method

---

# Template Method

* Purpose
    - defines the skeleton of an algorithm, deferring the implementation of some algorithm steps to derived classes
    - the template method defines the order of steps in an algorithm while allowing derived classes to change their implementation as needed

---

# Template Method - Context / Problem

* Context
    - an algorithm exists whose individual steps may require different implementations
* Problem
    - we want to implement the invariant part of an algorithm once and leave behavior that may change to the derived classes

---
class: white-slide
---

# Template Method - Structure

<div class="span-v-2"/>

<img src="/img/gof/TemplateMethod.excalidraw.svg" alt="Template Method" class="width-70 center">

---

# Template Method - Consequences

* Template methods are a fundamental technique for ensuring code reuse
* Template methods result in an inverted control structure: the base class calls operations of the derived class, rather than the other way around

---

# Template Method - Consequences

Template methods call:

- concrete operations from ``ConcreteClass``, ``AbstractClass``, or client classes
- abstract operations
- factory methods
- **hook methods**, which provide default behavior that can be extended by derived classes

---

# Template Method - Related Patterns

* Strategy
    - both Strategy and Template Method encapsulate algorithms, using composition and inheritance, respectively
* Factory Method
    - Template Method often calls factory methods

---

# Template Method

* Defines the skeleton of an algorithm in a particular method, deferring the implementation of some algorithm steps to subclasses
* Allows subclasses to redefine certain algorithm steps while preventing them from changing its structure
* Follows the so-called Hollywood Principle
    - decision-making should be placed in high-level modules, which can decide for themselves how and when to call low-level modules

---
layout: cover
background: /img/header-bg.svg 
---

# Strategy

---

# Strategy

* Purpose
    - defines families of algorithms
    - encapsulates them and makes them interchangeable
    - allows an algorithm to be modified independently of the client that uses it

---

# Strategy - Context / Problem

* Context
    - different variants of an algorithm are needed
    - several related classes differ only in their behavior
    - the algorithm uses data that the client should not know about
    - a class defines many behaviors that appear in its operations as repeated conditional statements
* Problem
    - we want to encapsulate an algorithm in a class and use composition to make its implementation interchangeable

---
class: white-slide
---

# Strategy - Structure

<div class="span-v-3"/>

<img src="/img/gof/Strategy.excalidraw.svg" alt="Strategy" class="width-90 center">

---

# Strategy - Consequences

* Encapsulating an algorithm in separate classes makes it possible to modify its implementation independently of the context
* Strategies eliminate conditional statements
    - an alternative to using conditional statements to select the desired behavior
* Clients must be aware of the different strategies and the differences between them, which is a potential drawback
* Increased number of objects

---

# Strategy - Implementation (1)

* Defining the strategy (Strategy) and context (Context) interfaces
    - Context passes data to the Strategy operation as arguments
    - Context passes itself as an argument, and the strategy explicitly requests data from the context
* The context class is easier to use when using it without a strategy object makes sense
    - a default strategy implementation means clients do not need to deal with strategy objects

---

# Strategy - Implementation (2)

* If strategies are simple functions, encapsulation in a separate class can be replaced with ``std::function``

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

# Strategy - Summary

* A strategy encapsulates an algorithm as an object
* A program using the Strategy pattern can offer multiple versions of an algorithm or behavior
* Object behavior can change dynamically during program execution
* Delegating behavior to a separate object with a defined interface produces highly cohesive classes
    - better compliance with SRP

---
layout: cover
background: /img/header-bg.svg 
---

# State

---

# State

* Enables an object to change its behavior when its internal state changes

---

# State - Context / Problem

* Context
    - an object must change its behavior during program execution depending on its state
    - operations contain large, multi-part conditional statements that depend on the object's state; the State pattern moves each conditional branch to a separate class
* Problem
    - we want an object to change its behavior when its internal state changes by encapsulating the state as a class

---
class: white-slide
---

# State - Structure

<div class="span-v-2"/>

<img src="/img/gof/State.excalidraw.svg" alt="State" class="width-80 center">

---

# State - Consequences

* Localizes state-specific behavior and separates behavior for different states
    - the code for each state is in a separate class, making it easier to add new states without extensive modifications to existing code
* Eliminates the need to divide method code into blocks for individual states (switch blocks)
* Makes transitions between states explicit
    - from the context's perspective, state transitions are atomic because they occur by replacing a single state object

---

# State - Implementation

* Possibility of sharing State objects
    - if State objects are *immutable*, contexts can share them
* Which participant defines transitions between states?

---

# State - Related Patterns

* Strategy
    - similar UML structure
    - the client always decides when behavior changes
* Flyweight
    – used when objects representing a state can be shared

---

# State - Summary

* The State design pattern describes a situation in which an object's behavior is determined by an internal state that can change in response to events
* Provides better scalability for logic related to managing an object's states
* Encapsulating states in classes localizes future changes in the code

---
layout: cover
background: /img/header-bg.svg
---

# Chain Of Responsibility

---

# Chain Of Responsibility

* Avoids coupling the request sender to its receiver by giving more than one object a chance to handle the request
* A request is passed along a chain of objects until one of them handles it
* The object that generated the request does not know who will handle it; the request has an **implicit receiver**
* To pass requests along the chain while keeping receivers implicit, each object in the chain uses a common interface for handling requests and accessing the next object in the chain

---

# Chain of Responsibility - Context / Problem

* Context
    - more than one object may handle a request, and the handling object is not known in advance
    - request execution is not guaranteed
    - the set of objects that can handle a request may be specified dynamically
* Problem
    - we want to send a request to one of several objects without explicitly specifying its receiver
    - we want to decouple the request sender from its receivers

---
class: white-slide
---

# Chain of Responsibility - Structure

<img src="/img/gof/ChainOfResponsibility.excalidraw.svg" alt="Chain of Responsibility" class="width-70 center">

---

# Chain of Responsibility - Consequences

* Reduced coupling
    - an object does not need to know which other object will handle a request
* Additional flexibility in assigning responsibilities to objects
* No guarantee that a request will be handled
    - because the request receiver is not explicitly known, there is no guarantee that it will be handled; the request may leave the chain without being handled at all

---

# Chain of Responsibility - Implementation

* Implementing the chain of successors:
    - defining new relationships
    - using existing relationships

---

# Chain of Responsibility - Related Patterns

* Composite
    - often used together with Chain of Responsibility
    - a component's parent can act as its successor

---

# Chain of Responsibility - Summary

* Separates the request sender from its receivers
* Request execution is not guaranteed
* Commonly used in windowing systems to handle events
    - for example, a mouse click or key press

---
layout: cover
background: /img/header-bg.svg
---

# Observer

---

# Observer

* Defines a one-to-many dependency between objects
* When one object changes state, all objects that depend on it are automatically notified and updated

---

# Observer - Context

* Context
    - changing the state of one object requires changing others, and the number of objects that must change is unknown
* Problem
    - an object should be able to notify other objects without making assumptions about what those objects represent, resulting in looser coupling between objects

---
class: white-slide
---

# Observer - Structure

<img src="/img/gof/Observer.excalidraw.svg" alt="Observer" class="width-70 center">

---

# Observer - Consequences

* An abstract coupling between the **Subject** and the **Observer**
    - they may belong to different abstraction layers in the system
* Support for broadcasting messages
    - a notification is automatically sent to all interested objects that have subscribed to it
* Unexpected updates
    - an apparently harmless state-changing operation on the **Subject** may trigger a cascade of updates in the **Observers** and the objects that depend on them

---

# Observer - Implementation

* Observing more than one **Subject**
    - the **Subject** can simply pass itself as an argument to the ``Update()`` operation, thereby telling the **Observer** which object to inspect
* Sending change information to the **Observer**, using two models:
    - **push model** – `Subject` sends detailed information about the change, whether or not observer objects want it
    - **pull model** – `Subject` sends nothing beyond the notification, and observers explicitly ask for the details afterward

---

# Observer - Summary

* Enables objects to register dependencies dynamically, allowing them to notify dependent objects about significant changes in their state
* Observing objects are loosely coupled to the observed object
* Information about changes to the observed object's state can be pushed or pulled

---
layout: cover
background: /img/header-bg.svg
---

# Command

---

# Command

* Encapsulates requests as objects
* Decouples the object that issues a request from the object that knows how to carry it out
* Enables
    - parameterizing clients with different requests
    - queuing requests and logging them
* Simplifies the implementation of undoing operations
* Encapsulates a receiver (the implementing object) with an operation or a series of operations to be performed

---

# Command - Context / Problem

* Use the Command pattern when we want to:
    - parameterize an object with an action; Command is an object-oriented implementation of callbacks (**callbacks**)
    - support undoing introduced changes
    - log changes so they can be replayed after a system failure
    - apply transaction semantics; a transaction encapsulates a set of data changes in the system

---
class: white-slide
---

# Command - Problem

<img src="/img/gof/Command-Before.excalidraw.svg" alt="Command Before" class="width-50 center">   

---
class: white-slide
---

# Command - Structure

<img src="/img/gof/Command.excalidraw.svg" alt="Command" class="width-60 center">

---

# Command - Consequences

* The Command pattern separates the object that invoked an operation from the object that knows how to perform it
    – separates the invoker's interface from the receiver's
* Commands are objects
    – they can be processed and extended like other objects
* New commands can be added easily because this does not require modifying existing classes

---

# Command - Implementation

* Commands can support undoing and restoring operations if they provide the appropriate tools, such as an ``Undo`` or ``Redo`` operation
* The ConcreteCommand class may need to retain additional state for this purpose
* This state may contain:
    - the receiver object (Receiver), which actually performs operations in response to a request
    - the arguments for operations performed by the receiver
    - all initial values in the receiver that may change as a result of handling the request; the receiver must provide operations that restore it to its previous state

---

# Command - Related Patterns

* Composite
    – this pattern can be used to implement macro commands
* Prototype
    – a command that must be copied before being placed on the command list acts as a Prototype
* Memento
    – a pattern often used to implement undo operations

---

# Command - Summary

* Separates the object requesting an operation from the object that knows how to perform it
* Simplifies queuing, selecting, and controlling the execution time of commands
* Simplifies undoing and redoing commands
* Simplifies maintaining a persistent history of executed commands

---
layout: cover
background: /img/header-bg.svg
---

# Memento

---

# Memento

* Stores and exposes an object's internal state without violating encapsulation, so that the object can later be restored to the saved state
* A Memento is an object that stores a snapshot of another object's internal state, its originator

---

# Memento - Problem / Context

* There is a need to save a snapshot of an object's state, or part of its state, so that it can later be restored
* A direct interface for obtaining the state would expose implementation details and violate the object's encapsulation

---
class: white-slide
---

# Memento - Structure

<div class="span-v-4"/>

<img src="/img/gof/Memento.excalidraw.svg" alt="Memento" class="width-80 center">

---

# Memento - Summary

* Saves and exposes an object's internal state without violating encapsulation
* A Memento allows an object to be restored to its previous state

---
layout: cover
background: /img/header-bg.svg
---

# Mediator

---

# Mediator

* Defines an object that encapsulates information about how objects interact
* Loosens coupling between objects because they do not refer to one another directly
* Separates collaborating objects from the system

---

# Mediator - Applicability

* A set of objects communicates in a well-defined but complex way
    - the resulting dependencies are disorganized and difficult to understand
* Reusing an object is difficult because it refers to and communicates with many other objects
* Behavior implementation is distributed across many classes; changing it requires creating many derived classes

---
class: white-slide
---

# Mediator - Structure

<img src="/img/gof/Mediator.excalidraw.svg" alt="Mediator" class="width-60 center">

---

# Mediator - Summary

* Encapsulates the communication process between objects
* Defines loose coupling between objects
* Enables easy reimplementation of how ``Coleague`` objects collaborate without defining derived classes for them

---
layout: cover
background: /img/header-bg.svg
---

# Visitor

---

# Visitor

* Represents an operation to be performed on the elements of an object structure
* Enables defining a new operation without modifying the classes of the elements on which it operates

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

# Visitor - Context / Problem

* Context
    - classes defining the object structure rarely change, but we want to define new operations on that structure frequently
* Problem
    - we want to make it easy to add a new operation to an object structure without opening the classes in that structure

---
class: white-slide
---

## Visitor - Structure

<div class="span-v-2"/>

<img src="/img/dp/Visitor.png" alt="Visitor" class="width-60 center">

---

# Visitor - Consequences

* Easy addition of new operations
    - visitors make it easier to add operations that depend on components of composite objects
    - a new operation on an object structure is defined simply by adding a new visitor (Visitor)
* Gathers related operations together and separates unrelated ones
    - related behavior is not scattered across all classes defining the object structure; it is located in the Visitor class
    - this simplifies both the classes defining the elements and the algorithms defined in visitors
* Adding new classes to the visited hierarchy is difficult

---

# Visitor - Consequences

* Visiting an entire class hierarchy
    - a visitor can visit objects that do not have a common parent
    - the Visitor interface can include operations for objects of any type
* Accumulating state
    - visitors can accumulate state while visiting individual elements of the object structure
    - without the pattern, this state would be passed as additional arguments to traversal operations or could appear as global variables

---

# Visitor - Implementation

* Using the CRTP idiom

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

# Visitor - Related Patterns

* Composite
    - visitors can be used to perform an operation on all elements of an object structure defined by the Composite pattern
* Interpreter
    - the Visitor pattern can be used to interpret an AST

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


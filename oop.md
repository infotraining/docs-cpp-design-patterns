# Foundations of SOLID Object-Oriented Programming

## Four tenets of object-oriented programming

Object-oriented programming (OOP) is a programming paradigm that uses "objects" to design applications and computer programs.

It utilizes several techniques to design and develop software. The four major principles of object-oriented programming are:

1. **Abstraction**

    * Abstraction is the concept of hiding the complex implementation details and showing only the necessary features of the object. It helps to reduce programming complexity and effort.

2. **Encapsulation**

    * Encapsulation (aka *Information Hiding*) is the process of bundling the **data** (variables) and the **methods** (functions) that operate on the data into a single unit called a class. All internal details of the class implementation are hidden from the outside world, and only the necessary interface that allows to operate on an object is exposed to the user.

3. **Polymorphism**

    * It allows to perform a single action in different ways. In other words, polymorphism allows to define one interface and have multiple implementations. Objects that have the same interface can be used interchangeably.

4. **Code Reusability: Composition & Inheritance**

    Composition and inheritance are two ways to achieve code reusability in object-oriented programming.

    * **Composition** is a "has-a" relationship between two classes. It allows creating complex types by combining objects of other types.

    * **Inheritance** is an "is-a" relationship between two classes. It allows creating a new class that inherits attributes and methods from an existing class.

## Object - Interface - Class

### Object

An object is a real-world entity that has a state and behavior. It can be a physical entity or a conceptual entity. An object has three main characteristics:

1. **State**: It represents the data (attributes) of an object.

2. **Behavior**: It represents the methods (functions) that operate on the object's state.

3. **Identity**: It gives a unique name to an object and enables one object to interact with another object.

In object oriented software objects communicate with each other using methods. 

### Interface

An interface is a public contract that defines the methods that a class must implement. It specifies what a class must do but not how it does it. Interfaces are used to define the behavior of an object.

Interface is separated from the objects implementation. Many objects that have the same interface can vary in their implementation.

### Class

Class defines the implementation for an object. It is a blueprint for creating objects. It defines the attributes and methods that an object will have.

Class allows to instantiate objects. An object is an instance of a class.

#### Abstract Class

An abstract class is a class that cannot be instantiated. 

It is used to define a common interface for all the subclasses. It allows to define **abstract methods** that must be implemented by the subclasses.

Abstract methods are methods that are declared in the abstract class but do not have an implementation. In C++ they are declared as **pure virtual functions**.

Class that contains at least one pure virtual function is called an abstract class.

Class that implements all the pure virtual functions of an abstract class is called a **concrete class** and can be instantiated.

![Abstract Class](img/abstract-class.png)
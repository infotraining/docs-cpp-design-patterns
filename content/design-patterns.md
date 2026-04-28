# Design Patterns

> Each pattern describes a problem which occurs over and over again in our environment, and then describes the core of the solution to that problem, in such a way that you can use this solution a million times over, without ever doing it the same way twice.
>
> -- Christopher Alexander, "A Pattern Language: Towns, Buildings, Construction"

> A description of communicating objects and classes that are customized to solve a general design problem in a particular context.
>
> -- Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides, "Design Patterns: Elements of Reusable Object-Oriented Software"

## Design Pattern

A Design Pattern is a general, reusable solution to a commonly occurring problem within a given context in software design. They are designed to help you write code that is easier to understand, maintain, and extend.

### Key Aspects of Design Patterns

* **Reusable Solutions:**

    Design patterns provide a proven solution that can be applied across different scenarios and projects.

* **Contextual:**

    They are not one-size-fits-all but are applied to specific problems within a certain context, providing flexibility in implementation.

* **Best Practices:**

    Patterns are developed from the experience of many developers over time, representing best practices for solving particular problems.

## Elements of a Design Pattern

A design pattern has **four essential elements**:

1. **Pattern Name** – A shorthand that can be used to concisely describe the design problem, its solution, and consequences. It allows designing at a higher level of abstraction.
2. **Context/Problem** – Specifies when to apply the pattern.
3. **Solution** – Describes the elements that make up the solution to the defined problem, their relationships, responsibilities, and collaborations. It does not describe a specific design or implementation.
4. **Consequences** – The advantages and disadvantages of using the pattern.

A pattern provides an abstract description of a design problem and how a general arrangement of elements (classes and objects) solves that problem.

## Pros & Cons of Design Patterns

### Pros of Design Patterns

1. **Reusability:**

   Design patterns provide proven solutions to common problems, promoting code reuse and reducing redundancy.

2. **Efficiency:**
  
    Using established patterns can speed up development as developers don't need to solve problems from scratch.

3. **Consistency**

    Patterns ensure consistency in code structure, making it easier for multiple developers to work on the same codebase.

4. **Communication:**

    Design patterns provide a **common vocabulary for developers**, simplifying discussions about software design and architecture.

5. **Maintainability:**

    Well-defined patterns can make code easier to understand and maintain, as the structure and purpose of the code are clearer.

6. **Scalability:**

    Patterns can provide scalable solutions that are flexible enough to handle growth and changes in requirements.

### Cons of Design Patterns

1. **Overhead:**

    Implementing patterns can sometimes introduce additional layers of complexity, making the codebase harder to understand for newcomers.

2. **Overuse:**

    There's a risk of overusing design patterns, applying them even when simpler solutions would suffice, leading to over-engineering.

3. **Learning Curve:**

    Understanding and correctly implementing design patterns requires a certain level of expertise and experience, which can be a barrier for novice developers.

4. **Misapplication:**

    Patterns can be misapplied to problems they weren't designed to solve, resulting in inefficient or convoluted code.

5. **Maintenance Challenges:**

    While patterns can enhance maintainability, they can also make code harder to modify if the pattern isn't well understood or if the requirements change significantly.

6. **Performance:**

Some patterns can introduce performance overhead due to additional abstraction layers, which might not be suitable for high-performance applications.

## Categories of Design Patterns

Design patterns are often categorized into three main types:

| **Creational Patterns** | **Structural Patterns** | **Behavioral Patterns** |
| ----------------------- | ----------------------- | ----------------------- |
| Factory Method          | Adapter                 | Template Method         |
| Abstract Factory        | Decorator               | Strategy                |
| Prototype               | Composite               | State                   |
| Bridge                  | Proxy                   | Observer                |
| Singleton               | Facade                  | Chain of Responsibility |
| Builder                 | Bridge                  | Command                 |
|                         | Flyweight               | Memento                 |
|                         |                         | Mediator                |
|                         |                         | Visitor                 |
|                         |                         | Interpreter             |
|                         |                         | Iterator                |
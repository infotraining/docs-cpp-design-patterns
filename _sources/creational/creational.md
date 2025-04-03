# Creational Patterns

Direct object creation is not always the best approach for instantiating objects. In such cases we have to deal with the complexity of object creation (organize dependencies and pass them to the constructor). This makes the code strongly coupled with its dependencies. Creational design patterns provide an abstract way to create objects while hiding the creation logic, rather than instantiating objects directly using constructor.

The main goal of creational design patterns is to provide better alternatives for situations where a direct object creation is not convenient. These patterns provide an abstraction layer that hides the actual object creation process. This can help improve the flexibility of the code.

## Pros of Creational Patterns

1. **Encapsulation:**

    Creational patterns encapsulate the object creation process, making it easier to change the object creation process without affecting the client code.

2. **Flexibility:**

    Creational patterns provide a way to create objects in a controlled manner, allowing you to change the object creation process at runtime.

3. **Reusability:**

    Creational patterns promote code reuse by providing a generic way to create objects.

4. **Decoupling:**

    Creational patterns help decouple the client code from the object creation process, making it easier to maintain and test the code.
# Interface Segregation Principle

```{important}
A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.
```

Instead of creating large, monolithic interfaces with many methods, you should design smaller, more specific interfaces tailored to the exact needs of the clients (classes) that use them. This keeps the design clean, avoids unnecessary dependencies, and makes the code easier to maintain and extend.

## Example of ISP

Let's consider a following diagram of classes:

```{image} ../img/solid/ISP-Before.png
:width: 700px
:align: center
```

In this example the user of the `Door` instance is forced to know about `Timer` interface and its `timeout()` method. 

The better solution is to create a separate interface for the `Door` class that only includes the methods that are relevant to the user of the `Door` instance. Then we can use multiple inheritance to create `TimedDoor` class that implements both `Door` and `Timer` interfaces. But now we can pass the `TimedDoor` instance through the `Door` interface without needing to know about the `Timer` interface. The same object can be passed to the `TimerUser` using the `Timer` interface. 

```{image} ../img/solid/ISP-After.svg
:width: 900px
:align: center
```


## Benefits of ISP

- **Prevents Code Smells**: Large interfaces can lead to "fat interface" problems, where classes implement methods they don’t actually need. This results in unused or meaningless code.
- **Improves Flexibility**: Smaller, focused interfaces are easier to implement and less likely to break existing code when modified.
- **Promotes Modularity**: Clear, specific interfaces lead to more modular and reusable components.

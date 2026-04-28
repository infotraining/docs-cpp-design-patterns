# Dependency Inversion Principle

```{important}
1. **High-level modules** should not depend on **low-level modules**. Both should depend on **abstractions** (e.g., interfaces).

2. **Abstractions** should not depend on details. Details (concrete implementations) should depend on abstractions.
```

Instead of high-level code (e.g., application logic) being tightly coupled to low-level code (e.g., specific implementations), both should rely on an abstraction (like an interface or abstract class). This helps reduce dependency between components and makes the system more adaptable to changes.

## Example of DIP

Let's consider a simple example when using a button we want to turn on a LED indicator. 

```{image} ../img/solid/dip-before.png
:width: 500px
:align: center
```

When `ToggleButton` uses the `LEDLight` class directly to turn on the light, it creates a tight coupling between the two classes. If we want to change the LED implementation (e.g., to a different type of light), we would need to modify the `ToggleButton` class. 

This kind of design violates the Dependency Inversion Principle:

1. The high-level module (`ToggleButton`) depends on a low-level module (`LEDLight`). When we want to turn on the light we need to directly call `set_rgb(255, 255, 255)` method of the `LEDLight` class. We push the knowledge how the low-level module works into the high-level module (we need to know what is RGB color space in order to use the LED light properly). 
2. If we are forced to use different LED light implementations, we need to modify the `ToggleButton` class. Thus changes in the low-level module require updates in the high-level module code.

To comply with the Dependency Inversion Principle, we can introduce an interface called `ISwitch` that defines the methods for turning on and off the light.

```{image} ../img/solid/dip-after.png
:width: 800px
:align: center
```

The `ISwitch` interface defines the methods `on()` and `off()` that the `ToggleButton` class can use without knowing the details of how the light works. This interface reflects what high-level module needs to operate. The `LEDSwitch` class implements this interface and provides the specific implementation for turning on and off the LED light. This is a part of a low-level module that depends on the `ISwitch` interface. Now both high-level and low-level modules depend on the abstraction (`ISwitch` interface), not on each other. The `LEDSwitch` works as an adapter that adapts the `LEDLight` class to the `ISwitch` interface created for a high-level module.

Future changes in the low-level module (e.g., changing the LED light implementation) will not affect the high-level module (`ToggleButton` class). The `ToggleButton` class can work with any implementation of the `ISwitch` interface, making it flexible and adaptable to changes.

## Benefits of DIP

- **Encourages the use of abstractions**: Promotes flexibility and reusability by decoupling high-level and low-level modules.
- **Encourages loose coupling between components**: Makes your application easier to test and maintain.
- **Facilitates separation of concerns across architectural layers**: Aligns with best practices for clean architecture.




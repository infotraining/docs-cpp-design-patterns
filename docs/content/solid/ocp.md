# Open/Closed Principle

```{important}
Software entities (such as classes, modules, or functions) should be open for extension but closed for modification
```

What This Means:

- **"Open for extension"** means you should be able to add new functionality to a class or module without altering its existing code.
- **"Closed for modification"** means that once a class is written and tested, its behavior should not be changed.

If a class has specific behavior that works correctly, modifications to its functioning code should be avoided. To extend the code (add new functionality), a new class should be created using inheritance, where selected methods can be overridden and adapted to current requirements. While the class code cannot be modified (Closed Principle), it remains open to extension (Open Principle).

Introducing new functionality should not require changes to the code of the clients using it.

Classes open to extension should include **extension points** that allow new functionality to integrate seamlessly into the existing code. These extension points can be defined by **extracting an interface** for specific functionality.

## Strong Coupling vs. Loose Coupling

**Strong coupling** refers to a situation where classes or modules are tightly interconnected, meaning that changes in one class can significantly impact others. This can lead to a fragile system where modifications in one part can cause unexpected behavior in another.

**Loose coupling**, on the other hand, refers to a design where classes or modules are independent of each other. Changes in one class have minimal or no impact on others. This is achieved through the use of interfaces, abstract classes, and dependency injection.

Let's consider an example:

```{image} ../img/solid/OCP-Before.png
:width: 600px
:align: center
```

In this case, the specific `Client` class uses a specific `Server` class. These classes are strongly coupled.

If we wanted the `Client` class object to use an object from a different server class, we would need to change the name of the server class being used within the `Client` class.

This project can be transformed to comply with the Open/Closed Principle (OCP) by applying the Strategy Design Pattern. We can extract an interface for the functionality called `InterfaceForClient`, which is used by the `Client` class. 

```{image} ../img/solid/OCP-After.png
:width: 600px
:align: center
```

Through this newly created extension point, we can substitute various implementations of the interface without needing to modify the client's code. Introducing new functionality in this setup involves writing a new class implementing the `InterfaceForClient`. The implementation of the `Client` class itself remains unchanged.

Now classes are loosely coupled, meaning that changes in one class do not affect others. This makes the system more flexible and easier to maintain.

The Open/Closed Principle (OCP) combines the concepts of encapsulation and interface extraction. It involves identifying consistent behaviors, extracting an interface, and using subclasses to handle new or changed behaviors.

## Example of OCP

Let's consider a simple example of a `UserService` class that sends notifications to users. Initially, it only supports email and SMS notifications:

```cpp
class UserService
{
    enum class NotificationType
    {
        Email,
        SMS
    };
public:
    void notify_user(const User& user)
    {
        if (notification_type_ == NotificationType::Email)
        {
            // sending email to a user
        }
        else if (notification_type_ == NotificationType::SMS)
        {
            // sending SMS to a user
        }
    }

    void set_notification_type(NotificationType type)
    {
        notification_type_ = type;
    }
private:
    NotificationType notification_type_ = NotificationType::Email;
};
```

In this example, the `UserService` class is responsible for sending notifications to users. However, if we want to add support for a new notification type (e.g., push notifications), we would need to modify the existing code, which violates the OCP.

To adhere to the OCP, we can refactor the code by introducing an interface for notifications:

```cpp
class NotificationService
{
public:
    virtual ~NotificationService() = default;
    virtual void notify(const User& user) = 0;
};

class EmailNotificationService : public NotificationService
{
public:
    void notify(const User& user) override
    {
        // sending email to a user
    }
};

class SMSNotificationService : public NotificationService
{
public:
    void notify(const User& user) override
    {
        // sending SMS to a user
    }
};
```

Now, we can create a new class for push notifications without modifying the existing code:

```cpp
class PushNotificationService : public NotificationService
{
public:
    void notify(const User& user) override
    {
        // sending push notification to a user
    }
};
```

Now, the `UserService` class can use any notification service without needing to know the details of how each service works:

```cpp
class UserService
{
    NotificationService& notification_service_;
public:
    explicit UserService(NotificationService& service) noexcept : notification_service_(service) {}

    void notify_user(const User& user)
    {
        notification_service_.notify(user);
    }
};
```

This example is a simple illustration of how to apply the Open/Closed Principle. By using interfaces and polymorphism, we can extend the functionality of our code without modifying existing classes. This makes our code more maintainable and flexible, allowing us to add new features with minimal impact on existing code. 

It is also an example of **Strategy Design Pattern**, where the `UserService` class uses a strategy (notification service) to perform its task. The strategy can be changed at runtime by passing different implementations of the `NotificationService` interface.


## Benefits of OCP

- **Fewer bugs**: Reduces the likelihood of introducing bugs when extending functionality.
- **Extendable**: Promotes flexibility and scalability.
- **Loose coupling**: Encourages the use of abstraction and polymorphism, which improves maintainability by reducing dependencies between components. This allows changes in one part of the system to have minimal impact on others, making the codebase easier to understand and modify.

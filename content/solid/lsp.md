# Liskov Substitution Principle

```{important}
Objects in a program should be replaceable with instances of their subtypes without altering the correctness of the program.
```

If `B` is a subclass of `A`, you should be able to use an instance of `B` wherever an instance of `A` is expected, without the program breaking. In other words, derived classes must behave in a way that is consistent with the expectations set by their base classes.

## Example of LSP

Let's consider a simple example with a `Rectangle` class and a `Square` class that inherits from it:

```cpp
class Rectangle
{   
    size_t width_;
    size_t height_;
public:
    Rectangle(size_t width, size_t height) noexcept : width_(width), height_(height) {}
    virtual ~Rectangle() = default;	

    virtual size_t width() const { return width_; }
    virtual size_t height() const { return height_; }

    virtual void set_width(size_t width) { width_ = width; }
    virtual void set_height(size_t height) { height_ = height; }
};

class Square : public Rectangle
{
public:
    explicit Square(size_t size) : Rectangle(size, size) {}

    void set_width(size_t width) override
    {
        Rectangle::set_width(width);
        Rectangle::set_height(width);
    }

    void set_height(size_t height) override
    {
        Rectangle::set_width(height);
        Rectangle::set_height(height);
    }
};
```

The problem with this design that `Rectangle` has in its interface methods to set independently the width and height. Inheriting from `Rectangle` forces `Square` to override these methods to maintain the invariant that a square has equal width and height. This violates the LSP because substituting `Square` for `Rectangle` can lead to unexpected behavior for the client. 

```cpp
void process_rectangle(Rectangle& rect)
{
    rect.set_width(5);
    rect.set_height(10);
    size_t area = rect.width() * rect.height();
    assert(area == 50); // This assertion will fail if TRectangle is Square
}

Rectangle rect(2, 3);
process_rectangle(rect);   // OK

Square square(2);
process_rectangle(square); // Fails, as the area will not be 50
```

## Design by Contracts

**Design by Contract (DbC)**, is a software design methodology where classes and methods come with clearly defined *"contracts"*. These contracts define precise agreements between a method's caller and the method itself, specifying:
- **Preconditions**: Conditions that must be true before a method or function is executed.
- **Postconditions**: Conditions that must be true after the execution of the method.
- **Invariants**: Conditions that must always remain true throughout the lifecycle of an object.

The Liskov Substitution Principle (LSP) complements DbC by ensuring that subclasses adhere to their base class's contracts. Together, DbC and LSP promote robust, reusable, and correct code.

### LSP in the Context of Contracts

The LSP requires that a derived class remains substitutable for its base class. In the context of DbC, this means:
- **Preconditions**: A derived class must not strengthen preconditions (i.e., it cannot impose stricter requirements than the base class).
- **Postconditions**: A derived class must honor the postconditions of the base class and can strengthen them (i.e., guarantee stronger results if needed). The derived class cannot weaken the postconditions (promises) made by the base class.
- **Invariants**: The derived class must maintain the invariants defined by the base class.

## Benefits of LSP

- Promotes robust inheritance hierarchies.
- Ensures code remains extensible and reusable.
- Helps avoid unexpected behavior or runtime errors.



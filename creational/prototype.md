# Prototype

## Intent

The **Prototype** pattern is a creational design pattern that allows cloning objects, even complex ones, without coupling to their specific classes. It's useful when you want to create a new object by copying an existing object, and you don't know the exact type of the object you want to copy.

## Example - from problem to solution

Let's say we are developing a drawing application that allows users to create and manipulate shapes. The application should support different types of shapes, such as circles, squares, and triangles. Each shape has its own properties and methods for drawing and manipulating it. The hierarchy of shapes is represented by the following classes:

```cpp
class Shape
{
public:
    virtual ~Shape() = default;
    virtual void draw() = 0;
    virtual void move(int dx, int dy) = 0;
};

class Circle : public Shape
{
public:
    Circle(int x, int y, int radius) : x_(x), y_(y), radius_(radius) {}
    void draw() override { /* draw circle */ }
    void move(int dx, int dy) override { x_ += dx; y_ += dy; }
private:
    int x_, y_, radius_;
};

class Square : public Shape
{
public:
    Square(int x, int y, int side) : x_(x), y_(y), side_(side) {}
    void draw() override { /* draw square */ }
    void move(int dx, int dy) override { x_ += dx; y_ += dy; }
private:
    int x_, y_, side_;
};

// etc.
```

We would like to implement a Copy/Paste feature in our application, allowing users to duplicate shapes. However, the current design requires us to know the exact type of shape we want to copy, which can be cumbersome and error-prone. A typical implementation of the Copy/Paste feature might look like this:

```cpp
void copy_selected_shape()
{
    std::unique_ptr<Shape> copied_shape = nullptr;
    
    Shape* selected_shape = get_selected_shape();
    
    if (selected_shape)
    {
        if (Circle* circle = dynamic_cast<Circle*>(selected_shape); circle)
        {
            copied_shape = std::make_unique<Circle>(*circle); // call of copy constructor for Circle
        }
        else if (Square* square = dynamic_cast<Square*>(selected_shape); square)
        {
            copied_shape = std::make_unique<Square>(*square); // call of copy constructor for Square
        }
        // etc.
        
        if (copied_shape)
        {
            add_to_clipboard(copied_shape);
        }
    }
}
```

The problem with this approach is that it requires us to know the exact type of shape we want to copy. If we add a new shape type, we need to modify the `copy_selected_shape()` function to handle the new type. This violates the Open/Closed Principle (OCP), which states that software entities should be open for extension but closed for modification.

What we need is a way to create a new shape by copying an existing one without knowing its exact type. This is where the **Prototype** pattern comes in.

The solution is to add a `clone()` method to the `Shape` class and implement it in each derived class. The `clone()` method can be used by the clients to create a copy of the shape without knowing its exact type:

```cpp
class Shape
{
public:
    virtual ~Shape() = default;
    virtual void draw() = 0;
    virtual void move(int dx, int dy) = 0;
    virtual std::unique_ptr<Shape> clone() const = 0; // Prototype method
};
```

The `clone()` method has to be implemented in each derived class:

```cpp
class Circle : public Shape
{
public:
    Circle(int x, int y, int radius) : x_(x), y_(y), radius_(radius) {}
    void draw() override { /* draw circle */ }
    void move(int dx, int dy) override { x_ += dx; y_ += dy; }
    
    std::unique_ptr<Shape> clone() const override 
    { 
        return std::make_unique<Circle>(*this); // call of copy constructor for Circle
    } 
};

class Square : public Shape
{
public:
    Square(int x, int y, int side) : x_(x), y_(y), side_(side) {}
    void draw() override { /* draw square */ }
    void move(int dx, int dy) override { x_ += dx; y_ += dy; }
    
    std::unique_ptr<Shape> clone() const override 
    { 
        return std::make_unique<Square>(*this); // call of copy constructor for Square
    }
};
```

Now we can implement the `copy_selected_shape()` function without knowing the exact type of shape we want to copy:

```cpp
void copy_selected_shape()
{
    std::unique_ptr<Shape> copied_shape = nullptr;
    
    Shape* selected_shape = get_selected_shape();
    
    if (selected_shape)
    {
        copied_shape = selected_shape->clone(); // call of clone method
        add_to_clipboard(copied_shape);
    }
}
```

The code is now cleaner and adheres to the OCP. If we add a new shape type, we only need to implement the `clone()` method in that class, and the `copy_selected_shape()` function will work without any modifications.

## CRTP for cloning

Now introduction of a new shape requires overriding the `clone()` method in the derived class. It's implementation is very similar in each subclass:

```cpp
class Rectangle : public Shape
{
public:
    std::unique_ptr<Shape> clone() const override
    {
        return std::make_unique<Rectangle>(*this); // call of copy constructor for Rectangle
    }
    //.. rest of the class
};
```

The only thing we have to change is a name of a concreate type passed to `std::make_unique` function. We can use the **Curiously Recurring Template Pattern** (CRTP) to avoid code duplication in the `clone()` method. Let us introduce a `CloneableShape` class template that will implement the `clone()` method using CRTP:

```cpp
template <typename TShape>
class CloneableShape : public Shape
{
public:
    std::unique_ptr<Shape> clone() const override
    {
        // Use static_cast to ensure the correct type is passed to the copy constructor.
        // This converts the current object (*this) to the derived type (TShape) safely.
        return std::make_unique<TShape>(static_cast<const TShape&>(*this));
    }
};
```

Static cast is used to convert `*this` reference (which is `const CloneableShape<TShape>&`) to the reference type that will allow calling the copy constructor for an object of the type passed as a template parameter. 

The advantage of CRTP is that it eliminates repetitive code by providing a reusable implementation of the `clone()` method. This ensures consistency and reduces the likelihood of errors when adding new shape types. For example, a new shape type (`Rectangle`) can inherit from `CloneableShape<Rectangle>`, which automatically injects the correct implementation of the `clone()` method:

```cpp
class Rectangle : public CloneableShape<Rectangle>
{
public:
    Rectangle(int x, int y, int width, int height) : x_(x), y_(y), width_(width), height_(height) {}
    void draw() override { /* draw rectangle */ }
    void move(int dx, int dy) override { x_ += dx; y_ += dy; }
};
```

## Prototype and Liskov Substitution Principle (LSP)

The **Prototype** pattern relies on the **Liskov Substitution Principle** (LSP), which states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In the context of the Prototype pattern, this means that the `clone()` method should return a pointer to the base class (`Shape`) but create an object of the derived class (e.g., `Circle`, `Square`, etc.). This allows us to treat all shapes uniformly while still being able to create specific instances of each shape type.

Unfortunately, we can easily introduce a bug when we inherit from a concrete class and forget to override the `clone()` method. To adhere to the Liskov Substitution Principle (LSP) and ensure correct behavior, the `clone()` method must be explicitly overridden in every derived class. If not, the derived class will not be able to clone itself correctly, which can lead to unexpected behavior:

```cpp
class ColorCircle : public Circle
{
public:
    ColorCircle(int x, int y, int radius, const Color& color) : Circle(x, y, radius), color_(color) {}
    void draw() override { /* draw colored circle */ }
    void move(int dx, int dy) override { Circle::move(dx, dy); } 
private:
    Color color_;
};

void liskov_substitution_violation(Shape& shape)
{
    std::unique_ptr<Shape> cloned_shape = shape.clone(); // client should get the exact type of the shape
    cloned_shape->draw(); 
}

int main()
{
    auto color_circle = std::make_unique<ColorCircle>(0, 0, 10, Color::Red);
    liskov_substitution_violation(*color_circle); // This will call the draw method of Circle instead of ColorCircle
}
```

This is a violation of the LSP because the derived class (`ColorCircle`) does not behave as expected when used in place of the base class (`Shape`). The `clone()` method should be implemented in each concrete class that derives from `Shape` to ensure that it returns a pointer to the correct type. This bug is very difficult to find, because the code compiles and runs. But at some point (after clonning) it runs in a wrong state.


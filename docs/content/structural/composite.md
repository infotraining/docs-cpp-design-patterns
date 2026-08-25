# Composite

The **Composite Pattern** is a design pattern that enables uniform treatment of individual objects and their compositions. It is commonly applied to represent part-whole hierarchies, allowing both single objects and groups to be handled identically.

Consider a typical example of the **Composite Pattern**. When developing an application that draws shapes on the screen, like circles and squares, a common interface for all shapes is desirable. This interface allows us to draw or scale (inflate) individual shapes without concern for their specific types. This is a classic example of runtime polymorphism. These concrete classes sharing a common interface are often referred to as *leaves* in the context of the Composite Pattern. 

The *leaf* hierarchy for the shapes could be implemented as follows:

```cpp
class Shape
{
public:
    virtual void draw() const = 0;   
    virtual void inflate(int factor) = 0; 
    virtual ~Shape() = default; 
};

class Circle : public Shape
{
    Coordinates position_;
    int radius_;
public:
    Circle(const Coordinates& pos, int r) : position_{pos}, radius_{r} {}

    void draw() const override
    {
        std::cout << "Drawing a circle at " << position_ 
                  << " with radius " << radius_ << "\n";
    }

    void inflate(int factor) override
    {
        radius_ *= factor;
    }
};

class Rectangle : public Shape
{
    Coordinates position_;
    int width_;
    int height_;
public:
    Rectangle(const Coordinates& pos, int w, int h) : position_{pos}, width_{w}, height_{h} {}

    void draw() const override
    {
        std::cout << "Drawing a rectangle at " << position_ 
                  << " with width " << width_ 
                  << " and height " << height_ << "\n";
    }

    void inflate(int factor) override
    {    
        width_ *= factor;
        height_ *= factor;
    }
};
```

The client code can use `Shape` interface to work consistently with a collection of shapes:

```cpp
void client(std::vector<std::unique_ptr<Shape>>& shapes)
{
    for (auto& shape : shapes)
    {
        shape->draw();
        shape->inflate(2);
        shape->draw();
    }
}

std::vector<std::unique_ptr<Shape>> create_shapes()
{
    std::vector<std::unique_ptr<Shape>> shapes;
    shapes.push_back(std::make_unique<Circle>(Coordinates{10, 20}, 5));
    shapes.push_back(std::make_unique<Rectangle>(Coordinates{30, 40}, 10, 20));
    return shapes;
}

std::vector<std::unique_ptr<Shape>> shapes = create_shapes();
client(shapes);
```

At this point, we can observe that employing dynamic polymorphism in traditional C++ requires us to introduce **reference semantics**. We must use pointers both to store objects in containers and to pass them as function arguments for subsequent polymorphic use.

## Composite objects

A fundamental feature in any graphics application is the ability to group several shapes together, treating the group as a single entity. This allows us to draw or inflate the entire group, with each shape being affected accordingly. The Composite Pattern is ideal for this purpose. It involves creating a class that aggregates a group of shapes while implementing the same interface as individual shapes. This approach enables us to handle both single shapes and shape groups uniformly. Let's implement this:

```cpp
class ShapeGroup : public Shape // inherits from Shape, so it can be treated as a single shape
{
    std::vector<std::unique_ptr<Shape>> shapes_; // aggregates (owns) a group of shapes
public:
    ShapeGroup() = default;

    void add_shape(std::unique_ptr<Shape> shape)
    {
        shapes_.push_back(std::move(shape));
    }

    void draw() const override
    {
        std::cout << "Drawing a group of shapes:" << std::endl;
        for (const auto& shape : shapes_)
        {
            shape->draw();
        }
    }

    void inflate(int factor) override
    {
        for (const auto& shape : shapes_)
        {
            shape->inflate(factor);
        }
    }
};
```

Creating a composite class offers numerous advantages.

1.  It enables us to handle individual shapes and shape groups consistently, simplifying client code. The client no longer needs to manage individual shapes and groups separately.
    
    Now, the `clients_code()` function is much more streamlined. It doesn't need to iterate over the collection of shapes to call `draw()` and `inflate()` on each one individually. Instead, it can simply call these methods on the group, which will manage invoking them on each shape.
    

```cpp
void clients_code(ShapeGroup& group)
{
    group.draw();
}
```

2.  We can form intricate shapes by composing simpler ones. By grouping several basic shapes, we create a complex shape that can be treated as a single entity.
    

```cpp
ShapeGroup smiley_face;
smiley_face.add_shape(std::make_unique<Circle>(Coordinates{50, 50}, 20)); // face
smiley_face.add_shape(std::make_unique<Circle>(Coordinates{40, 60}, 5)); // left eye
smiley_face.add_shape(std::make_unique<Circle>(Coordinates{60, 60}, 5)); // right eye
smiley_face.add_shape(std::make_unique<Rectangle>(Coordinates{45, 40}, 10, 5)); // mouth

clients_code(smiley_face);
```

3.  Finally, it allows for the creation of shape hierarchies, which is a key advantage of the Composite Pattern. In graphics applications, we can implement layers, where each layer contains multiple shapes and can be treated as a single shape. This enables us to draw or inflate an entire layer, affecting all the shapes within it simultaneously.
    

```cpp
auto layer1 = std::make_unique<ShapeGroup>();
layer1->add_shape(std::make_unique<Circle>(Coordinates{10, 20}, 5));
layer1->add_shape(std::make_unique<Rectangle>(Coordinates{30, 40}, 10, 20));

auto layer2 = std::make_unique<ShapeGroup>();
layer2->add_shape(std::make_unique<Circle>(Coordinates{50, 60}, 15));
layer2->add_shape(std::make_unique<Rectangle>(Coordinates{70, 80}, 20, 30));

// create a drawing that contains multiple layers of shapes
ShapeGroup drawing{};
drawing.add_shape(std::move(layer1));
drawing.add_shape(std::move(layer2));

// pass the drawing to the client code, which can treat it as a single shape
clients_code(drawing);
```

## Copying Composites

When dealing with a composite structure, it's often necessary to copy it. Each leaf type, such as `Circle` or `Rectangle`, can be copied individually. However, copying a composite structure requires creating a deep copy of the entire structure. This presents a common challenge with polymorphic objects: how to copy them when we only have a pointer or reference to the base class, without knowing the object's actual type. The **Prototype Pattern** offers a solution. By defining a `clone()` method in the base class that returns a pointer to a new copy of the object, each derived class can implement this method to return a copy of itself. This clone operation acts like a *"virtual copy constructor."*

```cpp
class Shape
{
public:
    virtual void draw() const = 0;   
  virtual void inflate(int factor) = 0;
    virtual std::unique_ptr<Shape> clone() const = 0; // new method for cloning shapes

    virtual ~Shape() = default;
};
```

The implementation in a concrete class of `clone()` operation looks like this:

```cpp
class Circle : public Shape
{
  Coordinates position_;
  int radius_;
public:
  Circle(const Coordinates& pos, int r) : position_(pos), radius_(r) {}

  /* rest of implementation */
        
  std::unique_ptr<Shape> clone() const override
  {
    return std::make_unique<Circle>(*this); // create a new Circle by copying the current one (using the copy constructor)
  }
};
```

We can now use the `clone()` method to generate copies of existing shapes without being aware of their specific types. The implementation of a composite type is as follows:

```cpp
class ShapeGroup : public Shape
{
  std::vector<std::unique_ptr<Shape>> shapes_; // aggregates (owns) a group of shapes 
public:
  ShapeGroup() = default; 

  ShapeGroup(const ShapeGroup& other) // copy constructor for deep copying the shapes in the group
  {
    shapes_.reserve(other.shapes_.size()); 

    for (const auto& shape : other.shapes_)
    {
      shapes_.push_back(shape->clone()); // clone each shape and add it to the new group
    }
  }

  ShapeGroup& operator=(const ShapeGroup& other) 
  {
    ShapeGroup temp(other); // create a temporary copy of the other object (using the copy constructor)
    swap(temp);   // swap the contents of this object with the temporary copy (temp)
    return *this; // return the current object (after the swap)
  }

  void swap(ShapeGroup& other) noexcept // swap function for the copy-and-swap idiom
  {
    std::swap(shapes_, other.shapes_); // swap the shapes vector of this object with the other object
  }

  ShapeGroup(ShapeGroup&&) = default; // move constructor
  ShapeGroup& operator=(ShapeGroup&&) = default; // move assignment operator

  void add_shape(std::unique_ptr<Shape> shape)
  {
    shapes_.push_back(std::move(shape));
  }

  void draw() const override
  {
    std::cout << "Drawing a group of shapes:" << std::endl;
    for (const auto& shape : shapes_)
    {
      shape->draw();
    }
  }

  void inflate(int factor) override
  {
    std::cout << "Inflating a group of shapes:" << std::endl;
    for (const auto& shape : shapes_)
    {
      shape->inflate(factor);
    }
  }  

  std::unique_ptr<Shape> clone() const override
  {
    return std::make_unique<ShapeGroup>(*this); // create a new ShapeGroup by copying the current one (using the copy constructor)
  }
};
```

The custom copy constructor, which performs a deep copy of the aggregated shapes using the `clone()` operation, means we should review all special member functions (Rule of Five) to ensure composite objects are copied and moved correctly and efficiently. In this case, move operations can still be defaulted.

### CRTP and Cloning

Implementing the `clone()` method in each derived class is repetitive. We can use the **Curiously Recurring Template Pattern** (**CRTP**) to eliminate this redundancy. By defining a templated base class that implements the `clone()` method, each derived class can inherit from this template base class, passing itself as a template parameter. This approach allows us to implement the `clone()` method once in the template base class, making it applicable to all derived classes without needing individual implementations.

```cpp
class Shape
{
public:
  virtual void draw() const = 0;   
  virtual void inflate(int factor) = 0;
  virtual std::unique_ptr<Shape> clone() const = 0; // new method for cloning shapes

  virtual ~Shape() = default;
};

template <typename TShape, typename TShapeBase = Shape>
class CloneableShape : public TShapeBase
{        
public:
  using TShapeBase::TShapeBase; // inherit constructors from the base class

  std::unique_ptr<Shape> clone() const override
  {
    return std::make_unique<TShape>(static_cast<const TShape&>(*this));
  }
};

class Circle : public CloneableShape<Circle>
{
  Coordinates position_;
  int radius_;
public:
  Circle(const Coordinates& pos, int r) : position_(pos), radius_(r) {} 

  void draw() const override { /* implementation */ }

  void inflate(int factor) override { /* implementation */ }
            
  const Coordinates& position() const { return position_; }
  int radius() const { return radius_; }
};

class ShapeGroup : public CloneableShape<ShapeGroup>
{
  std::vector<std::unique_ptr<Shape>> shapes_; // aggregates (owns) a group of shapes
public:
  ShapeGroup() = default; 

  ShapeGroup(const ShapeGroup& other) // copy constructor for deep copying the shapes in the group
  {
    shapes_.reserve(other.shapes_.size()); 

    for (const auto& shape : other.shapes_)
    {
      shapes_.push_back(shape->clone()); // clone each shape and add it to the new group
    }
  }

  ShapeGroup& operator=(const ShapeGroup& other) // copy assignment operator for deep copying the shapes in the group
  {
    if (this != &other) // check for self-assignment
    {
      ShapeGroup temp(other); // create a temporary copy of the other object (using the copy constructor)
      swap(temp);   // swap the contents of this object with the temporary copy (temp)
    }
    return *this; // return the current object (after the swap)
  }

  void swap(ShapeGroup& other) noexcept
  {
    std::swap(shapes_, other.shapes_); // swap the shapes vector of this object with the other object
  }

  ShapeGroup(ShapeGroup&&) = default; // move constructor
  ShapeGroup& operator=(ShapeGroup&&) = default; // move assignment operator

  void add_shape(std::unique_ptr<Shape> shape)
  {
    shapes_.push_back(std::move(shape));
  }

  void draw() const override
  {
    std::cout << "Drawing a group of shapes:" << std::endl;
    for (const auto& shape : shapes_)
    {
      shape->draw();
    }
  }   

  void inflate(int factor) override
  {
    std::cout << "Inflating a group of shapes:" << std::endl;
    for (const auto& shape : shapes_)
    {
      shape->inflate(factor);
    }
  }
};
```

Our solution aims to minimize redundancy in the `clone()` implementation, though it is not yet perfect. We must remember to inherit from the `CloneableShape` class. This is essential - failing to override the `clone()` method from the base class can lead to serious bugs. The client may receive an incorrect clone of the shape, making the issue difficult to trace. This problem arises from violating the **Liskov Substitution Principle**, as we cannot substitute a derived class for its base class without compromising the program's correctness.

```cpp
class EvilCircle : public ColorCircle // BEWARE!!!
{
public:
  EvilCircle(const Coordinates& pos, int r, const std::string& color) 
    : ColorCircle(pos, r, color) {}

  void draw() const override
  {
    std::cout << "Drawing an evil " << color() << " circle at " << position() << " with radius " << radius() << std::endl;
  }
};

void lsp_violation()
{
  auto evil_shape = std::make_unique<PrototypePattern::CRTP::EvilCircle>(Coordinates{50, 60}, 15, "black");
  evil_shape->draw();

  std::unique_ptr<PrototypePattern::CRTP::Shape> cloned_shape = evil_shape->clone(); // this will call the clone() method of the ColorCircle class, which creates a new ColorCircle object, not an EvilCircle object
  cloned_shape->draw(); // this will call the draw() method of the ColorCircle class
}
```

To avoid this pitfall, you can apply the **Template Method Pattern**, as suggested by Andrei Alexandrescu in his renowned book, *Modern C++ Design*. This involves defining a non-virtual `clone()` method in the base class that invokes a virtual `do_clone()` method, ensuring the returned object's type matches the expected type. Although each derived class still needs to implement the `do_clone()` method, this approach provides a safety check to prevent the oversight of not overriding the `clone()` method.

## Polymorphic Value Type to the Rescue (C++26)

Implementing composite objects that aggregate polymorphic objects is complex. Each derived class requires a `clone()` method, and forgetting to override it can lead to elusive bugs. Thankfully, the upcoming C++ standard (C++26) introduces **Polymorphic Value Types** (**PVT**s), simplifying and securing this process. PVTs enable the creation of a class that can store a value of any type derived from a common base class, automatically managing the copying and moving of the contained object. This allows us to create a composite class holding a collection of shapes without needing to implement the `clone()` method in each derived class, ensuring correct handling of copying and moving.

The PVT type is a template class that takes a base class as a template parameter. It allows us to store any object that derives from the base class, and it automatically handles copying and moving of the contained object (using type erasure). It allows us to properly use **value semantics** with **polymorphic types**, which is a common requirement when implementing composite objects.

We can create a `poly_shape` object that holds a `Circle`. PVT type overloads `operator->` and `operator*` to allow us to access the contained object as if it were a pointer or reference to the base class. This means that we can call the `draw()` and `inflate()` methods on the `poly_shape` object, and it automatically forwards these calls to the contained `Circle` object.

```cpp
std::polymorphic<Shape> poly_shape(Circle(Coordinates(10, 20), 30));
poly_shape->draw();
```

Now we can assign a new `Rectangle` to the same `poly_shape` object:

```cpp
poly_shape = std::polymorphic<Shape>(Rectangle(Coordinates(20, 30), 100, 200));
poly_shape->draw();
(*poly_shape).draw();
```

The key advantage of `std::polymorphic<T>` is its support for **value semantics**, allowing you to copy and assign it without concerns about slicing or memory management. Copying a `std::polymorphic` creates a new instance of the current underlying shape (e.g., `Circle` or `Rectangle`). This enables you to work with polymorphic objects naturally and intuitively, without directly handling pointers or dynamic memory allocation.

```cpp
std::polymorphic<Shape> another_poly_shape = poly_shape; // copy constructor is called, which creates a deep copy of the Rectangle
another_poly_shape->draw(); // drawing another_poly_shape should produce the same output as drawing poly_shape, since it is a deep copy 
```

### Holding null values

Overloading `operator->` and `operator*` allows us to access the contained object as if we used a pointer to a base class. However, contrary to smart pointers, the design of PVT doesn't support holding null values. If we want to add support for null values, we can use `std::optional` to wrap the PVT type. This way, we can represent the absence of any shape by using an empty `std::optional`:

```cpp
std::optional<std::polymorphic<Shape>> optional_shape; // default-constructed optional is empty (does not contain a value)

optional_shape = std::polymorphic<Shape>(Circle(Coordinates(10, 20), 30)); // assign a Circle to the optional, now it contains a value

if (optional_shape)
{
    (*optional_shape)->draw(); // this will be executed, since optional_shape contains a value
}
else
{
    std::cout << "optional_shape is empty" << std::endl;
}
```

### Valueless after move

When we use PVTs, we have to be aware of the possibility of a *valueless after move* state. When we move a `std::polymorphic<Shape>`, the source object is left in a valid but unspecified state. Any attempt to access its underlying object leads to undefined behavior. To mitigate this issue, we can check (very rarely) if the source object is in a valueless state before accessing it.

```cpp
std::polymorphic<Shape> some_shape(Circle(Coordinates(10, 20), 30)); // create a polymorphic shape that holds a Circle

std::polymorphic<Shape> target = std::move(some_shape); 
assert(some_shape.valueless_after_move());
```

## Implementing Composite Pattern with PVTs

Using the new `std::polymorphic<T>` type, which provides value semantics for polymorphic type hierarchies, we can streamline the implementation of `ShapeGroup`. The **Rule of Zero** greatly reduces the complexity of the entire class.

```cpp
class ShapeGroup : public Shape
{
   std::vector<std::polymorphic<Shape>> shapes_; // aggregates (owns) a group of shapes

public:
   ShapeGroup() = default;

   template <typename TShape, typename... TArgs>
   void add(TArgs&&... args)
   {
     shapes_.emplace_back(TShape(std::forward<TArgs>(args)...)); // perfect forwarding of arguments to construct a new shape in place within the vector
   }

   void draw() const override
   {
     std::cout << "Drawing a group of shapes:" << std::endl;
     for (const auto& shape : shapes_)
     {
       shape->draw();
     }
   }

   void inflate(int factor) override
   {
     std::cout << "Inflating a group of shapes:" << std::endl;
     for (const auto& shape : shapes_)
     {
       shape->inflate(factor);
     }
   }
};
```

## Polymorphic Wrappers

In order to break the strong coupling between types introduced by inheritance, we can use *Polymorphic Wrappers*. The Standard Library has a whole family of polymorphic wrappers that allow us to treat uniformly objects that can be called as functions (function pointers, function objects, or closures): `std::function`, `std::copyable_function`, and `std::move_only_function`. They erase differences in concrete types and emphasize the common interface for all stored values (for functions it is the function call operator). Unfortunately, we do not have a generic standard wrapper for arbitrary custom interfaces (there is a chance that C++29 will introduce one).

We can try to implement a simplified version of such a wrapper using `std::polymorphic<T>`. The idea is to define an internal wrapper class that inherits from an interface class (this interface is now an implementation detail). The wrapper is a template class that takes a concrete type as a template parameter, and it implements the interface by forwarding calls to the contained object of the concrete type. This way, we can create polymorphic wrappers for each of our shape types (for example, `ShapeWrapper<Circle>` and `ShapeWrapper<Rectangle>`) that implement the same interface (`IShape`). The `Shape` class holds a `std::polymorphic<IShape>` object, which can hold any wrapped shape. This allows us to treat uniformly all objects that have the required set of methods (duck typing known from Python).

```cpp
template <typename T>
concept ShapeProtocol = requires(T shape, int factor) {
   shape.draw();
  shape.inflate(factor);
};

class Shape
{
  class IShape
  {
  public:
    virtual void draw() const = 0;
    virtual void inflate(int factor) = 0;
    virtual ~IShape() = default; 
  };

  template <ShapeProtocol TShape>
  class ShapeWrapper : public IShape
  {
    TShape shape_;
  public:
    ShapeWrapper(const TShape& shape) : shape_(shape) {}
    ShapeWrapper(TShape&& shape) : shape_(std::move(shape)) {}

    void draw() const override { shape_.draw(); }
    void inflate(int factor) override { shape_.inflate(factor); }
  };

  std::polymorphic<IShape> shape_; // holder of wrapped shapes - can hold any shape that implements the IShape interface (which is implemented by the ShapeWrapper template class)

public:
  template <ShapeProtocol TShape>
     requires (!std::is_same_v<std::decay_t<TShape>, Shape>) // prevent recursive nesting of Shape within itself
        Shape(TShape&& shp) : shape_(ShapeWrapper<std::decay_t<TShape>>(std::forward<TShape>(shp))) {} // perfect forwarding of arguments to construct a new shape in

        void draw() const { shape_->draw(); }
        void inflate(int factor) { shape_->inflate(factor); }
      };
```

Composite using Polymorphic Wrappers:

```cpp
class ShapeGroup
{
  std::vector<Shape> shapes_; // aggregates a group of shapes
public:
  ShapeGroup() = default;

  template <typename TShape>
  void add_shape(TShape&& shape)
  {
    shapes_.emplace_back(std::forward<TShape>(shape)); 
  }

  void draw() const
  {
    std::cout << "Drawing a group of shapes:" << std::endl;
    for (const auto& shape : shapes_)
    {
      shape.draw();
    }
  }

  void inflate(int factor)
  {
    std::cout << "Inflating a group of shapes:" << std::endl;
    for (auto& shape : shapes_)
    {
      shape.inflate(factor);
    }
  }
};

class Circle // no inheritance
{
  Coordinates position_;
  int radius_;
public:
  Circle(const Coordinates& pos, int r) : position_(pos), radius_(r) 
  {
    std::cout << "Constructing a circle at " << position_ << " with radius " << radius_ << std::endl;
  }

  void draw() const
  {
    std::cout << "Drawing a circle at " << position_ << " with radius " << radius_ << std::endl;
  }

  void inflate(int factor)
  {
    radius_ *= factor;
    std::cout << "Inflated a circle at " << position_ << " to radius " << radius_ << std::endl;
  }
};

void using_polymorphic_wrappers()
{
  ShapeGroup upper_layer;
  upper_layer.add_shape(Circle(Coordinates{10, 20}, 5)); 
  upper_layer.add_shape(Rectangle(Coordinates{30, 40}, 10, 20));
    
  ShapeGroup picture;
  picture.add_shape(Circle(Coordinates{10, 20}, 5));
  picture.add_shape(Rectangle(Coordinates{30, 40}, 10, 20));
  picture.add_shape(upper_layer); // add a group of shapes as a single shape within the picture

  picture.draw();
  picture.inflate(2);
  picture.draw();
}
```

After implementing `Shape` as polymorphic wrappers, we can use Python-style duck typing for runtime polymorphism. We do not have to inherit from a base class. If an object has `draw()` and `inflate(int)` methods, it can be stored in our wrapper. The whole client code promotes value semantics. We do not need explicit pointers (smart or raw) or explicit dynamic allocation in client code. The virtual-dispatch implementation details remain encapsulated.

## Summary

The **Composite Pattern** allows us to treat individual objects and their compositions uniformly. It is particularly useful for representing part-whole hierarchies, enabling us to work with both single objects and groups of objects through a common interface. By implementing a composite class that aggregates a group of shapes, we can simplify client code and create complex shapes by composing simpler ones. The **Prototype Pattern** can be used to facilitate copying of composite structures, while the **Curiously Recurring Template Pattern (CRTP)** can help eliminate redundancy in clone implementations. 

With the introduction of **Polymorphic Value Types** in C++26, we can further simplify the implementation of composite objects, providing value semantics for polymorphic types and reducing the complexity of managing polymorphic objects.
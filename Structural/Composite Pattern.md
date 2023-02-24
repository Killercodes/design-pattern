Composite Pattern
===
The Composite Pattern is a structural design pattern that allows you to treat a group of objects as if they were a single object. It composes objects into tree structures to represent part-whole hierarchies. With this pattern, you can create a hierarchy of objects and treat the individual objects and the composite of objects in the same way.

The key participants in the Composite Pattern are:

1. Component: the abstract class or interface that defines the common methods for all the objects in the composite structure. It's the base class for all the objects in the hierarchy, and it can be either a leaf or a composite.

2. Leaf: the concrete class that represents the individual objects in the composite structure. It implements the methods defined by the component.

3. Composite: the concrete class that represents the composite objects in the composite structure. It implements the methods defined by the component and also has a list of child components.

4. Client: the class that uses the component to interact with the objects in the composite structure.

The Composite Pattern is useful when you want to create a hierarchy of objects where each object has some common functionality and some specific functionality. It's also useful when you want to treat a group of objects as a single object. This pattern provides a way to create complex object structures while keeping the interface simple and consistent. It also makes it easy to add new objects to the hierarchy without affecting the existing code.

Some of the benefits of the Composite Pattern are:
- It simplifies the code by treating the individual objects and the composite of objects in the same way.
- It provides a flexible structure that can be extended easily.
- It allows you to add new objects to the hierarchy without affecting the existing code.
- It provides a clear and consistent interface for working with complex object structures.
- It promotes code reuse by allowing the same code to be used for individual objects and composite objects.

Some of the drawbacks of the Composite Pattern are:
- It can be difficult to ensure that the composite structure is consistent and valid.
- It can be difficult to maintain the performance of the composite structure as the size of the structure grows.
- It can be difficult to implement operations that only make sense for individual objects and not for the composite object.

---
## c#
here's an example of the Composite Pattern in C#:
```cs
using System;
using System.Collections.Generic;

// Component
interface IShape
{
    void Draw();
}

// Leaf
class Circle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Circle");
    }
}

// Leaf
class Square : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Square");
    }
}

// Composite
class Group : IShape
{
    private List<IShape> shapes = new List<IShape>();

    public void Add(IShape shape)
    {
        shapes.Add(shape);
    }

    public void Draw()
    {
        Console.WriteLine("Drawing Group:");
        foreach (IShape shape in shapes)
        {
            shape.Draw();
        }
    }
}

// Client
class Program
{
    static void Main(string[] args)
    {
        // Creating individual shapes
        Circle circle = new Circle();
        Square square = new Square();

        // Creating a group and adding shapes to it
        Group group = new Group();
        group.Add(circle);
        group.Add(square);

        // Drawing individual shapes and group
        circle.Draw();
        square.Draw();
        group.Draw();
    }
}

```
In this example, IShape is the Component interface, Circle and Square are the Leaf classes, and Group is the Composite class. The Group class can contain other IShape objects and can be treated as a single IShape object itself. The Draw method is implemented in each of the classes and is called recursively on each of the objects in the composite structure to draw them.

---

## Javascript
Here's an example of the Composite Pattern in JavaScript:

```js
// Component
class Shape {
  draw() {}
}

// Leaf
class Circle extends Shape {
  draw() {
    console.log("Drawing Circle");
  }
}

// Leaf
class Square extends Shape {
  draw() {
    console.log("Drawing Square");
  }
}

// Composite
class Group extends Shape {
  constructor() {
    super();
    this.shapes = [];
  }

  add(shape) {
    this.shapes.push(shape);
  }

  draw() {
    console.log("Drawing Group:");
    this.shapes.forEach((shape) => {
      shape.draw();
    });
  }
}

// Client
let circle = new Circle();
let square = new Square();
let group = new Group();
group.add(circle);
group.add(square);

circle.draw();
square.draw();
group.draw();
```

In this example, Shape is the Component class, Circle and Square are the Leaf classes, and Group is the Composite class. The Group class can contain other Shape objects and can be treated as a single Shape object itself. 
The draw method is implemented in each of the classes and is called recursively on each of the objects in the composite structure to draw them.

---
## Python
Here's an example of the Composite Pattern in Python 3:
```py
# Component
class Shape:
    def draw(self):
        pass

# Leaf
class Circle(Shape):
    def draw(self):
        print("Drawing Circle")

# Leaf
class Square(Shape):
    def draw(self):
        print("Drawing Square")

# Composite
class Group(Shape):
    def __init__(self):
        self.shapes = []

    def add(self, shape):
        self.shapes.append(shape)

    def draw(self):
        print("Drawing Group:")
        for shape in self.shapes:
            shape.draw()

# Client
circle = Circle()
square = Square()
group = Group()
group.add(circle)
group.add(square)

circle.draw()
square.draw()
group.draw()

```
In this example, Shape is the Component class, Circle and Square are the Leaf classes, and Group is the Composite class. 
The Group class can contain other Shape objects and can be treated as a single Shape object itself. The draw method is implemented in each of the classes and is called recursively on each of the objects in the composite structure to draw them.

---

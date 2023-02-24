Bridge Pattern
===
The Bridge pattern is a design pattern that separates the abstraction from its implementation so that they can vary independently. This allows us to change the implementation details without affecting the clients that use the abstraction. It is used when we have a system that has multiple implementations, and we want to decouple the abstraction from the implementation.

In the Bridge pattern, there are two main components: the Abstraction and the Implementation. The Abstraction defines the interface that the clients use to interact with the system, and the Implementation provides the concrete implementation of the system. The Abstraction has a reference to the Implementation, but the Implementation does not have any reference to the Abstraction. This allows us to change the implementation without affecting the Abstraction.

The Bridge pattern is useful when we want to avoid a situation where we have to create multiple subclasses to handle the variations in the implementation details. Instead, we can create separate Abstraction and Implementation hierarchies and use composition to combine them.

Some benefits of using the Bridge pattern are:
- It allows us to vary the implementation details independently of the abstraction.
- It provides a way to decouple the abstraction from the implementation, which makes the code more modular and maintainable.
- It enables us to create a more flexible and extensible system.

Some common use cases for the Bridge pattern are:
- When we have a system that has multiple implementations, and we want to decouple the abstraction from the implementation.
- When we want to create a more flexible and extensible system.
- When we want to avoid a situation where we have to create multiple subclasses to handle the variations in the implementation details.


---
## c#

here's an example of the Bridge pattern in C#:

First, we'll define the Abstraction interface:

```cs
public interface IShape
{
    void Draw();
}

```
Next, we'll define the concrete implementations of the shape:

```cs
public class Circle : IShape
{
    private readonly IRenderer _renderer;
    private readonly float _radius;

    public Circle(IRenderer renderer, float radius)
    {
        _renderer = renderer;
        _radius = radius;
    }

    public void Draw()
    {
        _renderer.RenderCircle(_radius);
    }
}

public class Square : IShape
{
    private readonly IRenderer _renderer;
    private readonly float _side;

    public Square(IRenderer renderer, float side)
    {
        _renderer = renderer;
        _side = side;
    }

    public void Draw()
    {
        _renderer.RenderSquare(_side);
    }
}

```
Next, we'll define the IRenderer interface:
```cs
public interface IRenderer
{
    void RenderCircle(float radius);
    void RenderSquare(float side);
}

```
And finally, we'll define the concrete implementations of the IRenderer interface:
```cs
public class OpenGLRenderer : IRenderer
{
    public void RenderCircle(float radius)
    {
        Console.WriteLine($"OpenGLRenderer rendering circle with radius {radius}");
    }

    public void RenderSquare(float side)
    {
        Console.WriteLine($"OpenGLRenderer rendering square with side {side}");
    }
}

public class DirectXRenderer : IRenderer
{
    public void RenderCircle(float radius)
    {
        Console.WriteLine($"DirectXRenderer rendering circle with radius {radius}");
    }

    public void RenderSquare(float side)
    {
        Console.WriteLine($"DirectXRenderer rendering square with side {side}");
    }
}

```

Finally, we can use the Bridge pattern to create different combinations of shapes and renderers:
```cs
var openGLRenderer = new OpenGLRenderer();
var directXRenderer = new DirectXRenderer();

var circle = new Circle(openGLRenderer, 5);
circle.Draw(); // OpenGLRenderer rendering circle with radius 5

var square = new Square(directXRenderer, 7);
square.Draw(); // DirectXRenderer rendering square with side 7

```
In this example, we've used the Bridge pattern to decouple the Abstraction (IShape) from its Implementation (IRenderer). We've created different combinations of shapes and renderers by passing in the concrete implementations of IRenderer to the concrete implementations of IShape. This allows us to change the implementation details of IRenderer without affecting the clients that use IShape.

If this example is too complx consider this simple one:
```cs
using System;

// Implementor
interface IImplementor
{
    string Operation();
}

// Concrete Implementor A
class ConcreteImplementorA : IImplementor
{
    public string Operation()
    {
        return "Concrete Implementor A Operation";
    }
}

// Concrete Implementor B
class ConcreteImplementorB : IImplementor
{
    public string Operation()
    {
        return "Concrete Implementor B Operation";
    }
}

// Abstraction
class Abstraction
{
    private readonly IImplementor _implementor;

    public Abstraction(IImplementor implementor)
    {
        _implementor = implementor;
    }

    public string Operation()
    {
        return $"Abstraction: {_implementor.Operation()}";
    }
}

// Client code
class Client
{
    static void Main()
    {
        var implementorA = new ConcreteImplementorA();
        var abstraction = new Abstraction(implementorA);
        Console.WriteLine(abstraction.Operation());  // Output: Abstraction: Concrete Implementor A Operation

        var implementorB = new ConcreteImplementorB();
        abstraction = new Abstraction(implementorB);
        Console.WriteLine(abstraction.Operation());  // Output: Abstraction: Concrete Implementor B Operation
    }
}

```
In this example, the IImplementor interface is an abstraction that defines the interface for concrete implementors. The ConcreteImplementorA and ConcreteImplementorB classes are concrete implementors that provide the actual implementation for the Operation method.

The Abstraction class defines the abstraction that uses an instance of an IImplementor. It has an Operation method that delegates to the Operation method of its IImplementor instance.

The client code creates an instance of ConcreteImplementorA and assigns it to an Abstraction instance. It calls the Operation method on the Abstraction instance, which in turn calls the Operation method of its IImplementor instance (ConcreteImplementorA), and returns the result.

Then the client code creates a new instance of ConcreteImplementorB and assigns it to the existing Abstraction instance. The Operation method is called again on the Abstraction instance, but this time it delegates to the Operation method of the new IImplementor instance (ConcreteImplementorB), and returns the new result.


---
## Javascript
here's an example of the Bridge pattern in JavaScript:

```js
// Implementor
class Implementor {
  operation() {}
}

// Concrete Implementor A
class ConcreteImplementorA extends Implementor {
  operation() {
    return "Concrete Implementor A Operation";
  }
}

// Concrete Implementor B
class ConcreteImplementorB extends Implementor {
  operation() {
    return "Concrete Implementor B Operation";
  }
}

// Abstraction
class Abstraction {
  constructor(implementor) {
    this.implementor = implementor;
  }

  operation() {
    return `Abstraction: ${this.implementor.operation()}`;
  }
}

// Client code
const implementorA = new ConcreteImplementorA();
const abstraction = new Abstraction(implementorA);
console.log(abstraction.operation()); // Output: Abstraction: Concrete Implementor A Operation

const implementorB = new ConcreteImplementorB();
abstraction.implementor = implementorB;
console.log(abstraction.operation()); // Output: Abstraction: Concrete Implementor B Operation

```
In this example, the Implementor class is an abstract class that defines the interface for concrete implementors. The ConcreteImplementorA and ConcreteImplementorB classes are concrete implementors that provide the actual implementation for the operation method.

The Abstraction class defines the abstraction that uses an instance of an Implementor. It has a operation method that delegates to the operation method of its Implementor instance.

The client code creates an instance of Abstraction with a specific Implementor instance (in this case, ConcreteImplementorA). It calls the operation method on the Abstraction instance, which in turn calls the operation method of its Implementor instance (ConcreteImplementorA), and returns the result.

Then the client code creates a new instance of ConcreteImplementorB and assigns it to the implementor property of the existing Abstraction instance. The operation method is called again on the Abstraction instance, but this time it delegates to the operation method of the new Implementor instance (ConcreteImplementorB), and returns the new result.

---
## Python
here's an example of the Bridge pattern in Python 3:
```py
# Implementor
class Implementor:
    def operation(self) -> str:
        pass

# Concrete Implementor A
class ConcreteImplementorA(Implementor):
    def operation(self) -> str:
        return "Concrete Implementor A Operation"

# Concrete Implementor B
class ConcreteImplementorB(Implementor):
    def operation(self) -> str:
        return "Concrete Implementor B Operation"

# Abstraction
class Abstraction:
    def __init__(self, implementor: Implementor):
        self.implementor = implementor

    def operation(self) -> str:
        return f"Abstraction: {self.implementor.operation()}"

# Client code
implementorA = ConcreteImplementorA()
abstraction = Abstraction(implementorA)
print(abstraction.operation())  # Output: Abstraction: Concrete Implementor A Operation

implementorB = ConcreteImplementorB()
abstraction.implementor = implementorB
print(abstraction.operation())  # Output: Abstraction: Concrete Implementor B Operation

```
In this example, the Implementor class is an abstract class that defines the interface for concrete implementors. The ConcreteImplementorA and ConcreteImplementorB classes are concrete implementors that provide the actual implementation for the operation method.

The Abstraction class defines the abstraction that uses an instance of an Implementor. It has an operation method that delegates to the operation method of its Implementor instance.

The client code creates an instance of Abstraction with a specific Implementor instance (in this case, ConcreteImplementorA). It calls the operation method on the Abstraction instance, which in turn calls the operation method of its Implementor instance (ConcreteImplementorA), and returns the result.

Then the client code creates a new instance of ConcreteImplementorB and assigns it to the implementor attribute of the existing Abstraction instance. The operation method is called again on the Abstraction instance, but this time it delegates to the operation method of the new Implementor instance (ConcreteImplementorB), and returns the new result.
---

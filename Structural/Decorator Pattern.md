Decorator Pattern
===
The Decorator pattern is a structural design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class. It is used to extend the functionality of objects at runtime, without the need to change the original code.

The key participants in this pattern are:

1. Component: This is an abstract class or interface that defines the methods that will be implemented by the concrete components and decorators.

2. Concrete Component: This is a class that implements the Component interface.

3. Decorator: This is an abstract class that inherits from the Component class and contains a reference to an object of the Component class. It also implements the same methods as the Component class.

4. Concrete Decorator: This is a class that inherits from the Decorator class and adds new functionality to the Component class.

The Decorator pattern follows the Open/Closed Principle, which states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means that the behavior of existing code should be able to be modified without changing that code.

The Decorator pattern is useful in situations where a class needs to have different combinations of behaviors. Instead of creating all the possible combinations in different classes, we can use the Decorator pattern to dynamically add or remove behaviors at runtime. This results in a more flexible and modular design, which is easier to maintain and extend.

---
## c#
here's an example of the decorator pattern in a single C# file:
```cs
using System;

// Component interface
interface IComponent
{
    string Operation();
}

// Concrete component
class ConcreteComponent : IComponent
{
    public string Operation()
    {
        return "ConcreteComponent: basic operation.\n";
    }
}

// Base decorator
class Decorator : IComponent
{
    private IComponent _component;

    public Decorator(IComponent component)
    {
        this._component = component;
    }

    public virtual string Operation()
    {
        return this._component.Operation();
    }
}

// Concrete decorator
class ConcreteDecoratorA : Decorator
{
    public ConcreteDecoratorA(IComponent component) : base(component)
    {
    }

    public override string Operation()
    {
        return $"ConcreteDecoratorA({base.Operation()})";
    }
}

// Concrete decorator
class ConcreteDecoratorB : Decorator
{
    public ConcreteDecoratorB(IComponent component) : base(component)
    {
    }

    public override string Operation()
    {
        return $"ConcreteDecoratorB({base.Operation()})";
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Client code
        var simple = new ConcreteComponent();
        Console.WriteLine("Client: I get a simple component:");
        Console.WriteLine($"Result: {simple.Operation()}");

        // Decorators can wrap not only simple components but the other decorators as well
        var decorator1 = new ConcreteDecoratorA(simple);
        var decorator2 = new ConcreteDecoratorB(decorator1);
        Console.WriteLine("\nClient: Now I've got a decorated component:");
        Console.WriteLine($"Result: {decorator2.Operation()}");
    }
}

```
In this example, we have an IComponent interface that defines the Operation method, which is implemented by the ConcreteComponent class. The Decorator class is a base decorator that implements the IComponent interface and contains a reference to an IComponent instance. It also provides a default implementation of the Operation method that simply calls the wrapped object's Operation method.

The ConcreteDecoratorA and ConcreteDecoratorB classes are concrete decorators that extend the Decorator class. They override the Operation method to add their own behavior before or after calling the wrapped object's Operation method.

In the Main method, we first create a simple ConcreteComponent instance and call its Operation method. Then, we create two decorators (ConcreteDecoratorA and ConcreteDecoratorB) and chain them together with the simple component, creating a decorated component. Finally, we call the Operation method on the decorated component to see the output, which includes the added behavior from the decorators.

Overall, the decorator pattern allows us to add behavior to objects dynamically, without having to create a new subclass for each variation. Instead, we can create new decorator classes that wrap existing objects and add new behavior as needed. This can be useful when we have a complex hierarchy of objects and want to add behavior in a flexible and maintainable way.

---
## Javascript
Here's the example of the Decorator pattern in JavaScript
```js
class Component {
  operation() {}
}

class ConcreteComponent extends Component {
  operation() {
    return 'ConcreteComponent';
  }
}

class Decorator extends Component {
  constructor(component) {
    super();
    this._component = component;
  }

  operation() {
    return this._component.operation();
  }
}

class ConcreteDecoratorA extends Decorator {
  constructor(component) {
    super(component);
  }

  operation() {
    return `ConcreteDecoratorA(${super.operation()})`;
  }
}

class ConcreteDecoratorB extends Decorator {
  constructor(component) {
    super(component);
  }

  operation() {
    return `ConcreteDecoratorB(${super.operation()})`;
  }
}

// Usage
const simple = new ConcreteComponent();
console.log('Simple component: ', simple.operation());

const decorator1 = new ConcreteDecoratorA(simple);
const decorator2 = new ConcreteDecoratorB(decorator1);
console.log('Decorated component: ', decorator2.operation());

```
In this example, we have a Component class with a ConcreteComponent implementation. We also have a Decorator class that inherits from Component and contains a reference to a Component object. The ConcreteDecoratorA and ConcreteDecoratorB classes inherit from Decorator and add their own behavior to the Component.

We then create a simple ConcreteComponent object and decorate it with ConcreteDecoratorA and ConcreteDecoratorB. When we call the operation() method on the final decorated component, it returns a string that represents the combined behavior of all the decorators and the component itself.

---
## Python
Here's the same example in Python 3 with separate classes for Component, Decorator, and Concrete Decorator:
```py
from abc import ABC, abstractmethod

# Component interface
class Shape(ABC):
    @abstractmethod
    def draw(self):
        pass

# Concrete component
class Circle(Shape):
    def draw(self):
        return "Circle"

# Decorator
class ShapeDecorator(Shape):
    def __init__(self, decorated_shape):
        self.decorated_shape = decorated_shape
    
    def draw(self):
        return self.decorated_shape.draw()

# Concrete decorator
class RedShapeDecorator(ShapeDecorator):
    def draw(self):
        return f"Red {self.decorated_shape.draw()}"

# Client code
circle = Circle()
red_circle = RedShapeDecorator(circle)

print("Circle with normal border:")
print(circle.draw())

print("\nCircle of red border:")
print(red_circle.draw())

```
The first class Shape is an abstract class that serves as the Component interface. It has a single abstract method draw() that all concrete shapes must implement.

The Circle class is a concrete implementation of Shape. It has its own implementation of the draw() method that returns the string "Circle".

The ShapeDecorator class is the Decorator interface. It is also an abstract class that extends Shape and has an instance variable decorated_shape that is of type Shape. It also implements the draw() method by calling the draw() method of its decorated_shape instance variable.

The RedShapeDecorator class is a Concrete Decorator that extends ShapeDecorator. It has an additional instance variable color that stores the color of the shape. It overrides the draw() method of its super class to first call the draw() method of its decorated_shape instance variable, and then append the string " in Red Color" to the returned string.

The main() function in the example creates a Circle object and decorates it with a RedShapeDecorator object. It then calls the draw() method of the decorated object, which first calls the draw() method of the Circle object and then adds the string " in Red Color" to the returned string.

This implementation of the decorator pattern in Python follows the same structure and principles as the C# implementation. The main difference is the syntax and language-specific conventions.

---

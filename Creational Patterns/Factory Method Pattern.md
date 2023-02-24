Factory Method Pattern
===

The Factory Method pattern is a creational design pattern that provides an interface for creating objects, but allows subclasses to decide which class to instantiate. This means that the Factory Method pattern provides a way to delegate the responsibility of object creation to subclasses, rather than creating the objects directly in the client code.

The Factory Method pattern consists of several components:

1. Creator: an abstract class that defines the interface for creating products. This class is responsible for declaring the factory method, which returns a new instance of the Product class.

2. ConcreteCreator: a concrete subclass of Creator that implements the factory method. This class is responsible for creating the specific products.

3. Product: an abstract class that defines the interface for the objects that the factory method creates.

4. ConcreteProduct: a concrete subclass of Product that provides an implementation of the interface.

---
## C#

The Factory Method pattern is a creational design pattern that provides an interface for creating objects in a super class, but allows subclasses to alter the type of objects that will be created. In other words, it lets subclasses decide which class to instantiate.

The Factory Method pattern defines an abstract Creator class that declares a factory method for creating objects, and a ConcreteCreator class that implements the factory method to create specific products. The products are represented by an abstract Product class and its ConcreteProduct subclasses.

Here's an example of the Factory Method pattern in C#:

```cs
// Abstract Product
abstract class Product
{
    public abstract void Operation();
}

// Concrete Products
class ConcreteProductA : Product
{
    public override void Operation()
    {
        Console.WriteLine("Concrete Product A");
    }
}

class ConcreteProductB : Product
{
    public override void Operation()
    {
        Console.WriteLine("Concrete Product B");
    }
}

// Abstract Creator
abstract class Creator
{
    public abstract Product FactoryMethod();
}

// Concrete Creators
class ConcreteCreatorA : Creator
{
    public override Product FactoryMethod()
    {
        return new ConcreteProductA();
    }
}

class ConcreteCreatorB : Creator
{
    public override Product FactoryMethod()
    {
        return new ConcreteProductB();
    }
}

// Client code
class Client
{
    public void Main()
    {
        Creator creatorA = new ConcreteCreatorA();
        Product productA = creatorA.FactoryMethod();
        productA.Operation();

        Creator creatorB = new ConcreteCreatorB();
        Product productB = creatorB.FactoryMethod();
        productB.Operation();
    }
}

```
In this example, the Product class represents the abstract product and its subclasses ConcreteProductA and ConcreteProductB represent the concrete products that can be created.

The Creator class is the abstract creator and its subclasses ConcreteCreatorA and ConcreteCreatorB implement the FactoryMethod that creates a specific product.

Finally, the client code creates instances of the ConcreteCreator classes and calls their FactoryMethod to get the desired product. The client code does not need to know which concrete product it is getting, it only knows that it is getting a Product. This allows for flexibility and extensibility in the system.


---
## javascript

Here is an example implementation of the Factory Method pattern in JavaScript:
```js
// Product class
class Product {
  getDescription() {
    throw new Error('Abstract method called');
  }
}

// Concrete product classes
class ConcreteProductA extends Product {
  getDescription() {
    return 'This is a ConcreteProductA';
  }
}

class ConcreteProductB extends Product {
  getDescription() {
    return 'This is a ConcreteProductB';
  }
}

// Creator class
class Creator {
  factoryMethod() {
    throw new Error('Abstract method called');
  }

  someOperation() {
    const product = this.factoryMethod();
    return `Creator: The same creator's code has just worked with ${product.getDescription()}`;
  }
}

// Concrete creator classes
class ConcreteCreatorA extends Creator {
  factoryMethod() {
    return new ConcreteProductA();
  }
}

class ConcreteCreatorB extends Creator {
  factoryMethod() {
    return new ConcreteProductB();
  }
}

// Client code
function clientCode(creator) {
  console.log(creator.someOperation());
}

clientCode(new ConcreteCreatorA());
clientCode(new ConcreteCreatorB());

```
In this example, the Product class is an abstract class that defines the interface for the objects that the factory method creates. The ConcreteProductA and ConcreteProductB classes are concrete subclasses of Product that provide an implementation of the interface.

The Creator class is an abstract class that defines the interface for creating Product objects. The ConcreteCreatorA and ConcreteCreatorB classes are concrete subclasses of Creator that implement the factoryMethod to create specific products.

Finally, the client code creates instances of the ConcreteCreator classes and calls their someOperation method to get the desired product. The client code does not need to know which concrete product it is getting, it only knows that it is getting a Product. This allows for flexibility and extensibility in the system.


---
## Python

here is an example implementation of the Factory Method pattern in Python:



```py
# Product class
class Product:
    def get_description(self):
        raise NotImplementedError('Abstract method called')

# Concrete product classes
class ConcreteProductA(Product):
    def get_description(self):
        return 'This is a ConcreteProductA'

class ConcreteProductB(Product):
    def get_description(self):
        return 'This is a ConcreteProductB'

# Creator class
class Creator:
    def factory_method(self):
        raise NotImplementedError('Abstract method called')

    def some_operation(self):
        product = self.factory_method()
        return f'Creator: The same creator\'s code has just worked with {product.get_description()}'

# Concrete creator classes
class ConcreteCreatorA(Creator):
    def factory_method(self):
        return ConcreteProductA()

class ConcreteCreatorB(Creator):
    def factory_method(self):
        return ConcreteProductB()

# Client code
def client_code(creator):
    print(creator.some_operation())

client_code(ConcreteCreatorA())
client_code(ConcreteCreatorB())

```
In this example, the Product class is an abstract class that defines the interface for the objects that the factory method creates. The ConcreteProductA and ConcreteProductB classes are concrete subclasses of Product that provide an implementation of the interface.

The Creator class is an abstract class that defines the interface for creating Product objects. The ConcreteCreatorA and ConcreteCreatorB classes are concrete subclasses of Creator that implement the factory_method to create specific products.

Finally, the client code creates instances of the ConcreteCreator classes and calls their some_operation method to get the desired product. The client code does not need to know which concrete product it is getting, it only knows that it is getting a Product. This allows for flexibility and extensibility in the system.


---

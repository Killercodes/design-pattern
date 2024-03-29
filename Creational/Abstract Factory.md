__Abstract Factory__
===
The Abstract Factory pattern is a creational design pattern that provides an interface for creating families of related objects, without specifying their concrete classes. It allows you to create objects that are related to each other in a specific way, such as all the objects needed to create a user interface, or all the objects needed to create a database connection.

> The Abstract Factory design pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes.

The classes and objects participating in this pattern include:

- `AbstractFactory`  (ContinentFactory) declares an interface for operations that create abstract products
- `ConcreteFactory`   (AfricaFactory, AmericaFactory) implements the operations to create concrete product objects
- `AbstractProduct`   (Herbivore, Carnivore) declares an interface for a type of product object
- `Product`  (Wildebeest, Lion, Bison, Wolf) defines a product object to be created by the corresponding concrete factory implements the AbstractProduct interface
- `Client`  (AnimalWorld) uses interfaces declared by AbstractFactory and AbstractProduct classes

The main advantage of the Abstract Factory pattern is that it promotes loose coupling between the client code and the concrete classes that it uses. By using an abstract factory to create the necessary objects, the client code is shielded from the details of how these objects are created and can be easily switched between different families of related objects.

The Abstract Factory pattern is made up of several key components:
1. Abstract Factory: This is an interface that defines the methods for creating the related objects.
2. Concrete Factory: This is a class that implements the Abstract Factory interface and creates the related objects.
3. Abstract Product: This is an interface that defines the common properties and methods that the related objects must have.
4. Concrete Product: These are the actual classes that implement the Abstract Product interface and provide specific implementations of the methods.
5. Client: This is the code that uses the Abstract Factory to create the related objects.

The Abstract Factory pattern is useful when you need to create objects that are related in a specific way and want to shield the client code from the details of how these objects are created. It can also be useful when you need to create families of objects that are specific to a particular platform or environment.


## Structural code in C#

This structural code demonstrates the Abstract Factory pattern creating parallel hierarchies of objects. Object creation has been abstracted and there is no need for hard-coded class names in the client code.

```cs
using System;

namespace Creational.Abstract.Structural
{
    /// <summary>
    /// MainApp startup class for Structural
    /// Abstract Factory Design Pattern.
    /// </summary>

    class MainApp
    {
        /// <summary>
        /// Entry point into console application.
        /// </summary>

        public static void Main()
        {
            // Abstract factory #1

            AbstractFactory factory1 = new ConcreteFactory1();
            Client client1 = new Client(factory1);
            client1.Run();

            // Abstract factory #2

            AbstractFactory factory2 = new ConcreteFactory2();
            Client client2 = new Client(factory2);
            client2.Run();

            // Wait for user input

            Console.ReadKey();
        }
    }

    /// <summary>
    /// The 'AbstractFactory' abstract class
    /// </summary>

    abstract class AbstractFactory
    {
        public abstract AbstractProductA CreateProductA();
        public abstract AbstractProductB CreateProductB();
    }


    /// <summary>
    /// The 'ConcreteFactory1' class
    /// </summary>

    class ConcreteFactory1 : AbstractFactory
    {
        public override AbstractProductA CreateProductA()
        {
            return new ProductA1();
        }
        public override AbstractProductB CreateProductB()
        {
            return new ProductB1();
        }
    }

    /// <summary>
    /// The 'ConcreteFactory2' class
    /// </summary>

    class ConcreteFactory2 : AbstractFactory
    {
        public override AbstractProductA CreateProductA()
        {
            return new ProductA2();
        }
        public override AbstractProductB CreateProductB()
        {
            return new ProductB2();
        }
    }

    /// <summary>
    /// The 'AbstractProductA' abstract class
    /// </summary>

    abstract class AbstractProductA
    {
    }

    /// <summary>
    /// The 'AbstractProductB' abstract class
    /// </summary>

    abstract class AbstractProductB
    {
        public abstract void Interact(AbstractProductA a);
    }


    /// <summary>
    /// The 'ProductA1' class
    /// </summary>

    class ProductA1 : AbstractProductA
    {
    }

    /// <summary>
    /// The 'ProductB1' class
    /// </summary>

    class ProductB1 : AbstractProductB
    {
        public override void Interact(AbstractProductA a)
        {
            Console.WriteLine(this.GetType().Name +
              " interacts with " + a.GetType().Name);
        }
    }

    /// <summary>
    /// The 'ProductA2' class
    /// </summary>

    class ProductA2 : AbstractProductA
    {
    }

    /// <summary>
    /// The 'ProductB2' class
    /// </summary>

    class ProductB2 : AbstractProductB
    {
        public override void Interact(AbstractProductA a)
        {
            Console.WriteLine(this.GetType().Name +
              " interacts with " + a.GetType().Name);
        }
    }

    /// <summary>
    /// The 'Client' class. Interaction environment for the products.
    /// </summary>

    class Client
    {
        private AbstractProductA _abstractProductA;
        private AbstractProductB _abstractProductB;

        // Constructor

        public Client(AbstractFactory factory)
        {
            _abstractProductB = factory.CreateProductB();
            _abstractProductA = factory.CreateProductA();
        }

        public void Run()
        {
            _abstractProductB.Interact(_abstractProductA);
        }
    }
}

```

## Example 2

In this implementation, we define two interfaces IProductA and IProductB that represent the abstract products that our factories will produce. We also define an IAbstractFactory interface that defines the methods for creating these products.

We then create concrete implementations of these interfaces, such as ConcreteProductA1, ConcreteProductB1, ConcreteProductA2, and ConcreteProductB2, and two concrete factories ConcreteFactory1 and ConcreteFactory2 that implement the IAbstractFactory interface.



```cs
using System;

interface IProductA
{
    string GetName();
}

interface IProductB
{
    string GetName();
}

interface IAbstractFactory
{
    IProductA CreateProductA();
    IProductB CreateProductB();
}

class ConcreteProductA1 : IProductA
{
    public string GetName()
    {
        return "Product A1";
    }
}

class ConcreteProductB1 : IProductB
{
    public string GetName()
    {
        return "Product B1";
    }
}

class ConcreteProductA2 : IProductA
{
    public string GetName()
    {
        return "Product A2";
    }
}

class ConcreteProductB2 : IProductB
{
    public string GetName()
    {
        return "Product B2";
    }
}

class ConcreteFactory1 : IAbstractFactory
{
    public IProductA CreateProductA()
    {
        return new ConcreteProductA1();
    }

    public IProductB CreateProductB()
    {
        return new ConcreteProductB1();
    }
}

class ConcreteFactory2 : IAbstractFactory
{
    public IProductA CreateProductA()
    {
        return new ConcreteProductA2();
    }

    public IProductB CreateProductB()
    {
        return new ConcreteProductB2();
    }
}

class Client
{
    private IAbstractFactory factory;

    public Client(IAbstractFactory factory)
    {
        this.factory = factory;
    }

    public void Run()
    {
        IProductA productA = factory.CreateProductA();
        IProductB productB = factory.CreateProductB();

        Console.WriteLine(productA.GetName());
        Console.WriteLine(productB.GetName());
    }
}

class Program
{
    static void Main(string[] args)
    {
        IAbstractFactory factory1 = new ConcreteFactory1();
        IAbstractFactory factory2 = new ConcreteFactory2();

        Client client1 = new Client(factory1);
        Client client2 = new Client(factory2);

        client1.Run();
        client2.Run();
    }
}


```

Finally, we create a Client class that takes an IAbstractFactory object in its constructor and uses it to create the necessary products. We create two instances of Client, one with ConcreteFactory1 and one with ConcreteFactory2, and call their Run methods.

When we run this program, we get the following output:
```
Product A1
Product B1
Product A2
Product B2
```
As you can see, each client is able to create the appropriate products from its assigned factory. This allows us to easily switch between different families of related products by simply using a different factory object.

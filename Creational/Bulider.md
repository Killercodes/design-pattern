__Bulider__
===
The Builder pattern is a creational design pattern that separates the construction of a complex object from its representation, allowing the same construction process to create different representations. It is useful when you need to create complex objects that require a lot of configuration, but you want to shield the client code from the details of how these objects are built.

The Builder pattern is a creational design pattern that allows you to create complex objects step by step, without requiring you to know the details of the implementation. It separates the construction of a complex object from its representation, allowing different representations to be created depending on the needs of the client.

> The Builder design pattern separates the construction of a complex object from its representation so that the same construction process can create different representations.

The Builder pattern is useful when you need to create complex objects that require a lot of configuration. It allows you to separate the construction of the object from its representation, so that you can use the same construction process to create different representations. This can be useful when you need to create objects for different platforms or environments.

One of the benefits of using the Builder pattern is that it promotes a clear separation of concerns between the client code, the builder, and the director. The client code is responsible for creating the director and passing it a builder, but it does not need to know the details of how the builder works. The builder is responsible for creating the parts of the product, but it does not need to know how those parts will be assembled into the final product. The director is responsible for managing the building process, but it does not need to know how the parts are created.

Another benefit of the Builder pattern is that it can improve the maintainability of your code. By separating the construction of the object from its representation, you can modify the construction process without affecting the client code. This makes it easier to change the behavior of your application over time, as you can modify the builder or the director without having to change the client code.




## Structural code in C#

This structural code demonstrates demonstrates the Builder pattern in which complex objects are created in a step-by-step fashion. The construction process can create different object representations and provides a high level of control over the assembly of the objects.

The classes and objects participating in this pattern include:

- `Builder`  (VehicleBuilder)
  - specifies an abstract interface for creating parts of a Product object
- `ConcreteBuilder`  (MotorCycleBuilder, CarBuilder, ScooterBuilder)
  - constructs and assembles parts of the product by implementing the Builder interface
  - defines and keeps track of the representation it creates
  - provides an interface for retrieving the product
- `Director`  (Shop)
  - constructs an object using the Builder interface
- `Product`  (Vehicle)
  - represents the complex object under construction. ConcreteBuilder builds the product's internal representation and defines the process by which it's assembled
  - includes classes that define the constituent parts, including interfaces for assembling the parts into the final result

```cs
using System;
using System.Collections.Generic;

namespace Creational.Builder.Structure
{
    /// <summary>
    /// MainApp startup class for Structural
    /// Builder Design Pattern.
    /// </summary>

    public class MainApp
    {
        /// <summary>
        /// Entry point into console application.
        /// </summary>

        public static void Main()
        {
            // Create director and builders

            Director director = new Director();

            Builder b1 = new ConcreteBuilder1();
            Builder b2 = new ConcreteBuilder2();

            // Construct two products

            director.Construct(b1);
            Product p1 = b1.GetResult();
            p1.Show();

            director.Construct(b2);
            Product p2 = b2.GetResult();
            p2.Show();

            // Wait for user

            Console.ReadKey();
        }
    }

    /// <summary>
    /// The 'Director' class
    /// </summary>

    class Director
    {
        // Builder uses a complex series of steps

        public void Construct(Builder builder)
        {
            builder.BuildPartA();
            builder.BuildPartB();
        }
    }

    /// <summary>
    /// The 'Builder' abstract class
    /// </summary>

    abstract class Builder
    {
        public abstract void BuildPartA();
        public abstract void BuildPartB();
        public abstract Product GetResult();
    }

    /// <summary>
    /// The 'ConcreteBuilder1' class
    /// </summary>

    class ConcreteBuilder1 : Builder
    {
        private Product _product = new Product();

        public override void BuildPartA()
        {
            _product.Add("PartA");
        }

        public override void BuildPartB()
        {
            _product.Add("PartB");
        }

        public override Product GetResult()
        {
            return _product;
        }
    }

    /// <summary>
    /// The 'ConcreteBuilder2' class
    /// </summary>

    class ConcreteBuilder2 : Builder
    {
        private Product _product = new Product();

        public override void BuildPartA()
        {
            _product.Add("PartX");
        }

        public override void BuildPartB()
        {
            _product.Add("PartY");
        }

        public override Product GetResult()
        {
            return _product;
        }
    }

    /// <summary>
    /// The 'Product' class
    /// </summary>

    class Product
    {
        private List<string> _parts = new List<string>();

        public void Add(string part)
        {
            _parts.Add(part);
        }

        public void Show()
        {
            Console.WriteLine("\nProduct Parts -------");
            foreach (string part in _parts)
                Console.WriteLine(part);
        }
    }
}

```

## C#
The Builder pattern consists of several key components:
1. Product: This is the complex object being built.
2. Builder: This is an interface that specifies the methods for creating the parts of the product.
3. Concrete Builder: This is a class that implements the Builder interface and provides concrete implementations of the methods for creating the parts of the product.
4. Director: This is a class that manages the building process, using the methods provided by the Builder interface to create the product.
5. Client: This is the code that creates the Director and passes it a Concrete Builder to build the product.


In C#, the Builder pattern is implemented using an interface and a concrete builder class that implements the interface. The client code interacts with the builder object, specifying the desired configuration for the object being built. The builder object then creates and returns the final object to the client.

Here's an example of how to implement the Builder pattern in C#:
```cs
// Define the product that will be built
public class Product
{
    public string PartA { get; set; }
    public string PartB { get; set; }
    public string PartC { get; set; }
}

// Define the builder interface
public interface IBuilder
{
    void BuildPartA();
    void BuildPartB();
    void BuildPartC();
    Product GetResult();
}

// Define the concrete builder class
public class ConcreteBuilder : IBuilder
{
    private Product _product = new Product();

    public void BuildPartA()
    {
        _product.PartA = "Part A";
    }

    public void BuildPartB()
    {
        _product.PartB = "Part B";
    }

    public void BuildPartC()
    {
        _product.PartC = "Part C";
    }

    public Product GetResult()
    {
        return _product;
    }
}

// Define the director class
public class Director
{
    private IBuilder _builder;

    public Director(IBuilder builder)
    {
        _builder = builder;
    }

    public void Construct()
    {
        _builder.BuildPartA();
        _builder.BuildPartB();
        _builder.BuildPartC();
    }
}

// Client code
public static void Main()
{
    var builder = new ConcreteBuilder();
    var director = new Director(builder);

    director.Construct();
    var product = builder.GetResult();

    Console.WriteLine(product.PartA);
    Console.WriteLine(product.PartB);
    Console.WriteLine(product.PartC);
}

```

In this example, the client code creates a ConcreteBuilder object and passes it to the Director object. The Director object then uses the ConcreteBuilder object to create the desired Product object, which is returned to the client code. The client code can then access the individual parts of the Product object as needed.

The Builder pattern is useful when you need to create complex objects that require a lot of configuration, but you want to shield the client code from the details of how these objects are built. It also allows you to create multiple representations of the same object, which can be useful in situations where you need to create objects for different platforms or environments.


---

## Javascript
here's an example of the Builder pattern in JavaScript:

```js
// Product
class Car {
  constructor() {
    this.make = "";
    this.model = "";
    this.year = "";
    this.color = "";
    this.engineSize = "";
  }

  describe() {
    return `${this.make} ${this.model} (${this.year}) in ${this.color} with a ${this.engineSize}L engine`;
  }
}

// Builder
class CarBuilder {
  constructor() {
    this.car = new Car();
  }

  setMake(make) {
    this.car.make = make;
    return this;
  }

  setModel(model) {
    this.car.model = model;
    return this;
  }

  setYear(year) {
    this.car.year = year;
    return this;
  }

  setColor(color) {
    this.car.color = color;
    return this;
  }

  setEngineSize(engineSize) {
    this.car.engineSize = engineSize;
    return this;
  }

  build() {
    return this.car;
  }
}

// Director
class CarDirector {
  static buildSportscar() {
    return new CarBuilder()
      .setMake("Porsche")
      .setModel("911")
      .setYear("2022")
      .setColor("red")
      .setEngineSize("3.0")
      .build();
  }

  static buildSedan() {
    return new CarBuilder()
      .setMake("Honda")
      .setModel("Accord")
      .setYear("2022")
      .setColor("blue")
      .setEngineSize("2.0")
      .build();
  }
}

// Client
const sportscar = CarDirector.buildSportscar();
const sedan = CarDirector.buildSedan();

console.log(sportscar.describe());
console.log(sedan.describe());

```
In this example, we have a Car class representing the product we want to build, a CarBuilder class implementing the builder interface, and a CarDirector class managing the building process.

The CarBuilder class has methods for setting the different properties of the car and a build method that returns the final product.

The CarDirector class has methods for building different types of cars, using the methods of the CarBuilder to set the properties of the car.

Finally, the client code uses the CarDirector to build different types of cars and then displays their descriptions using the describe method of the Car class.

---

## Python
An example of the Builder pattern in Python:

```py
class Car:
    def __init__(self):
        self.make = ""
        self.model = ""
        self.year = ""
        self.color = ""
        self.engine_size = ""

    def describe(self):
        return f"{self.make} {self.model} ({self.year}) in {self.color} with a {self.engine_size}L engine"


class CarBuilder:
    def __init__(self):
        self.car = Car()

    def set_make(self, make):
        self.car.make = make
        return self

    def set_model(self, model):
        self.car.model = model
        return self

    def set_year(self, year):
        self.car.year = year
        return self

    def set_color(self, color):
        self.car.color = color
        return self

    def set_engine_size(self, engine_size):
        self.car.engine_size = engine_size
        return self

    def build(self):
        return self.car


class CarDirector:
    @staticmethod
    def build_sportscar():
        return CarBuilder() \
            .set_make("Porsche") \
            .set_model("911") \
            .set_year("2022") \
            .set_color("red") \
            .set_engine_size("3.0") \
            .build()

    @staticmethod
    def build_sedan():
        return CarBuilder() \
            .set_make("Honda") \
            .set_model("Accord") \
            .set_year("2022") \
            .set_color("blue") \
            .set_engine_size("2.0") \
            .build()


# Client
sportscar = CarDirector.build_sportscar()
sedan = CarDirector.build_sedan()

print(sportscar.describe())
print(sedan.describe())

```
In this example, we have a Car class representing the product we want to build, a CarBuilder class implementing the builder interface, and a CarDirector class managing the building process.

The CarBuilder class has methods for setting the different properties of the car and a build method that returns the final product.

The CarDirector class has methods for building different types of cars, using the methods of the CarBuilder to set the properties of the car.

Finally, the client code uses the CarDirector to build different types of cars and then displays their descriptions using the describe method of the Car class.






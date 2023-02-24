Facade Pattern
===
The Facade pattern is a software design pattern that provides a unified interface to a set of interfaces in a subsystem, making it easier to use and reducing the complexity of the overall system.

In other words, the Facade pattern provides a simplified interface to a larger and more complex body of code, such as a class library or an entire software system, by creating a higher-level interface that hides the implementation details of the underlying system.

The main benefits of using the Facade pattern are:

1. Simplifies the use of a complex system: By providing a simplified interface, the Facade pattern makes it easier for clients to use a complex system without having to understand the details of the subsystem.

2. Encapsulates the system: The Facade pattern encapsulates the subsystem behind a single interface, which reduces the complexity of the overall system and makes it easier to modify or replace the underlying implementation.

3. Improves maintainability: By reducing the coupling between the subsystem and its clients, the Facade pattern makes it easier to modify or extend the subsystem without affecting the clients.

Some common examples of the Facade pattern include:

1. Operating System APIs: Operating systems such as Windows and macOS provide APIs that allow developers to interact with the underlying system. The APIs are often complex and difficult to use, so the operating system may provide a Facade that simplifies the API and makes it easier for developers to use.

2. Software libraries: Many software libraries provide a Facade that simplifies the API and makes it easier for developers to use. For example, a graphics library may provide a Facade that simplifies the process of drawing shapes and images on a screen.

3. Web services: Web services often provide a Facade that simplifies the API and makes it easier for developers to interact with the service. For example, a web service that provides weather data may provide a Facade that simplifies the process of requesting and receiving the data.
---

## c#
Here's a single-file example of the Facade pattern in C#:

```cs
using System;

// The Facade class provides a simple interface for the client code. It hides
// the complexity of the subsystems and delegates the client requests to the
// appropriate objects within the subsystems.
class Facade
{
    private Subsystem1 _subsystem1;
    private Subsystem2 _subsystem2;

    public Facade(Subsystem1 subsystem1, Subsystem2 subsystem2)
    {
        this._subsystem1 = subsystem1;
        this._subsystem2 = subsystem2;
    }

    public string Operation()
    {
        string result = "Facade initializes subsystems:\n";
        result += this._subsystem1.Operation1();
        result += this._subsystem2.Operation1();
        result += "Facade orders subsystems to perform the action:\n";
        result += this._subsystem1.OperationN();
        result += this._subsystem2.OperationZ();
        return result;
    }
}

// The Subsystem can accept requests either from the facade or client directly.
// In any case, to the Subsystem, the Facade is yet another client, and it's
// not a part of the Subsystem.
class Subsystem1
{
    public string Operation1()
    {
        return "Subsystem1: Ready!\n";
    }

    public string OperationN()
    {
        return "Subsystem1: Go!\n";
    }
}

// Some facades can work with multiple subsystems at the same time.
class Subsystem2
{
    public string Operation1()
    {
        return "Subsystem2: Get ready!\n";
    }

    public string OperationZ()
    {
        return "Subsystem2: Fire!\n";
    }
}

// The client code works with complex subsystems through a simple interface
// provided by the Facade. When a facade manages the lifecycle of the subsystem,
// the client might not even know about the existence of the subsystem. This
// approach lets you keep the complexity under

```
In this example, we have a Facade class that provides a simple interface for the client code. It hides the complexity of the subsystems and delegates the client requests to the appropriate objects within the subsystems. The subsystems are implemented as Subsystem1 and Subsystem2 classes, which represent different parts of a complex system.

The Facade class takes references to Subsystem1 and Subsystem2 objects in its constructor, and uses them to delegate work to the subsystems as needed. The Operation() method of the Facade class orchestrates the interaction between the subsystems by calling their methods in a specific order.

The Client class demonstrates how the Facade can be used by the client code to interact with the subsystems. The client creates instances of the subsystems and passes them to the Facade's constructor. Then, the client calls the Operation() method on the Facade to initiate the complex system operation.

Overall, the Facade pattern provides a simplified interface to a complex system, making it easier for clients to interact with the system without worrying about its inner workings.


---
## Javascript
Here's the same example in JavaScript:
```js
class SubsystemA {
  operationA() {
    console.log("Subsystem A operation");
  }
}

class SubsystemB {
  operationB() {
    console.log("Subsystem B operation");
  }
}

class SubsystemC {
  operationC() {
    console.log("Subsystem C operation");
  }
}

class Facade {
  constructor() {
    this.subsystemA = new SubsystemA();
    this.subsystemB = new SubsystemB();
    this.subsystemC = new SubsystemC();
  }

  operation1() {
    console.log("Operation 1\n" + "- - - - - - - - - - - - - - -");
    this.subsystemA.operationA();
    this.subsystemB.operationB();
  }

  operation2() {
    console.log("Operation 2\n" + "- - - - - - - - - - - - - - -");
    this.subsystemB.operationB();
    this.subsystemC.operationC();
  }
}

// Example usage:
const facade = new Facade();
facade.operation1();
facade.operation2();

```
In this example, we have three subsystems (SubsystemA, SubsystemB, and SubsystemC) that perform various operations. We also have a Facade class that provides a simple interface to these subsystems by hiding their complexity behind a single interface. The Facade class is responsible for managing the interactions between the subsystems and providing higher-level functionality to clients.

To use the Facade, clients simply create a new instance and call the appropriate methods (operation1 and operation2). The Facade takes care of calling the appropriate subsystem methods in the correct order to perform the desired operations.

---
## Python
here's an example of the Facade pattern in Python with two subsystems represented as classes:

```py
class Subsystem1:
    def operation1(self):
        return "Subsystem 1: Doing operation 1"

    def operation2(self):
        return "Subsystem 1: Doing operation 2"


class Subsystem2:
    def operation1(self):
        return "Subsystem 2: Doing operation 1"

    def operation2(self):
        return "Subsystem 2: Doing operation 2"


class Facade:
    def __init__(self):
        self._subsystem1 = Subsystem1()
        self._subsystem2 = Subsystem2()

    def operation(self):
        result = "Facade initializes subsystems:\n"
        result += self._subsystem1.operation1() + "\n"
        result += self._subsystem2.operation1() + "\n"
        result += "Facade orders subsystems to perform the action:\n"
        result += self._subsystem1.operation2() + "\n"
        result += self._subsystem2.operation2() + "\n"
        return result


def client_code(facade: Facade) -> None:
    print(facade.operation())


if __name__ == "__main__":
    facade = Facade()
    client_code(facade)

```
In this example, Subsystem1 and Subsystem2 represent complex subsystems with their own internal logic and operations. The Facade class serves as a simplified interface to these subsystems, providing a single entry point for the client to interact with.

The Facade class creates instances of the subsystems and exposes a single operation() method to the client. This method internally calls the appropriate methods on each of the subsystems to achieve the desired functionality.

The client_code() function is responsible for demonstrating the use of the Facade. In this case, it creates an instance of the Facade class and calls its operation() method to perform a series of operations on the subsystems.


---

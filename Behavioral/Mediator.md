Mediator Design Pattern
===

The Mediator pattern is a behavioral design pattern that allows communication between objects without them having to directly reference each other. It promotes loose coupling and reduces dependencies between objects, making it easier to maintain and extend the code.


> **Usage examples:** The most popular usage of the Mediator pattern in C# code is facilitating communications between GUI components of an app. The synonym of the Mediator is the Controller part of MVC pattern.

The Mediator pattern defines an object (the mediator) that encapsulates the communication between a set of objects (the colleagues). The colleagues only communicate with the mediator, which then handles the communication between them.

The Mediator pattern is useful in situations where a complex system of objects needs to communicate with each other, but direct communication between these objects would result in a tightly coupled system that's difficult to maintain and modify.



In summary, the Mediator pattern provides a way to decouple objects and promote communication through a central object, resulting in a more flexible and maintainable system.

- Some common examples of Mediator pattern include chat rooms, air traffic control systems, and event-driven systems.

- In the Mediator pattern, each object doesn't communicate with other objects directly but through the mediator object. The mediator object acts as a central hub that receives requests from one object and sends it to other objects involved in the request. The pattern promotes decoupling and flexibility in a system, allowing for easier maintenance and modification.

- The Mediator pattern is useful in scenarios where many objects need to interact with each other but direct coupling between them would create a complex and tangled system. It is also useful when adding new objects to the system or modifying existing ones.

- Some common examples of the Mediator pattern in practice include chat rooms, where users communicate with each other through a central server, and air traffic control systems, where planes communicate with each other through a central system.


## Example 1

```cs
// The Mediator interface declares a method used by components to notify the
// mediator about various events. The Mediator may react to these events and
// pass the execution to other components.
public interface IMediator
{
    void Notify(object sender, string ev);
}

// Concrete Mediators implement cooperative behavior by coordinating several
// components.
class ConcreteMediator : IMediator
{
    private Component1 _component1;

    private Component2 _component2;

    public ConcreteMediator(Component1 component1, Component2 component2)
    {
        this._component1 = component1;
        this._component1.SetMediator(this);
        this._component2 = component2;
        this._component2.SetMediator(this);
    } 

    public void Notify(object sender, string ev)
    {
        if (ev == "A")
        {
            Console.WriteLine("Mediator reacts on A and triggers folowing operations:");
            this._component2.DoC();
        }
        if (ev == "D")
        {
            Console.WriteLine("Mediator reacts on D and triggers following operations:");
            this._component1.DoB();
            this._component2.DoC();
        }
    }
}

// The Base Component provides the basic functionality of storing a
// mediator's instance inside component objects.
class BaseComponent
{
    protected IMediator _mediator;

    public BaseComponent(IMediator mediator = null)
    {
        this._mediator = mediator;
    }

    public void SetMediator(IMediator mediator)
    {
        this._mediator = mediator;
    }
}

// Concrete Components implement various functionality. They don't depend on
// other components. They also don't depend on any concrete mediator
// classes.
class Component1 : BaseComponent
{
    public void DoA()
    {
        Console.WriteLine("Component 1 does A.");

        this._mediator.Notify(this, "A");
    }

    public void DoB()
    {
        Console.WriteLine("Component 1 does B.");

        this._mediator.Notify(this, "B");
    }
}

class Component2 : BaseComponent
{
    public void DoC()
    {
        Console.WriteLine("Component 2 does C.");

        this._mediator.Notify(this, "C");
    }

    public void DoD()
    {
        Console.WriteLine("Component 2 does D.");

        this._mediator.Notify(this, "D");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code.
        Component1 component1 = new Component1();
        Component2 component2 = new Component2();
        new ConcreteMediator(component1, component2);

        Console.WriteLine("Client triggers operation A.");
        component1.DoA();

        Console.WriteLine();

        Console.WriteLine("Client triggers operation D.");
        component2.DoD();
    }
}
```



## Example 2
The Mediator pattern typically consists of the following components:
- Mediator: defines the interface for communicating with Colleague objects.
- ConcreteMediator: implements the Mediator interface and maintains a reference to all Colleague objects.
- Colleague: defines the interface for communicating with the Mediator object.
- ConcreteColleague: implements the Colleague interface and communicates with other Colleague objects through the Mediator object.
- By using the Mediator pattern, communication between objects can be simplified, resulting in a more modular and maintainable system.


### C#

```cs
using System;

// Mediator interface that defines communication between colleagues
interface IMediator
{
    void SendMessage(Colleague colleague, string message);
}

// Concrete mediator that implements communication between colleagues
class ConcreteMediator : IMediator
{
    private Colleague1 colleague1;
    private Colleague2 colleague2;

    public Colleague1 Colleague1
    {
        set { colleague1 = value; }
    }

    public Colleague2 Colleague2
    {
        set { colleague2 = value; }
    }

    public void SendMessage(Colleague colleague, string message)
    {
        if (colleague == colleague1)
        {
            colleague2.ReceiveMessage(message);
        }
        else
        {
            colleague1.ReceiveMessage(message);
        }
    }
}

// Abstract colleague class
abstract class Colleague
{
    protected IMediator mediator;

    public Colleague(IMediator mediator)
    {
        this.mediator = mediator;
    }

    public abstract void SendMessage(string message);

    public abstract void ReceiveMessage(string message);
}

// Concrete colleague class 1
class Colleague1 : Colleague
{
    public Colleague1(IMediator mediator) : base(mediator)
    {

    }

    public override void SendMessage(string message)
    {
        mediator.SendMessage(this, message);
    }

    public override void ReceiveMessage(string message)
    {
        Console.WriteLine("Colleague1 received message: " + message);
    }
}

// Concrete colleague class 2
class Colleague2 : Colleague
{
    public Colleague2(IMediator mediator) : base(mediator)
    {

    }

    public override void SendMessage(string message)
    {
        mediator.SendMessage(this, message);
    }

    public override void ReceiveMessage(string message)
    {
        Console.WriteLine("Colleague2 received message: " + message);
    }
}

// Client class that uses the colleagues and mediator
class Client
{
    static void Main(string[] args)
    {
        ConcreteMediator mediator = new ConcreteMediator();

        Colleague1 colleague1 = new Colleague1(mediator);
        Colleague2 colleague2 = new Colleague2(mediator);

        mediator.Colleague1 = colleague1;
        mediator.Colleague2 = colleague2;

        colleague1.SendMessage("Hello from colleague1!");
        colleague2.SendMessage("Hello from colleague2!");
    }
}

```
In this example, we have three main components: the Mediator, Colleague, and Client.

The Mediator interface defines the communication protocol between colleagues. In this case, it defines a SendMessage() method that takes a Colleague object and a message.

The ConcreteMediator class is a concrete implementation of the IMediator interface. It keeps track of two colleagues and passes messages between them based on which colleague is sending the message.

The Colleague class is an abstract class that defines the basic properties and behavior of a colleague. It takes an IMediator object in its constructor and defines abstract SendMessage() and ReceiveMessage() methods.

The Colleague1 and Colleague2 classes are concrete implementations of the Colleague class. They both override the SendMessage() and ReceiveMessage() methods to send and receive messages through the mediator.

Finally, the Client class creates the concrete mediator and colleagues, sets the colleagues on the mediator, and sends messages between the colleagues to test the communication between them.

This example shows how the Mediator pattern can be used to decouple communication between objects and promote more flexible and reusable code.

### Javascript

```js
class Mediator {
  constructor() {
    this.colleagues = [];
  }

  addColleague(colleague) {
    this.colleagues.push(colleague);
  }

  send(sender, message) {
    this.colleagues.forEach((colleague) => {
      if (colleague !== sender) {
        colleague.receive(message);
      }
    });
  }
}

class Colleague {
  constructor(mediator) {
    this.mediator = mediator;
  }

  send(message) {
    this.mediator.send(this, message);
  }

  receive(message) {
    console.log(`Received message: ${message}`);
  }
}

class ConcreteColleague1 extends Colleague {
  constructor(mediator) {
    super(mediator);
  }

  send(message) {
    console.log(`ConcreteColleague1 sends message: ${message}`);
    super.send(message);
  }

  receive(message) {
    console.log(`ConcreteColleague1 receives message: ${message}`);
  }
}

class ConcreteColleague2 extends Colleague {
  constructor(mediator) {
    super(mediator);
  }

  send(message) {
    console.log(`ConcreteColleague2 sends message: ${message}`);
    super.send(message);
  }

  receive(message) {
    console.log(`ConcreteColleague2 receives message: ${message}`);
  }
}

const mediator = new Mediator();
const colleague1 = new ConcreteColleague1(mediator);
const colleague2 = new ConcreteColleague2(mediator);

mediator.addColleague(colleague1);
mediator.addColleague(colleague2);

colleague1.send('Hello from colleague 1');
colleague2.send('Hi from colleague 2');

```


### Python
```py
class Mediator:
    def __init__(self):
        self.colleagues = []

    def add_colleague(self, colleague):
        self.colleagues.append(colleague)

    def send(self, sender, message):
        for colleague in self.colleagues:
            if colleague != sender:
                colleague.receive(message)


class Colleague:
    def __init__(self, mediator):
        self.mediator = mediator

    def send(self, message):
        self.mediator.send(self, message)

    def receive(self, message):
        print(f"Received message: {message}")


class ConcreteColleague1(Colleague):
    def __init__(self, mediator):
        super().__init__(mediator)

    def send(self, message):
        print(f"ConcreteColleague1 sends message: {message}")
        super().send(message)

    def receive(self, message):
        print(f"ConcreteColleague1 receives message: {message}")


class ConcreteColleague2(Colleague):
    def __init__(self, mediator):
        super().__init__(mediator)

    def send(self, message):
        print(f"ConcreteColleague2 sends message: {message}")
        super().send(message)

    def receive(self, message):
        print(f"ConcreteColleague2 receives message: {message}")


mediator = Mediator()
colleague1 = ConcreteColleague1(mediator)
colleague2 = ConcreteColleague2(mediator)

mediator.add_colleague(colleague1)
mediator.add_colleague(colleague2)

colleague1.send("Hello from colleague 1")
colleague2.send("Hi from colleague 2")

```



Flyweight Pattern
===
The Flyweight pattern is a structural design pattern that allows efficient sharing of objects that have a large number of shared state among them. This pattern aims to minimize memory usage and improve performance by reusing already created objects instead of creating new ones.

The Flyweight pattern divides objects into two categories: intrinsic and extrinsic. Intrinsic state is shared between objects and is stored in the Flyweight object, while extrinsic state is unique to each object and is stored outside of the Flyweight object.

The Flyweight pattern consists of the following components:
1. Flyweight: This is an interface or abstract class that defines the shared state and behavior of the Flyweight objects.
2. Concrete Flyweight: This is a class that implements the Flyweight interface and contains the intrinsic state. Multiple instances of the Concrete Flyweight class can be shared among different contexts.
3. Flyweight Factory: This is a factory class that creates and manages the Flyweight objects. It ensures that Flyweight objects are shared properly by caching and returning existing objects, or creating new ones if necessary.
4. Client: This is a class that uses the Flyweight objects.

The Flyweight pattern is often used in situations where a large number of similar objects need to be created, and memory usage and performance are a concern. It is commonly used in game programming to manage game assets such as textures and meshes, where the same asset is used in multiple instances throughout the game.

---

## c#
here's a simple example of the Flyweight pattern in C#:
```cs
using System;
using System.Collections.Generic;

public class Flyweight
{
    private string sharedState;

    public Flyweight(string sharedState)
    {
        this.sharedState = sharedState;
    }

    public void Operation(string uniqueState)
    {
        Console.WriteLine($"Flyweight: Displaying shared ({this.sharedState}) and unique ({uniqueState}) state.");
    }
}

public class FlyweightFactory
{
    private Dictionary<string, Flyweight> flyweights = new Dictionary<string, Flyweight>();

    public Flyweight GetFlyweight(string sharedState)
    {
        if (!flyweights.ContainsKey(sharedState))
        {
            flyweights.Add(sharedState, new Flyweight(sharedState));
        }

        return flyweights[sharedState];
    }

    public int GetFlyweightCount()
    {
        return flyweights.Count;
    }
}

public class Client
{
    private FlyweightFactory factory;

    public Client(FlyweightFactory factory)
    {
        this.factory = factory;
    }

    public void DoWork(string sharedState, string uniqueState)
    {
        Flyweight flyweight = factory.GetFlyweight(sharedState);
        flyweight.Operation(uniqueState);
    }
}

public class Program
{
    public static void Main()
    {
        FlyweightFactory factory = new FlyweightFactory();
        Client client1 = new Client(factory);
        Client client2 = new Client(factory);
        Client client3 = new Client(factory);

        client1.DoWork("A", "1");
        client2.DoWork("B", "2");
        client3.DoWork("A", "3");

        Console.WriteLine($"Number of flyweights created: {factory.GetFlyweightCount()}");
    }
}

```
In this example, we have a Flyweight class that has a shared state and an Operation method that takes a unique state as a parameter. We also have a FlyweightFactory class that keeps track of existing flyweights based on their shared state.

The Client class uses the FlyweightFactory to get a flyweight with the desired shared state and then calls the Operation method with a unique state.

In the Main method, we create three clients that use the FlyweightFactory to create and use flyweights. At the end, we output the number of flyweights created by the factory.

This example demonstrates how the Flyweight pattern can reduce memory usage by sharing objects with similar states instead of creating new objects for each unique state.

---
## Javascript
here's the same example in JavaScript:
```js
// Flyweight Factory
class FlyweightFactory {
  constructor() {
    this.flyweights = {};
  }

  getFlyweight(key) {
    if (!this.flyweights[key]) {
      this.flyweights[key] = new Flyweight(key);
    }
    return this.flyweights[key];
  }

  getCount() {
    return Object.keys(this.flyweights).length;
  }
}

// Flyweight
class Flyweight {
  constructor(sharedState) {
    this.sharedState = sharedState;
  }

  operation(uniqueState) {
    console.log(`Flyweight: Displaying shared (${this.sharedState}) and unique (${uniqueState}) state.`);
  }
}

// Client
class Client {
  constructor(flyweightFactory) {
    this.flyweightFactory = flyweightFactory;
    this.flyweights = {};
  }

  addFlyweight(key, sharedState) {
    this.flyweights[key] = this.flyweightFactory.getFlyweight(sharedState);
  }

  listFlyweights() {
    const count = this.flyweightFactory.getCount();
    console.log(`Client: I have ${count} flyweights:`);
    for (const key in this.flyweights) {
      console.log(key);
    }
  }

  operation() {
    console.log('Client: Operations with the flyweights:');
    for (const key in this.flyweights) {
      this.flyweights[key].operation(Date.now());
    }
  }
}

// Usage
const factory = new FlyweightFactory();

const client1 = new Client(factory);
client1.addFlyweight('1', 'Red');
client1.addFlyweight('2', 'Blue');
client1.listFlyweights();
client1.operation();

const client2 = new Client(factory);
client2.addFlyweight('3', 'Red');
client2.addFlyweight('4', 'Blue');
client2.listFlyweights();
client2.operation();

```
This code has the same functionality as the C# example provided earlier, but is written in JavaScript. 
It defines the FlyweightFactory, Flyweight, and Client classes and implements the flyweight pattern. 
The Client class is responsible for using the flyweights obtained from the FlyweightFactory to execute operations.


---
## Python
here's an example of the Flyweight pattern in Python
```py
from typing import Dict

class Flyweight:
    def __init__(self, shared_state):
        self.shared_state = shared_state
    
    def operation(self, unique_state):
        print(f"Flyweight: Displaying shared ({self.shared_state}) and unique ({unique_state}) state.")

class FlyweightFactory:
    flyweights: Dict[str, Flyweight] = {}

    def __init__(self, initial_flyweights):
        for state in initial_flyweights:
            self.flyweights[self.get_key(state)] = Flyweight(state)

    def get_key(self, state):
        return "_".join(sorted(state))

    def get_flyweight(self, shared_state):
        key = self.get_key(shared_state)

        if not self.flyweights.get(key):
            print("FlyweightFactory: Can't find a flyweight, creating new one.")
            self.flyweights[key] = Flyweight(shared_state)
        else:
            print("FlyweightFactory: Reusing existing flyweight.")

        return self.flyweights[key]

class Client:
    def __init__(self, factory: FlyweightFactory):
        self.factory = factory
        self.flyweights = []

    def operation(self, shared_state):
        flyweight = self.factory.get_flyweight(shared_state)
        self.flyweights.append(flyweight)
        flyweight.operation(len(self.flyweights))

if __name__ == "__main__":
    factory = FlyweightFactory([["1", "Red"], ["2", "Blue"]])

    client1 = Client(factory)
    client2 = Client(factory)

    client1.operation(["1", "Red"])
    client2.operation(["2", "Blue"])
    client1.operation(["2", "Blue"])
    client2.operation(["1", "Red"])

```
This example defines a Flyweight class with a shared state and an operation that takes a unique state. The FlyweightFactory class manages a dictionary of Flyweight instances, and clients can request a flyweight with a given shared state. If the requested flyweight doesn't exist, the factory creates a new one; otherwise, it returns the existing flyweight. The Client class stores the flyweights it receives and calls their operations with a unique state.

In the main program, a FlyweightFactory instance is created with two initial flyweights, "1_Red" and "2_Blue". Two Client instances are then created, and each one calls the operation method twice with different shared states. The output of the program shows that the first time a given shared state is requested, a new flyweight is created; subsequent requests for the same shared state reuse the existing flyweight.

---

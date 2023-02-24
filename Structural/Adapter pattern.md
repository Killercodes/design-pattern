Adapter pattern
===
The Adapter pattern is a design pattern that allows the interface of an existing class to be adapted to another interface without changing the original code. It is used when we have an object that has an interface that does not match the one that our client code expects, and we want to use that object in our client code without modifying it. The Adapter pattern solves this problem by creating an adapter class that implements the interface that our client code expects, and then delegating the calls to the original object.

In this pattern, there are typically three components: the Target interface, the Adaptee class, and the Adapter class. The Target interface defines the interface that our client code expects to use, and the Adaptee class is the class that has an incompatible interface. The Adapter class is used to adapt the interface of the Adaptee class to the Target interface.

There are two types of Adapter patterns: the class adapter pattern and the object adapter pattern. The class adapter pattern uses inheritance to adapt the interface of the Adaptee class to the Target interface. The object adapter pattern uses composition to adapt the interface of the Adaptee class to the Target interface.

The benefits of using the Adapter pattern are:
- It allows us to reuse existing code that cannot be directly used in our client code.
- It helps to keep our code modular and maintainable by separating the interface adaptation logic from the client code.
- It provides a way to integrate legacy systems with modern systems.

Some common use cases for the Adapter pattern are:
- When we have an existing class that provides some functionality, but its interface is not compatible with the interface expected by our client code.
- When we want to use a third-party library in our code, but the library's interface is not compatible with the interface expected by our code.
- When we want to decouple our client code from the implementation details of an external system.

---
## c#
The Adapter pattern is a structural pattern that allows objects with incompatible interfaces to work together. It involves creating a wrapper (the adapter) that translates the interface of one object to the interface of another, so that they can interact seamlessly. 

Here's an example of how the Adapter pattern can be implemented in C#:
```cs
// The Target interface
public interface ITarget
{
    void Request();
}

// The Adaptee class with an incompatible interface
public class Adaptee
{
    public void SpecificRequest()
    {
        Console.WriteLine("Adaptee's SpecificRequest method called.");
    }
}

// The Adapter class that adapts the Adaptee interface to the Target interface
public class Adapter : ITarget
{
    private readonly Adaptee _adaptee;

    public Adapter(Adaptee adaptee)
    {
        _adaptee = adaptee;
    }

    public void Request()
    {
        _adaptee.SpecificRequest();
    }
}

// The client code that uses the Target interface to communicate with the Adaptee object
public class Client
{
    private readonly ITarget _target;

    public Client(ITarget target)
    {
        _target = target;
    }

    public void MakeRequest()
    {
        _target.Request();
    }
}

```
In this example, we have a Target interface (ITarget) that defines the interface that our client code expects to use. We also have an Adaptee class (Adaptee) that has an incompatible interface. The Adapter class (Adapter) is used to adapt the interface of the Adaptee class to the Target interface. It does this by implementing the Target interface and delegating the request to the Adaptee object. Finally, we have a client class (Client) that uses the Target interface to make requests to the Adaptee object, without needing to know about the details of how the Adapter class is translating the interface.

To use this pattern, we can create an instance of the Adaptee class, pass it to the Adapter constructor, and then pass the Adapter to the client constructor. The client code can then make requests to the Target interface, which will be translated by the Adapter to calls to the Adaptee's interface.

---
## Javascript
Here's an example of how the Adapter pattern can be implemented in JavaScript:
```js
// The Target interface
class ITarget {
  request() {}
}

// The Adaptee class with an incompatible interface
class Adaptee {
  specificRequest() {
    console.log("Adaptee's specificRequest method called.");
  }
}

// The Adapter class that adapts the Adaptee interface to the Target interface
class Adapter extends ITarget {
  constructor(adaptee) {
    super();
    this.adaptee = adaptee;
  }

  request() {
    this.adaptee.specificRequest();
  }
}

// The client code that uses the Target interface to communicate with the Adaptee object
class Client {
  constructor(target) {
    this.target = target;
  }

  makeRequest() {
    this.target.request();
  }
}

// Usage example
const adaptee = new Adaptee();
const adapter = new Adapter(adaptee);
const client = new Client(adapter);

client.makeRequest(); // Outputs: "Adaptee's specificRequest method called."

```
In this example, we have a ITarget interface that defines the interface that our client code expects to use. We also have an Adaptee class that has an incompatible interface. The Adapter class is used to adapt the interface of the Adaptee class to the ITarget interface. It does this by extending the ITarget class and delegating the request() method to the specificRequest() method of the Adaptee object. Finally, we have a client class (Client) that uses the ITarget interface to make requests to the Adaptee object, without needing to know about the details of how the Adapter class is translating the interface.

To use this pattern, we can create an instance of the Adaptee class, pass it to the Adapter constructor, and then pass the Adapter to the Client constructor. The client code can then make requests to the ITarget interface, which will be translated by the Adapter to calls to the Adaptee's interface.


---
## Python
here's an example of how the Adapter pattern can be implemented in Python:

```py
# The Target interface
class ITarget:
    def request(self):
        pass

# The Adaptee class with an incompatible interface
class Adaptee:
    def specific_request(self):
        print("Adaptee's specificRequest method called.")

# The Adapter class that adapts the Adaptee interface to the Target interface
class Adapter(ITarget):
    def __init__(self, adaptee):
        self.adaptee = adaptee

    def request(self):
        self.adaptee.specific_request()

# The client code that uses the Target interface to communicate with the Adaptee object
class Client:
    def __init__(self, target):
        self.target = target

    def make_request(self):
        self.target.request()

# Usage example
adaptee = Adaptee()
adapter = Adapter(adaptee)
client = Client(adapter)

client.make_request() # Outputs: "Adaptee's specificRequest method called."

```
In this example, we have a ITarget interface that defines the interface that our client code expects to use. We also have an Adaptee class that has an incompatible interface. The Adapter class is used to adapt the interface of the Adaptee class to the ITarget interface. It does this by implementing the ITarget interface and delegating the request() method to the specific_request() method of the Adaptee object. Finally, we have a client class (Client) that uses the ITarget interface to make requests to the Adaptee object, without needing to know about the details of how the Adapter class is translating the interface.

To use this pattern, we can create an instance of the Adaptee class, pass it to the Adapter constructor, and then pass the Adapter to the Client constructor. The client code can then make requests to the ITarget interface, which will be translated by the Adapter to calls to the Adaptee's interface.

---

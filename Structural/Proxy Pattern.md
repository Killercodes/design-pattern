Proxy Pattern
===
The Proxy pattern is a structural design pattern that provides a surrogate or placeholder for another object to control access to it. The Proxy acts as an intermediary between the client and the real object, allowing the client to interact with the object indirectly through the proxy.

The Proxy pattern can be used for a variety of purposes, including:

1. Remote Proxy: The Proxy pattern can be used to represent an object that is located on a remote server. The Proxy acts as a local representative for the remote object, allowing the client to interact with it as if it were local.

2. Virtual Proxy: The Proxy pattern can be used to represent an object that is expensive to create or access. The Proxy creates the object only when it is needed and caches it for future use.

3. Protection Proxy: The Proxy pattern can be used to control access to an object. The Proxy checks the access permissions before allowing the client to access the object.

4. Smart Proxy: The Proxy pattern can be used to add additional functionality to an object. The Proxy intercepts the client's requests and adds its own behavior before passing the request to the real object.

The Proxy pattern consists of the following components:

1. Subject: This is the interface that the Proxy and the RealSubject implement. It defines the common interface that clients use to interact with the objects.

2. RealSubject: This is the real object that the client wants to interact with.

3. Proxy: This is the surrogate or placeholder for the RealSubject. It implements the same interface as the RealSubject and maintains a reference to the RealSubject. The Proxy intercepts the client's requests and either passes them directly to the RealSubject or performs additional tasks before passing the requests on.

4. Client: This is the object that uses the Proxy to interact with the RealSubject.

The Proxy pattern provides a way to control access to objects and add additional functionality to them without changing their interface or implementation. This makes it a powerful tool for managing complex systems and improving performance.

---

## c#
Here's a simple example of the Proxy pattern in C#
```cs
using System;

// The 'Subject' abstract class
abstract class Subject
{
    public abstract void Request();
}

// The 'RealSubject' class
class RealSubject : Subject
{
    public override void Request()
    {
        Console.WriteLine("RealSubject: Handling request.");
    }
}

// The 'Proxy' class
class Proxy : Subject
{
    private RealSubject _realSubject;

    public override void Request()
    {
        // Create the 'RealSubject' on first use
        if (_realSubject == null)
        {
            Console.WriteLine("Proxy: Creating 'RealSubject'.");
            _realSubject = new RealSubject();
        }

        _realSubject.Request();
    }
}

// The 'Client' class
class Client
{
    public void DoSomeWork()
    {
        Console.WriteLine("Client: Making a request to Proxy.");
        Proxy proxy = new Proxy();
        proxy.Request();
    }
}

// The 'Program' class
class Program
{
    static void Main(string[] args)
    {
        Client client = new Client();
        client.DoSomeWork();
    }
}

```
In this example, we have three classes: Subject, RealSubject, and Proxy. The Subject class is an abstract class that defines the interface for both RealSubject and Proxy classes. The RealSubject class implements the actual functionality and the Proxy class acts as a surrogate for the RealSubject class, controlling access to it.

In the Proxy class, we have a private field _realSubject that is lazily initialized on the first call to Request(). If the _realSubject is not null, the request is delegated to the _realSubject. Otherwise, the RealSubject object is created and the request is handled.

In the Client class, we instantiate a Proxy object and call its Request() method. Finally, in the Main method of the Program class, we create an instance of the Client class and call its DoSomeWork() method.

When we run this program, we see the following output:

```
Client: Making a request to Proxy.
Proxy: Creating 'RealSubject'.
RealSubject: Handling request.
```
This demonstrates how the Proxy pattern allows us to add a level of indirection to control access to an object.


---
## Javascript
 here's an example of the Proxy pattern implemented in JavaScript with Subject, RealSubject, Proxy, and Client:
 
```js
// Subject interface
class Subject {
  request() {}
}

// RealSubject class
class RealSubject extends Subject {
  request() {
    console.log("RealSubject: Handling request.");
  }
}

// Proxy class
class Proxy extends Subject {
  constructor(realSubject) {
    super();
    this.realSubject = realSubject;
  }

  request() {
    if (this.checkAccess()) {
      this.realSubject.request();
      this.logAccess();
    }
  }

  checkAccess() {
    console.log("Proxy: Checking access prior to firing a real request.");
    return true;
  }

  logAccess() {
    console.log("Proxy: Logging the time of request.");
  }
}

// Client code
function clientCode(subject) {
  subject.request();
}

// Usage example
console.log("Client: Executing the client code with a real subject:");
const realSubject = new RealSubject();
clientCode(realSubject);

console.log("");

console.log("Client: Executing the same client code with a proxy:");
const proxy = new Proxy(realSubject);
clientCode(proxy);

```
In this example, Subject is an interface that defines the common methods for both RealSubject and Proxy classes. RealSubject is the class that does the actual work, while Proxy acts as an intermediary between the client and RealSubject. The Client code interacts with either RealSubject or Proxy objects depending on the scenario. When a request is made to Proxy, it checks access and logs the request before forwarding the request to the RealSubject.


---
## Python
here's an example of the Proxy pattern implemented in Python with a Subject, RealSubject, Proxy, and client:

```py
from abc import ABC, abstractmethod


class Subject(ABC):
    @abstractmethod
    def request(self) -> None:
        pass


class RealSubject(Subject):
    def request(self) -> None:
        print("RealSubject: Handling request.")


class Proxy(Subject):
    def __init__(self, real_subject: RealSubject) -> None:
        self._real_subject = real_subject

    def request(self) -> None:
        if self.check_access():
            self._real_subject.request()
            self.log_access()

    def check_access(self) -> bool:
        print("Proxy: Checking access prior to firing a real request.")
        return True

    def log_access(self) -> None:
        print("Proxy: Logging the time of request.")


def client_code(subject: Subject) -> None:
    subject.request()


if __name__ == "__main__":
    real_subject = RealSubject()
    proxy = Proxy(real_subject)
    client_code(proxy)

```
In this example, Subject is an abstract class defining the interface for the RealSubject and Proxy classes. RealSubject is the actual object that performs the work, and Proxy acts as a stand-in for the RealSubject to control access to it.

The client_code function takes a Subject object as an argument and invokes the request method on it. In this case, it passes a Proxy object to the function.

When the Proxy object's request method is called, it first checks access permissions by calling its check_access method. If access is granted, it then calls the RealSubject object's request method and logs the time of the request by calling its log_access method. If access is denied, it does not call the RealSubject object's request method.


---

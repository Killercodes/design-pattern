Chain Of Responsibility
===
The Chain of Responsibility pattern is a behavioral design pattern that allows an object to pass a request along a chain of handlers until one or more of the handlers has processed the request. This pattern is useful when you have a set of objects that can handle a particular request, but you don't know which object should handle the request at runtime.

The basic idea behind this pattern is that a chain of handler objects is created to handle requests. When a request comes in, it is passed down the chain of handlers until one of the handlers can process it. Each handler decides whether to process the request or pass it on to the next handler in the chain.

The Chain of Responsibility pattern consists of three main components:
1. Handler: Defines an interface for handling requests and optionally implements the successor link to the next handler in the chain.
2. ConcreteHandler: Implements the Handler interface and handles requests it is responsible for. It can also forward requests to its successor.
3. Client: Sends requests to the first handler in the chain.

The advantages of using the Chain of Responsibility pattern include:
1. Decouples the sender of a request from its receiver by allowing more than one object to handle the request.
2. Simplifies object management by allowing objects to be added or removed from the chain dynamically.
3. Provides a flexible way of passing requests along a chain of objects until one object can handle it.
4. Promotes loose coupling between objects, making the code more maintainable and easier to modify.

Some common examples of the Chain of Responsibility pattern include:
1. Event handlers in a GUI framework.
2. Middleware in web frameworks.
3. Error handling in software systems.
4.Logging and auditing frameworks.

---

## c# 

### Example 1
here's a simple example of the Chain of Responsibility pattern in C#:
```cs
using System;

namespace ChainOfResponsibilityPattern
{
    class Program
    {
        static void Main(string[] args)
        {
            Handler concreteHandler1 = new ConcreteHandler1();
            Handler concreteHandler2 = new ConcreteHandler2();
            Handler concreteHandler3 = new ConcreteHandler3();

            concreteHandler1.SetSuccessor(concreteHandler2);
            concreteHandler2.SetSuccessor(concreteHandler3);

            concreteHandler1.HandleRequest(1);
            concreteHandler1.HandleRequest(2);
            concreteHandler1.HandleRequest(3);
        }
    }

    abstract class Handler
    {
        protected Handler successor;

        public void SetSuccessor(Handler successor)
        {
            this.successor = successor;
        }

        public abstract void HandleRequest(int request);
    }

    class ConcreteHandler1 : Handler
    {
        public override void HandleRequest(int request)
        {
            if (request == 1)
            {
                Console.WriteLine($"Request {request} handled by {nameof(ConcreteHandler1)}");
            }
            else if (successor != null)
            {
                successor.HandleRequest(request);
            }
        }
    }

    class ConcreteHandler2 : Handler
    {
        public override void HandleRequest(int request)
        {
            if (request == 2)
            {
                Console.WriteLine($"Request {request} handled by {nameof(ConcreteHandler2)}");
            }
            else if (successor != null)
            {
                successor.HandleRequest(request);
            }
        }
    }

    class ConcreteHandler3 : Handler
    {
        public override void HandleRequest(int request)
        {
            if (request == 3)
            {
                Console.WriteLine($"Request {request} handled by {nameof(ConcreteHandler3)}");
            }
            else if (successor != null)
            {
                successor.HandleRequest(request);
            }
        }
    }
}

```
This example demonstrates the Chain of Responsibility pattern by creating three concrete handlers that handle requests with certain criteria. Each concrete handler is set up to have a successor, which it will delegate to if it can't handle a request on its own. The client (in this case, the Main method) creates the chain of handlers by setting the successor of the first handler to be the second, and the successor of the second to be the third.

When a request is made to the first handler, it either handles the request or delegates it to the next handler in the chain, and so on until a handler is able to handle the request or the end of the chain is reached.

### Example 2
The Chain of Responsibility design pattern avoids coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. 
This pattern chains the receiving objects and passes the request along the chain until an object handles it

```cs
/// <summary>
/// The 'Handler' abstract class
/// </summary>
public abstract class Handler
{
    protected Handler successor;
    public void SetSuccessor(Handler successor)
    {
        this.successor = successor;
    }
    public abstract void HandleRequest(int request);
}

/// <summary>
/// The 'ConcreteHandler1' class
/// </summary>
public class ConcreteHandler1 : Handler
{
    public override void HandleRequest(int request)
    {
        if (request >= 0 && request < 10)
        {
            Console.WriteLine("{0} handled request {1}",
                this.GetType().Name, request);
        }
        else if (successor != null)
        {
            successor.HandleRequest(request);
        }
    }
}

/// <summary>
/// The 'ConcreteHandler2' class
/// </summary>
public class ConcreteHandler2 : Handler
{
    public override void HandleRequest(int request)
    {
        if (request >= 10 && request < 20)
        {
            Console.WriteLine("{0} handled request {1}",
                this.GetType().Name, request);
        }
        else if (successor != null)
        {
            successor.HandleRequest(request);
        }
    }
}

/// <summary>
/// The 'ConcreteHandler3' class
/// </summary>
public class ConcreteHandler3 : Handler
{
    public override void HandleRequest(int request)
    {
        if (request >= 20 && request < 30)
        {
            Console.WriteLine("{0} handled request {1}",
                this.GetType().Name, request);
        }
        else if (successor != null)
        {
            successor.HandleRequest(request);
        }
    }
}
```

The Main Method 
```cs
// Setup Chain of Responsibility
Handler h1 = new ConcreteHandler1();
Handler h2 = new ConcreteHandler2();
Handler h3 = new ConcreteHandler3();
h1.SetSuccessor(h2);
h2.SetSuccessor(h3);
// Generate and process request
int[] requests = { 2, 5, 14, 22, 18, 3, 27, 20 };
foreach (int request in requests)
{
    h1.HandleRequest(request);
}

```


---
## Javascript
here's an example of the Chain of Responsibility pattern in JavaScript 

```js
class Handler {
  constructor() {
    this.nextHandler = null;
  }

  setNextHandler(nextHandler) {
    this.nextHandler = nextHandler;
  }

  handle(request) {
    if (this.nextHandler) {
      return this.nextHandler.handle(request);
    }
    return null;
  }
}

class ConcreteHandler1 extends Handler {
  handle(request) {
    if (request === 'type1') {
      return 'Handled by ConcreteHandler1';
    }
    return super.handle(request);
  }
}

class ConcreteHandler2 extends Handler {
  handle(request) {
    if (request === 'type2') {
      return 'Handled by ConcreteHandler2';
    }
    return super.handle(request);
  }
}

class ConcreteHandler3 extends Handler {
  handle(request) {
    if (request === 'type3') {
      return 'Handled by ConcreteHandler3';
    }
    return super.handle(request);
  }
}

// Client
const handler1 = new ConcreteHandler1();
const handler2 = new ConcreteHandler2();
const handler3 = new ConcreteHandler3();

handler1.setNextHandler(handler2);
handler2.setNextHandler(handler3);

console.log(handler1.handle('type2')); // Output: Handled by ConcreteHandler2
console.log(handler1.handle('type3')); // Output: Handled by ConcreteHandler3
console.log(handler1.handle('type4')); // Output: null

```
In this example, Handler is the abstract class for all the concrete handlers. It defines a handle method that is responsible for handling the request and forwarding it to the next handler if it cannot handle the request. Each concrete handler (ConcreteHandler1, ConcreteHandler2, ConcreteHandler3) implements the handle method to handle a specific type of request.

The setNextHandler method is used to set the next handler in the chain. When a request is received, the first handler (handler1) is called to handle it. If it cannot handle the request, it passes the request to the next handler (handler2). If the next handler cannot handle the request, it passes it to the third handler (handler3), and so on. If none of the handlers can handle the request, the handle method returns null.


---
## Python

```py

```


---

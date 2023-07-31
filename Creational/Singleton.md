Singleton  Pattern
===
The Singleton pattern is a design pattern that restricts the instantiation of a class to one object and provides a global point of access to that instance. This means that there can be only one instance of the Singleton class, and it can be accessed from anywhere within the program.

Here are some characteristics and use cases of the Singleton pattern:

## Characteristics:
- There is only one instance of the Singleton class.
- The Singleton class controls the instantiation of the singleton object.
- The Singleton class provides a global point of access to the singleton object.
- The Singleton class is often implemented as a static class or a class with a private constructor.
- The Singleton object is usually created lazily (i.e., when it is first requested) or eagerly (i.e., when the program starts).

## Use cases:
- When we need to ensure that only one instance of a class is created, and that it is accessible throughout the program.
- When we need to control access to a shared resource, such as a database connection, a file system, or a thread pool.
- When we need to provide a single point of control for a complex system, such as a logging system or a configuration manager.
- When we need to limit the number of instances of a class that can be created due to resource constraints, such as a limited amount of memory or processing power.

It's important to note that the Singleton pattern has some drawbacks, such as making it difficult to test classes that depend on the singleton object and making it harder to achieve loose coupling between classes. Additionally, it can lead to issues with concurrency and thread safety if not implemented correctly. However, when used appropriately, the Singleton pattern can be a powerful tool for managing shared resources and providing a centralized point of control in a program.

> The singleton pattern is what you use when you want to ensure that only one instance of an object is ever created. In classical object-oriented programming languages, the concepts behind creating a singleton was a bit tricky to wrap your mind around because it involved a class that has both static and non-static properties and methods.

> The Singleton design pattern ensures a class has only one instance and provide a global point of access to it.

- Defines an Instance operation that lets clients access its unique instance. Instance is a class operation.
- Responsible for creating and maintaining its own unique instance.

## Structural code in C#
This structural code demonstrates the Singleton pattern which assures only a single instance (the singleton) of the class can be created.

```cs
using System;

namespace Creational.Singleton.Structure
{
    /// <summary>
    /// Singleton Design Pattern
    /// </summary>

    public class Program
    {
        public static void Main(string[] args)
        {
            // Constructor is protected -- cannot use new

            Singleton s1 = Singleton.Instance();
            Singleton s2 = Singleton.Instance();

            // Test for same instance

            if (s1 == s2)
            {
                Console.WriteLine("Objects are the same instance");
            }

            // Wait for user

            Console.ReadKey();
        }
    }

    /// <summary>
    /// The 'Singleton' class
    /// </summary>

    public class Singleton
    {
        static Singleton instance;

        // Constructor is 'protected'

        protected Singleton()
        {
        }

        public static Singleton Instance()
        {
            // Uses lazy initialization.
            // Note: this is not thread safe.
            if (instance == null)
            {
                instance = new Singleton();
            }

            return instance;
        }
    }
}
```

## .NET Optimized code in C#
The .NET optimized code demonstrates the same code as above but uses more modern, built-in .NET features.

Here is an elegant .NET specific solution. The Singleton pattern simply uses a private constructor and a static readonly instance variable that is lazily initialized. Thread safety is guaranteed by the compiler.

```cs
using System;
using System.Collections.Generic;
using static System.Console;

namespace Creational.Singleton.NetOptimized
{
    public class Program
    {
        /// <summary>
        /// Singleton Design Pattern
        /// </summary>

        public static void Main()
        {
            var b1 = LoadBalancer.GetLoadBalancer();
            var b2 = LoadBalancer.GetLoadBalancer();
            var b3 = LoadBalancer.GetLoadBalancer();
            var b4 = LoadBalancer.GetLoadBalancer();

            // Confirm these are the same instance

            if (b1 == b2 && b2 == b3 && b3 == b4)
            {
                WriteLine("Same instance\n");
            }

            // Next, load balance 15 requests for a server

            var balancer = LoadBalancer.GetLoadBalancer();
            for (int i = 0; i < 15; i++)
            {
                string serverName = balancer.NextServer.Name;
                WriteLine("Dispatch request to: " + serverName);
            }

            // Wait for user

            ReadKey();
        }
    }

    /// <summary>
    /// The 'Singleton' class
    /// </summary>

    public sealed class LoadBalancer
    {
        // Static members are 'eagerly initialized', that is, 
        // immediately when class is loaded for the first time.
        // .NET guarantees thread safety for static initialization

        private static readonly LoadBalancer instance = new LoadBalancer();

        private readonly List<Server> servers;
        private readonly Random random = new Random();

        // Note: constructor is 'private'

        private LoadBalancer()
        {
            // Load list of available servers

            servers = new List<Server>
                {
                  new Server{ Name = "ServerI", IP = "120.14.220.18" },
                  new Server{ Name = "ServerII", IP = "120.14.220.19" },
                  new Server{ Name = "ServerIII", IP = "120.14.220.20" },
                  new Server{ Name = "ServerIV", IP = "120.14.220.21" },
                  new Server{ Name = "ServerV", IP = "120.14.220.22" },
                };
        }

        public static LoadBalancer GetLoadBalancer()
        {
            return instance;
        }

        // Simple, but effective load balancer

        public Server NextServer
        {
            get
            {
                int r = random.Next(servers.Count);
                return servers[r];
            }
        }
    }

    /// <summary>
    /// Represents a server machine
    /// </summary>

    public class Server
    {
        public string Name { get; set; }
        public string IP { get; set; }
    }
}
```

---

## c#
 Here is an example of a thread-safe singleton pattern in C#:
```cs
public sealed class Singleton
{
    private static Singleton instance = null;
    private static readonly object padlock = new object();

    private Singleton()
    {
    }

    public static Singleton Instance
    {
        get
        {
            lock (padlock)
            {
                if (instance == null)
                {
                    instance = new Singleton();
                }
                return instance;
            }
        }
    }
}

```
In this implementation, we use the lock statement to ensure that only one thread can access the Instance property at a time. If two threads try to access the Instance property simultaneously, the second thread will be blocked until the first thread completes its work.

We also use the sealed keyword to prevent anyone from inheriting from the Singleton class and creating additional instances.

This implementation ensures that only one instance of the Singleton class will be created, even in a multi-threaded environment, and that it will be thread-safe.

---
## Javascript

Here is an example of a thread-safe singleton pattern in JavaScript:
```js
class Singleton {
  constructor() {
    if (typeof Singleton.instance === 'object') {
      return Singleton.instance;
    }

    Singleton.instance = this;
    return this;
  }
}

```
In this implementation, we use a static property instance to store the singleton instance, and the constructor function checks whether an instance has already been created. If an instance already exists, it returns the existing instance, otherwise it creates a new instance and sets Singleton.instance to it.

This implementation ensures that only one instance of the Singleton class will be created, even in a multi-threaded environment, and that it will be thread-safe. However, it's important to note that JavaScript is a single-threaded language, so the thread safety in this case refers to preventing race conditions in the case of asynchronous code or multiple callbacks.

---
## Python
Here is an example of a thread-safe singleton pattern in Python 
```py
from threading import Lock

class Singleton:
    _instance = None
    _lock = Lock()

    def __new__(cls):
        with cls._lock:
            if not cls._instance:
                cls._instance = super().__new__(cls)
        return cls._instance

```
In this implementation, we use a class-level variable `_instance` to store the singleton instance, and a Lock object to ensure that only one thread can access the `__new__` method at a time. 
If two threads try to access the `__new__` method simultaneously, the second thread will be blocked until the first thread completes its work.

The `__new__` method creates a new instance of the class if one does not already exist, otherwise it returns the existing instance.

This implementation ensures that only one instance of the Singleton class will be created, even in a multi-threaded environment, and that it will be thread-safe.

---


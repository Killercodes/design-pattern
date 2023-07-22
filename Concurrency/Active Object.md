**Active Object Design Pattern**
===

> The active object design pattern decouples method execution from method invocation for objects that each reside in their own thread of control.[1] The goal is to introduce concurrency, by using asynchronous method invocation and a scheduler for handling requests


The Active Object design pattern decouples method execution from method invocation for objects that each reside in their own thread of control. The goal is to introduce concurrency, by using asynchronous method invocation and a scheduler for handling requests1.

Here’s an example in C# that shows how the Active Object pattern can be used to implement a simple producer-consumer scenario:

```cs
using System;
using System.Collections.Concurrent;
using System.Threading;

namespace ActiveObject
{
    public interface IActiveObject
    {
        void Produce(string item);
        string Consume();
    }

    public class ActiveObject : IActiveObject
    {
        private readonly BlockingCollection<string> _queue = new BlockingCollection<string>();
        private readonly Thread _workerThread;

        public ActiveObject()
        {
            _workerThread = new Thread(WorkerThread) { IsBackground = true };
            _workerThread.Start();
        }

        public void Produce(string item)
        {
            _queue.Add(item);
        }

        public string Consume()
        {
            return _queue.Take();
        }

        private void WorkerThread()
        {
            while (true)
            {
                var item = _queue.Take();
                Console.WriteLine("Consumed: " + item);
                Thread.Sleep(1000);
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            var activeObject = new ActiveObject();

            for (int i = 0; i < 10; i++)
            {
                activeObject.Produce("Item " + i);
                Thread.Sleep(500);
            }

            Console.ReadLine();
        }
    }
}

```

In this example, the ActiveObject class implements the `IActiveObject` interface which defines methods for producing and consuming items. 
The `ActiveObject` class uses a `BlockingCollection` to store the items and a worker thread to consume them. The `Produce` method adds an item to the collection, while the `Consume` method takes an item from the collection. The worker thread runs an infinite loop that takes an item from the collection, processes it, and then sleeps for a while.

---

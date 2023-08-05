Producer Consumer
===

The Producer-Consumer design pattern is used to separate the concerns of producing and consuming data. It involves two types of components: a producer that generates data and a consumer that processes it. The producer and consumer communicate through a shared buffer, which can be implemented using various data structures such as a queue or a stack.

## Intent
Producer Consumer Design pattern is a classic concurrency pattern which reduces coupling between Producer and Consumer by separating Identification of work with Execution of Work.

## Explanation
### Real-world example
Consider a manufacturing process of item, the producer will need to pause the production when manufacturing pipeline is full and the consumer will need to pause the consumption of item when the manufacturing pipeline is empty. We can separate the process of production and consumption which work together and pause at separate times.

> It provides a way to share data between multiple loops running at different rates.

Dijkstra wrote about the case: "We consider two processes, which are called the 'producer' and the 'consumer' respectively. The producer is a cyclic process that produces a certain portion of information, that has to be processed by the consumer. The consumer is also a cyclic process that needs to process the next portion of information, as has been produced by the producer We assume the two processes to be connected for this purpose via a buffer with unbounded capacity."

## Applicability
Use the Producer Consumer idiom when
- Decouple system by separate work in two process produce and consume.
- Addresses the issue of different timing require to produce work or consuming work

## Example

```cs
using System;
using System.Collections.Concurrent;
using System.Threading.Tasks;

namespace ProducerConsumerPattern
{
    public class Producer
    {
        private readonly BlockingCollection<int> _buffer;

        public Producer(BlockingCollection<int> buffer)
        {
            _buffer = buffer;
        }

        public async Task ProduceAsync()
        {
            for (int i = 0; i < 10; i++)
            {
                await Task.Delay(1000);
                _buffer.Add(i);
                Console.WriteLine($"Produced: {i}");
            }
            _buffer.CompleteAdding();
        }
    }

    public class Consumer
    {
        private readonly BlockingCollection<int> _buffer;

        public Consumer(BlockingCollection<int> buffer)
        {
            _buffer = buffer;
        }

        public async Task ConsumeAsync()
        {
            foreach (var item in _buffer.GetConsumingEnumerable())
            {
                await Task.Delay(1000);
                Console.WriteLine($"Consumed: {item}");
            }
        }
    }

    class Program
    {
        static async Task Main(string[] args)
        {
            var buffer = new BlockingCollection<int>(boundedCapacity: 2);
            var producer = new Producer(buffer);
            var consumer = new Consumer(buffer);

            var produceTask = producer.ProduceAsync();
            var consumeTask = consumer.ConsumeAsync();

            await Task.WhenAll(produceTask, consumeTask);
        }
    }
}

```
In this example, the Producer and Consumer classes communicate through a shared BlockingCollection buffer. The Producer generates data and adds it to the buffer using the Add method. The Consumer processes data from the buffer using the GetConsumingEnumerable method. The ProduceAsync and ConsumeAsync methods demonstrate how the producer and consumer can operate concurrently using asynchronous tasks.


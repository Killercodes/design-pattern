Master-Worker
===

The Master-Worker design pattern, also known as the Master-Slave or Map-Reduce pattern, is used for parallel processing. It follows a simple approach that allows applications to perform simultaneous processing across multiple machines or processes via a Master and multiple Workers.

## Also known as
Master-slave or Map-reduce

## Intent
Used for centralised parallel processing.

## Applicability
This pattern can be used when data can be divided into multiple parts, all of which need to go through the same computation to give a result, which need to be aggregated to get the final result.

## Explanation
In this pattern, parallel processing is performed using a system consisting of a master and some number of workers, where a master divides the work among the workers, gets the result back from them and assimilates all the results to give final result. The only communication is between the master and the worker - none of the workers communicate among one another and the user only communicates with the master to get the required job done. The master has to maintain a record of how the divided data has been distributed, how many workers have finished their work and returned a result, and the results themselves to be able to aggregate the data correctly.


```cs
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

namespace MasterWorkerPattern
{
    public interface IWorker
    {
        int DoWork(int input);
    }

    public class Worker : IWorker
    {
        public int DoWork(int input)
        {
            // Perform some work here...
            return input * 2;
        }
    }

    public class Master
    {
        private readonly List<IWorker> _workers = new List<IWorker>();

        public void AddWorker(IWorker worker)
        {
            _workers.Add(worker);
        }

        public async Task<int[]> DoWorkAsync(int[] inputs)
        {
            var tasks = new List<Task<int>>();
            foreach (var input in inputs)
            {
                var worker = _workers[tasks.Count % _workers.Count];
                tasks.Add(Task.Run(() => worker.DoWork(input)));
            }
            return await Task.WhenAll(tasks);
        }
    }

    class Program
    {
        static async Task Main(string[] args)
        {
            var master = new Master();
            master.AddWorker(new Worker());
            master.AddWorker(new Worker());

            var inputs = new[] { 1, 2, 3, 4 };
            var results = await master.DoWorkAsync(inputs);

            Console.WriteLine(string.Join(", ", results));
        }
    }
}

```

In this example, the Master class maintains a list of IWorker objects and uses them to perform work concurrently using the Task.Run method. The DoWorkAsync method takes an array of inputs and returns an array of results. The Worker class implements the IWorker interface and defines its own behavior in the DoWork method.
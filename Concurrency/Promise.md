Promise
===

## Also known as
CompletableFuture

## Intent
A Promise represents a proxy for a value not necessarily known when the promise is created. It allows you to associate dependent promises to an asynchronous action's eventual success value or failure reason. Promises are a way to write async code that still appears as though it is executing in a synchronous way.

## Explanation
The Promise object is used for asynchronous computations. A Promise represents an operation that hasn't completed yet, but is expected in the future.

Promises provide a few advantages over callback objects:
- Functional composition and error handling.
- Prevents callback hell and provides callback aggregation.

### Real world example
We are developing a software solution that downloads files and calculates the number of lines and character frequencies in those files. Promise is an ideal solution to make the code concise and easy to understand.

> Promise is a placeholder for an asynchronous operation that is ongoing.

Wikipedia says:
In computer science, future, promise, delay, and deferred refer to constructs used for synchronizing program execution in some concurrent programming languages. They describe an object that acts as a proxy for a result that is initially unknown, usually because the computation of its value is not yet complete.

## Example

```cs
using System;
using System.Threading.Tasks;

namespace PromisePattern
{
    public static class AsyncMethods
    {
        public static async Task<int> GetNumberAsync()
        {
            await Task.Delay(1000);
            return 42;
        }
    }

    class Program
    {
        static async Task Main(string[] args)
        {
            var task = AsyncMethods.GetNumberAsync();
            Console.WriteLine("Waiting for result...");
            var result = await task;
            Console.WriteLine($"Result: {result}");
        }
    }
}

```

In this example, the GetNumberAsync method returns a Task<int> that represents an asynchronous operation that will eventually return an integer value. The await keyword is used to asynchronously wait for the task to complete and retrieve its result. This allows the program to perform other work while waiting for the asynchronous operation to complete.

---
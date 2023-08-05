Fan-Out/Fan-In
===

> Main function which internally calls multiple functions in parallel and aggregates the response to pass into another function to return the result.


## Intent
The pattern is used when a source system needs to run one or more long-running processes that will fetch some data. The source will not block itself waiting for the reply.
The pattern will run the same function in multiple services or machines to fetch the data. This is equivalent to invoking the function multiple times on different chunks of data.

## Explanation
The FanOut/FanIn service will take in a list of requests and a consumer. Each request might complete at a different time. FanOut/FanIn service will accept the input params and returns the initial system an ID to acknowledge that the pattern service has received the requests. Now the caller will not wait or expect the result in the same connection.

Meanwhile, the pattern service will invoke the requests that have come. The requests might complete at different time. These requests will be processed in different instances of the same function in different machines or services. As the requests get completed, a callback service everytime is called that transforms the result into a common single object format that gets pushed to a consumer. The caller will be at the other end of the consumer receiving the result.

## Example

```cs
void Main()
{
	var fifo = new FanOutFanIn();
	fifo.AddTask(new Task<int>(()=>LongRunningTask(3)));
	fifo.AddTask(new Task<int>(()=>LongRunningTask(4)));
	fifo.AddTask(new Task<int>(()=>LongRunningTask(5)));
	
	fifo.Run().Dump("output");
}


Random r = new Random();
private  int LongRunningTask(int value)
{
    // Simulate a long running task
	var rr = r.Next(10000);
	$"Workdload = {rr}".Dump();
    Task.Delay(rr).Wait();
    return value +1;
}

public class FanOutFanIn
{
	List<Task<int>> TaskList;
	
	public FanOutFanIn()
	{
		TaskList = new List<Task<int>>();
	}
	
	public void AddTask(Task<int> newTask)
	{
		TaskList.Add(newTask);
	}
	
	async public Task<int> Run()
	{
		//run all the tasks
		foreach(var t in TaskList)
			t.Start();
		
		// Wait for all tasks to complete
    	var results = await Task.WhenAll(TaskList);
		
		// Aggregate the results
    	var sum = results.Sum();
		
		return sum;
	}
}

```

---
**Balking Design Pattern**
===

> The balking pattern is a software design pattern that only executes an action on an object when the object is in a particular state. 
For example, if an object reads ZIP files and a calling method invokes a get method on the object when the ZIP file is not open, the object would "balk" at the request.

Here’s an example in C# that shows how the Balking pattern can be used to implement a simple data loader:
```cs
public class DataLoader
{
    private bool _loaded = false;

    public void LoadData()
    {
        if (_loaded)
        {
            Console.WriteLine("Data already loaded");
            return;
        }

        Console.WriteLine("Loading data...");
        Thread.Sleep(1000);
        _loaded = true;
    }
}

class Program
{
    static void Main(string[] args)
    {
        var dataLoader = new DataLoader();

        dataLoader.LoadData();
        dataLoader.LoadData();

        Console.ReadLine();
    }
}

```

---

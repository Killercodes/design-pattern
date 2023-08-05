**Abstract Document** `Structural`
===

An object-oriented structural design pattern for organizing objects in loosely typed key-value stores and exposing the data using typed views. The purpose of the pattern is to achieve a high degree of flexibility between components in a strongly typed language where new properties can be added to the object-tree on the fly, without losing the support of type-safety. The pattern makes use of traits to separate different properties of a class into different interfaces.

> Abstract Document pattern allows attaching properties to objects without them knowing about it.

The Abstract Document pattern enables handling additional, non-static properties. This pattern uses concept of traits to enable type safety and separate properties of different classes into set of interfaces.

For example, Consider a car that consists of multiple parts. However we don't know if the specific car really has all the parts, or just some of them. Our cars are dynamic and extremely flexible.


## Intent:
Use dynamic properties and achieve flexibility of untyped languages while keeping type-safety.

## Example
Let's first define the base classes Document and AbstractDocument. They basically make the object hold a property map and any amount of child objects

The Abstract Document design pattern is a way to manage complex data structures by treating them as dynamic objects with properties. This pattern is useful when working with data that has a flexible schema, such as JSON or XML documents. Here is an example of the Abstract Document design pattern implemented in C#:
```cs
public interface IDocument
{
    object this[string key] { get; set; }
    T GetProperty<T>(string key);
}

public class Document : IDocument
{
    private readonly Dictionary<string, object> _properties = new Dictionary<string, object>();

    public object this[string key]
    {
        get => _properties[key];
        set => _properties[key] = value;
    }

    public T GetProperty<T>(string key)
    {
        return (T)_properties[key];
    }
}

public class Car : Document
{
    public string Make
    {
        get => GetProperty<string>(nameof(Make));
        set => this[nameof(Make)] = value;
    }

    public string Model
    {
        get => GetProperty<string>(nameof(Model));
        set => this[nameof(Model)] = value;
    }

    public int Year
    {
        get => GetProperty<int>(nameof(Year));
        set => this[nameof(Year)] = value;
    }
}

public static void Main(string[] args)
{
    var car = new Car
    {
        Make = "Toyota",
        Model = "Camry",
        Year = 2020
    };

    Console.WriteLine($"Make: {car.Make}");
    Console.WriteLine($"Model: {car.Model}");
    Console.WriteLine($"Year: {car.Year}");
}

```

In this example, the IDocument interface defines the contract for a document object that can store and retrieve properties using a string key. The Document class provides a concrete implementation of this interface using a dictionary to store the properties. The GetProperty<T> method allows retrieving properties of a specific type.

In this example, we have extended the Document class to create a Car class that represents a specific type of document. The Car class adds properties for the make, model, and year of the car, which are stored in the underlying dictionary of the Document class. This allows us to treat the Car object as a dynamic document with properties, while also providing strongly-typed access to specific properties.


---


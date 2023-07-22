**Marker Design pattern**
===

The Marker interface pattern is a design pattern used with languages that provide run-time type information about objects. It provides a means to associate metadata with a class where the language does not have explicit support for such metadata 1.

A marker interface, also known as a tagging interface, is an empty interface that is used to “mark” classes that implement the interface. This allows methods that interact with instances of the class to test for the existence of the interface and behave accordingly 1.

An example of a marker interface in Java is the Serializable interface. A class implements this interface to indicate that its non-transient data members can be written to an ObjectOutputStream. The ObjectOutputStream private method writeObject0 contains a series of instanceof tests to determine writeability, one of which looks for the Serializable interface. If any of these tests fails, the method throws a NotSerializableException 

In C#, you can use an empty interface to create a marker interface. Here’s an example:
```cs
public interface ISerializable
{
    // empty interface
}

public class MyClass : ISerializable
{
    // MyClass implements the ISerializable marker interface
}
```

In this example, we define an empty interface ISerializable that serves as a marker interface. The MyClass class implements this interface to indicate that it is serializable.

You can then use the is keyword to check if an object is an instance of a class that implements the ISerializable interface.

```cs
MyClass myObject = new MyClass();

if (myObject is ISerializable)
{
    // myObject is serializable
}
```

---

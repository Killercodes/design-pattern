**Servant Design pattern**
===

The servant pattern defines an object used to offer some functionality to a group of classes without defining that functionality in each of them. 
A Servant is a class whose instance (or even just class) provides methods that take care of a desired service, while objects for which (or with whom) the servant does something, are taken as parameters.

>The Servant design pattern is used to provide some behavior to a group of classes. Instead of defining that behavior in each class, or when we cannot factor out this behavior in the common parent class, it is defined once in the Servant.

A Servant is a class whose instance (or even just class) provides methods that take care of a desired service, while objects for which (or with whom) the servant does something are taken as parameters1.

Here’s an example in C# that shows how the Servant pattern can be used to provide a move method for a group of geometric objects:

```cs
interface IMovable
{
    void SetPosition(int x, int y);
    int GetX();
    int GetY();
}

class Circle : IMovable
{
    private int x;
    private int y;
    private int r;

    public Circle(int x, int y, int r)
    {
        this.x = x;
        this.y = y;
        this.r = r;
    }

    public void SetPosition(int x, int y)
    {
        this.x = x;
        this.y = y;
    }

    public int GetX()
    {
        return x;
    }

    public int GetY()
    {
        return y;
    }
}

class Rectangle : IMovable
{
    private int x;
    private int y;
    private int width;
    private int height;

    public Rectangle(int x, int y, int width, int height)
    {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
    }

    public void SetPosition(int x, int y)
    {
        this.x = x;
        this.y = y;
    }

    public int GetX()
    {
        return x;
    }

    public int GetY()
    {
        return y;
    }
}

class MoveServant
{
    public void MoveBy(IMovable movable, int dx, int dy)
    {
        movable.SetPosition(movable.GetX() + dx, movable.GetY() + dy);
    }
}


```
In this example, we have two classes `Circle` and `Rectangle` representing geometric objects. Both classes implement the `Movable` interface which defines methods for setting and getting 
the position of the object. The `MoveServant` class provides a `MoveBy` method that takes a `Movable` object and moves it by a specified amount. 
This way, we can use the same `MoveServant` to move both `Circle` and `Rectangle` objects.



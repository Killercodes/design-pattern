**Acyclic Visitor**
===

Category**Behavioral**

The Acyclic Visitor pattern allows new functions to be added to existing class hierarchies without affecting those hierarchies, and without creating the dependency cycles that are inherent to the GangOfFour VisitorPattern.

> Acyclic Visitor allows functions to be added to existing class hierarchies without modifying the hierarchies.


## Intent
Allow new functions to be added to existing class hierarchies without affecting those hierarchies, and without creating the troublesome dependency cycles that are inherent to the GoF Visitor Pattern.

## Applicability
This pattern can be used:
- When you need to add a new function to an existing hierarchy without the need to alter or affect that hierarchy.
- When there are functions that operate upon a hierarchy, but which do not belong in the hierarchy itself. e.g. the ConfigureForDOS / ConfigureForUnix / ConfigureForX issue.
- When you need to perform very different operations on an object depending upon its type.
- When the visited class hierarchy will be frequently extended with new derivatives of the Element class.
- When the recompilation, relinking, retesting or redistribution of the derivatives of Element is very expensive.


## Example
We have a hierarchy of modem classes. The modems in this hierarchy need to be visited by an external algorithm based on filtering criteria (is it Unix or DOS compatible modem).

```cs
public static class LOGGER
{
	public static void info(string msg)
	{
		Console.WriteLine(msg);
	}
}

//Here's the Modem hierarchy.
public abstract class Modem 
{
  public abstract void accept(ModemVisitor modemVisitor);
}

public class Zoom : Modem 
{

  public override void accept(ModemVisitor modemVisitor) {
    if (modemVisitor is ZoomVisitor) {
      ((ZoomVisitor) modemVisitor).visit(this);
    } 
	else {
      LOGGER.info("Only ZoomVisitor is allowed to visit Zoom modem");
    }
  }
}

public class Hayes : Modem 
{

  public override void accept(ModemVisitor modemVisitor) {
    if (modemVisitor is HayesVisitor) {
      ((HayesVisitor) modemVisitor).visit(this);
    } 
	else {
      LOGGER.info("Only HayesVisitor is allowed to visit Hayes modem");
    }
  }
}

//Next we introduce the ModemVisitor hierarchy.
public interface ModemVisitor 
{}

public interface HayesVisitor : ModemVisitor 
{
  void visit(Hayes hayes);
}

public interface ZoomVisitor : ModemVisitor 
{
  void visit(Zoom zoom);
}

public interface AllModemVisitor : ZoomVisitor, HayesVisitor 
{}

public class ConfigureForDosVisitor : AllModemVisitor 
{
 
  public void visit(Hayes hayes) {
    LOGGER.info(hayes + " used with Dos configurator.");
  }
  

  public void visit(Zoom zoom) {
    LOGGER.info(zoom + " used with Dos configurator.");
  }
}

public class ConfigureForUnixVisitor : ZoomVisitor 
{

  public void visit(Zoom zoom) {
    LOGGER.info(zoom + " used with Unix configurator.");
  }
}

//Finally, here are the visitors in action.
void Main()
{
	var conUnix = new ConfigureForUnixVisitor();
    var conDos = new ConfigureForDosVisitor();
    var zoom = new Zoom();
    var hayes = new Hayes();
    hayes.accept(conDos);
    zoom.accept(conDos);
    hayes.accept(conUnix);
    zoom.accept(conUnix);   
}
```

The visitor also has some disadvantages:

- It's a clumsy pattern: it requires repetitive code in the hierarchy classes to implement double-dispatching. It can be seem as a workaroud for the shortcomings of mainstream languages (e.g. Java, C# and C++). Also, the client code has to deal with the visitors, resulting in a bulky API.
- It assumes that the hierarchy of visited classes doesn't change. If it changes, the visitor interface and all existing visitors must be updated to accomodate the change. There's a cyclic dependency between the classes in the hierarchy and the methods in the visitor.
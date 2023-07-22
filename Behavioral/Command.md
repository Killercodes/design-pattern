**Command Design Pattern**
===

The Command pattern is a behavioral design pattern that encapsulates a request or an action as an object, thereby allowing you to parameterize clients with different requests, queue or log requests, and support undoable operations. It promotes decoupling between the object that invokes a request and the object that knows how to handle it.

> The Command design pattern encapsulates a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

Command is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations.

> **Usage examples:** The Command pattern is pretty common in C# code. Most often it’s used as an alternative for callbacks to parameterizing UI elements with actions. It’s also used for queueing tasks, tracking operations history, etc.

> **Identification:** The Command pattern is recognizable by behavioral methods in an abstract/interface type (sender) which invokes a method in an implementation of a different abstract/interface type (receiver) which has been encapsulated by the command implementation during its creation. Command classes are usually limited to specific actions.

The Command pattern is useful in situations where you want to separate the responsibilities of issuing requests, storing requests, and carrying out requests. It provides a way to achieve this separation while maintaining a flexible, extensible architecture.

Some common use cases for the Command pattern include implementing undo/redo functionality, queuing requests, logging requests, and implementing transactions.

## Example 1

```cs
// The Command interface declares a method for executing a command.
public interface ICommand
{
    void Execute();
}

// Some commands can implement simple operations on their own.
class SimpleCommand : ICommand
{
    private string _payload = string.Empty;

    public SimpleCommand(string payload)
    {
        this._payload = payload;
    }

    public void Execute()
    {
        Console.WriteLine($"SimpleCommand: See, I can do simple things like printing ({this._payload})");
    }
}

// However, some commands can delegate more complex operations to other
// objects, called "receivers."
class ComplexCommand : ICommand
{
    private Receiver _receiver;

    // Context data, required for launching the receiver's methods.
    private string _a;

    private string _b;

    // Complex commands can accept one or several receiver objects along
    // with any context data via the constructor.
    public ComplexCommand(Receiver receiver, string a, string b)
    {
        this._receiver = receiver;
        this._a = a;
        this._b = b;
    }

    // Commands can delegate to any methods of a receiver.
    public void Execute()
    {
        Console.WriteLine("ComplexCommand: Complex stuff should be done by a receiver object.");
        this._receiver.DoSomething(this._a);
        this._receiver.DoSomethingElse(this._b);
    }
}

// The Receiver classes contain some important business logic. They know how
// to perform all kinds of operations, associated with carrying out a
// request. In fact, any class may serve as a Receiver.
class Receiver
{
    public void DoSomething(string a)
    {
        Console.WriteLine($"Receiver: Working on ({a}.)");
    }

    public void DoSomethingElse(string b)
    {
        Console.WriteLine($"Receiver: Also working on ({b}.)");
    }
}

// The Invoker is associated with one or several commands. It sends a
// request to the command.
class Invoker
{
    private ICommand _onStart;

    private ICommand _onFinish;

    // Initialize commands.
    public void SetOnStart(ICommand command)
    {
        this._onStart = command;
    }

    public void SetOnFinish(ICommand command)
    {
        this._onFinish = command;
    }

    // The Invoker does not depend on concrete command or receiver classes.
    // The Invoker passes a request to a receiver indirectly, by executing a
    // command.
    public void DoSomethingImportant()
    {
        Console.WriteLine("Invoker: Does anybody want something done before I begin?");
        if (this._onStart is ICommand)
        {
            this._onStart.Execute();
        }
        
        Console.WriteLine("Invoker: ...doing something really important...");
        
        Console.WriteLine("Invoker: Does anybody want something done after I finish?");
        if (this._onFinish is ICommand)
        {
            this._onFinish.Execute();
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code can parameterize an invoker with any commands.
        Invoker invoker = new Invoker();
        invoker.SetOnStart(new SimpleCommand("Say Hi!"));
        Receiver receiver = new Receiver();
        invoker.SetOnFinish(new ComplexCommand(receiver, "Send email", "Save report"));

        invoker.DoSomethingImportant();
    }
}
```


## Example 2
```cs
/// <summary>
/// The 'Command' abstract class
/// </summary>
public abstract class Command
{
    protected Receiver receiver;
    // Constructor
    public Command(Receiver receiver)
    {
        this.receiver = receiver;
    }
    public abstract void Execute();
}

/// <summary>
/// The 'ConcreteCommand' class
/// </summary>
public class ConcreteCommand : Command
{
    // Constructor
    public ConcreteCommand(Receiver receiver) :
        base(receiver)
    {
    }
    public override void Execute()
    {
        receiver.Action();
    }
}

/// <summary>
/// The 'Receiver' class
/// </summary>
public class Receiver
{
    public void Action()
    {
        Console.WriteLine("Called Receiver.Action()");
    }
}

/// <summary>
/// The 'Invoker' class
/// </summary>
public class Invoker
{
    Command command;
    public void SetCommand(Command command)
    {
        this.command = command;
    }
    public void ExecuteCommand()
    {
        command.Execute();
    }
}

void Main()
{
	@"The Command design pattern encapsulates a request as an object, thereby letting you parameterize 
	clients with different requests, queue or log requests, and support undoable operations.".Dump("Command Design Pattern");
	
	// Create receiver, command, and invoker
  Receiver receiver = new Receiver();
  Command command = new ConcreteCommand(receiver);
  Invoker invoker = new Invoker();
  
  // Set and execute command
  invoker.SetCommand(command);
  invoker.ExecuteCommand();
}
```

## Example 3
The Command pattern consists of the following main components:

1. Command: Defines an interface for executing an operation.
2. ConcreteCommand: Implements the Command interface and defines the binding between the receiver object and the action to be executed.
3. Receiver: Represents the object that receives the request and performs the action.
4. Invoker: Requests the command to execute the operation.
5. Client: Creates and configures the commands, as well as optionally the receivers and the invokers.


```cs
using System;

// Receiver
class Light
{
    public void TurnOn()
    {
        Console.WriteLine("The light is on");
    }

    public void TurnOff()
    {
        Console.WriteLine("The light is off");
    }
}

// Command
interface ICommand
{
    void Execute();
}

// Concrete Command
class TurnOnCommand : ICommand
{
    private Light light;

    public TurnOnCommand(Light light)
    {
        this.light = light;
    }

    public void Execute()
    {
        light.TurnOn();
    }
}

// Concrete Command
class TurnOffCommand : ICommand
{
    private Light light;

    public TurnOffCommand(Light light)
    {
        this.light = light;
    }

    public void Execute()
    {
        light.TurnOff();
    }
}

// Invoker
class RemoteControl
{
    private ICommand onCommand;
    private ICommand offCommand;

    public RemoteControl(ICommand onCommand, ICommand offCommand)
    {
        this.onCommand = onCommand;
        this.offCommand = offCommand;
    }

    public void PressOnButton()
    {
        onCommand.Execute();
    }

    public void PressOffButton()
    {
        offCommand.Execute();
    }
}

// Client
class Program
{
    static void Main(string[] args)
    {
        Light light = new Light();
        ICommand turnOnCommand = new TurnOnCommand(light);
        ICommand turnOffCommand = new TurnOffCommand(light);

        RemoteControl remote = new RemoteControl(turnOnCommand, turnOffCommand);

        remote.PressOnButton();
        remote.PressOffButton();

        Console.ReadKey();
    }
}


```
In this example, we have a Light class which acts as the Receiver. We have an ICommand interface which defines the Execute method, and two concrete commands: TurnOnCommand and TurnOffCommand. The RemoteControl class acts as the Invoker, which holds references to the onCommand and offCommand and executes them when the PressOnButton and PressOffButton methods are called. Finally, in the Main method, we create a Light, the TurnOnCommand and TurnOffCommand instances, and a RemoteControl, and execute the commands by calling the PressOnButton and PressOffButton methods on the RemoteControl.

## Javascript

```js
// Command interface
class ICommand {
  execute() {}
}

// Concrete command classes
class LightOnCommand extends ICommand {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.switchOn();
  }
}

class LightOffCommand extends ICommand {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.switchOff();
  }
}

// Receiver class
class Light {
  constructor() {
    this.isOn = false;
  }

  switchOn() {
    this.isOn = true;
    console.log('Light is on');
  }

  switchOff() {
    this.isOn = false;
    console.log('Light is off');
  }
}

// Invoker class
class Switch {
  constructor() {
    this.commands = {};
  }

  addCommand(name, command) {
    this.commands[name] = command;
  }

  executeCommand(name) {
    if (this.commands[name]) {
      this.commands[name].execute();
    } else {
      console.log(`Command "${name}" not found`);
    }
  }
}

// Client code
const light = new Light();
const switcher = new Switch();

const lightOnCommand = new LightOnCommand(light);
const lightOffCommand = new LightOffCommand(light);

switcher.addCommand('on', lightOnCommand);
switcher.addCommand('off', lightOffCommand);

switcher.executeCommand('on'); // Output: Light is on
switcher.executeCommand('off'); // Output: Light is off
switcher.executeCommand('toggle'); // Output: Command "toggle" not found

```




## Python

```py
# Command interface
class ICommand:
    def execute(self):
        pass

# Concrete command classes
class LightOnCommand(ICommand):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.switch_on()

class LightOffCommand(ICommand):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.switch_off()

# Receiver class
class Light:
    def __init__(self):
        self.is_on = False

    def switch_on(self):
        self.is_on = True
        print('Light is on')

    def switch_off(self):
        self.is_on = False
        print('Light is off')

# Invoker class
class Switch:
    def __init__(self):
        self.commands = {}

    def add_command(self, name, command):
        self.commands[name] = command

    def execute_command(self, name):
        if name in self.commands:
            self.commands[name].execute()
        else:
            print(f'Command "{name}" not found')

# Client code
light = Light()
switcher = Switch()

light_on_command = LightOnCommand(light)
light_off_command = LightOffCommand(light)

switcher.add_command('on', light_on_command)
switcher.add_command('off', light_off_command)

switcher.execute_command('on')  # Output: Light is on
switcher.execute_command('off')  # Output: Light is off
switcher.execute_command('toggle')  # Output: Command "toggle" not found

```

---

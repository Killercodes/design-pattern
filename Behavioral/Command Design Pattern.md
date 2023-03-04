**Command Design Pattern**
===

The Command pattern is a behavioral design pattern that encapsulates a request or an action as an object, thereby allowing you to parameterize clients with different requests, queue or log requests, and support undoable operations. It promotes decoupling between the object that invokes a request and the object that knows how to handle it.

The Command pattern consists of the following main components:

1. Command: Defines an interface for executing an operation.
2. ConcreteCommand: Implements the Command interface and defines the binding between the receiver object and the action to be executed.
3. Receiver: Represents the object that receives the request and performs the action.
4. Invoker: Requests the command to execute the operation.
5. Client: Creates and configures the commands, as well as optionally the receivers and the invokers.

The Command pattern is useful in situations where you want to separate the responsibilities of issuing requests, storing requests, and carrying out requests. It provides a way to achieve this separation while maintaining a flexible, extensible architecture.

Some common use cases for the Command pattern include implementing undo/redo functionality, queuing requests, logging requests, and implementing transactions.

## C#

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

```ja
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

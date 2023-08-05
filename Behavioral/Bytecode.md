Bytecode
===

The Bytecode design pattern is a way to create a custom programming language, called bytecode, that can be used to drive the behavior of a system. This pattern is commonly used in game development, where it allows users to modify the behavior of the game without having direct access to the game engine or its source code

An instruction set defines the low-level operations that can be performed. A series of instructions is encoded as a sequence of bytes. A virtual machine executes these instructions one at a time, using a stack for intermediate values. By combining instructions, complex high-level behavior can be defined.

> Bytecode pattern enables behavior driven by data instead of code.

## Intent
Allows encoding behavior as instructions for a virtual machine.

```cs
public class BytecodeInterpreter
{
    private readonly Stack<int> _stack = new Stack<int>();
	
	public BytecodeInterpreter()
	{
		_stack.Push(1);
		_stack.Push(2);
		_stack.Push(3);
	}

    public void Execute(OpCode[] bytecode)
    {
        foreach (var op in bytecode)
        {
            switch (op)
            {
                case OpCode.Add:
                    if (_stack.Count < 2)
                        throw new InvalidOperationException("Stack must contain at least two elements to perform an Add operation.");
                    _stack.Push(_stack.Pop() + _stack.Pop());
                    break;
                case OpCode.Subtract:
                    if (_stack.Count < 2)
                        throw new InvalidOperationException("Stack must contain at least two elements to perform a Subtract operation.");
                    _stack.Push(_stack.Pop() - _stack.Pop());
                    break;
                case OpCode.Multiply:
                    if (_stack.Count < 2)
                        throw new InvalidOperationException("Stack must contain at least two elements to perform a Multiply operation.");
                    _stack.Push(_stack.Pop() * _stack.Pop());
                    break;
                case OpCode.Divide:
                    if (_stack.Count < 2)
                        throw new InvalidOperationException("Stack must contain at least two elements to perform a Divide operation.");
                    _stack.Push(_stack.Pop() / _stack.Pop());
                    break;
            }
        }
    }

    public int Result => _stack.Peek();
}

void Main()
{
	var interpreter = new BytecodeInterpreter();
	interpreter.Execute(new[] { OpCode.Add, OpCode.Multiply });
	int result = interpreter.Result;
}

```
In this example, we define an OpCode enumeration that represents the different operations that our bytecode language can perform. The BytecodeInterpreter class provides a way to execute a sequence of these operations. The Execute method takes an array of OpCode values and uses a stack to perform the specified operations.

In this example, we create an instance of the BytecodeInterpreter class and use it to execute a sequence of OpCode values that represent adding and multiplying two numbers. The result of this operation is then retrieved from the Result property of the interpreter instance.
Interpreter Design Pattern
===

The Interpreter pattern is a design pattern that specifies how to evaluate sentences in a language. It is useful when you have a language to represent data and you need to perform operations on it. The pattern specifies that you create an interpreter that can parse the language and execute the operations.

The Interpreter pattern consists of three main components: the context, the expression, and the interpreter. The context is where the data is stored and where the operations are performed. The expression is a representation of a sentence in the language, and the interpreter is responsible for interpreting the expression and executing the operations.

The Interpreter pattern is often used in combination with the Composite pattern, which allows you to build complex expressions from simpler ones.

The Interpreter pattern can be useful in situations where you need to perform operations on data that is represented in a custom language, or in situations where you need to implement a custom language for a specific task.


The Interpreter design pattern is a behavioral pattern that is used to define a language or notation, and interpret expressions or sentences in that language. It is commonly used in language processors, compilers, and interpreters.

The pattern involves defining a language grammar, and then implementing a parser that can interpret the expressions in the language. The parser breaks down the expressions into a parse tree, and then uses the tree to execute the expressions.

The Interpreter pattern involves several key components:

1. Abstract Expression: This defines the base interface for all expressions in the language.

2. Terminal Expression: These are the smallest expressions in the language, and can be evaluated without needing to be broken down further.

3. Non-Terminal Expression: These are expressions that are made up of one or more sub-expressions.

4. Context: This holds any information or state needed by the interpreter.

5. Client: This provides the input to the interpreter.

The Interpreter pattern can be useful when you need to define a new language, or extend an existing language. It can also be used when you need to implement complex rules or logic that would be difficult to implement using other design patterns.


## C#
```cs
using System;
using System.Collections.Generic;

// Abstract expression class
abstract class Expression
{
    public abstract int Interpret();
}

// Terminal expression class
class NumberExpression : Expression
{
    private int number;

    public NumberExpression(int number)
    {
        this.number = number;
    }

    public override int Interpret()
    {
        return number;
    }
}

// Nonterminal expression class for addition
class AddExpression : Expression
{
    private Expression leftOperand, rightOperand;

    public AddExpression(Expression left, Expression right)
    {
        leftOperand = left;
        rightOperand = right;
    }

    public override int Interpret()
    {
        return leftOperand.Interpret() + rightOperand.Interpret();
    }
}

// Nonterminal expression class for subtraction
class SubtractExpression : Expression
{
    private Expression leftOperand, rightOperand;

    public SubtractExpression(Expression left, Expression right)
    {
        leftOperand = left;
        rightOperand = right;
    }

    public override int Interpret()
    {
        return leftOperand.Interpret() - rightOperand.Interpret();
    }
}

// Context class
class Context
{
    private Dictionary<string, Expression> variables = new Dictionary<string, Expression>();

    public void SetVariable(string name, Expression expression)
    {
        variables[name] = expression;
    }

    public Expression GetVariable(string name)
    {
        if (!variables.ContainsKey(name))
        {
            throw new ArgumentException("Variable not found.");
        }

        return variables[name];
    }
}

// Client code
class Program
{
    static void Main(string[] args)
    {
        // Create a context
        Context context = new Context();

        // Define some variables
        Expression a = new NumberExpression(1);
        Expression b = new NumberExpression(2);
        Expression c = new NumberExpression(3);

        // Assign variables to context
        context.SetVariable("a", a);
        context.SetVariable("b", b);
        context.SetVariable("c", c);

        // Evaluate expressions
        Expression add = new AddExpression(new AddExpression(context.GetVariable("a"), context.GetVariable("b")), context.GetVariable("c"));
        Console.WriteLine(add.Interpret()); // Output: 6

        Expression subtract = new SubtractExpression(new SubtractExpression(context.GetVariable("c"), context.GetVariable("b")), context.GetVariable("a"));
        Console.WriteLine(subtract.Interpret()); // Output: 0
    }
}

```
In this example, we have created a simple interpreter that can interpret basic mathematical expressions. We have created an abstract Expression class, which has two concrete subclasses NumberExpression and AddExpression and SubtractExpression. We have also created a Context class that keeps track of the values of variables used in the expressions.

In the Client code, we create a Context object and define some variables using NumberExpression. We then use these variables to create and evaluate AddExpression and SubtractExpression objects. The Interpret() method of each Expression object is called to calculate the final result of the expression.



## Javascript

```js
class Context {
  constructor(input) {
    this.input = input;
    this.output = 0;
  }
}

class AbstractExpression {
  interpret(context) {}
}

class TerminalExpression extends AbstractExpression {
  interpret(context) {
    if (context.input.startsWith(this.symbol)) {
      context.output += this.value;
      context.input = context.input.substring(1);
    }
  }
  constructor(symbol, value) {
    super();
    this.symbol = symbol;
    this.value = value;
  }
}

class NonterminalExpression extends AbstractExpression {
  constructor() {
    super();
    this.expressionList = [];
  }

  interpret(context) {
    for (const e of this.expressionList) {
      e.interpret(context);
    }
  }

  add(expression) {
    this.expressionList.push(expression);
  }
}

// Usage
const context = new Context('XXXIX');
const expression = new NonterminalExpression();
expression.add(new TerminalExpression('X', 10));
expression.add(new TerminalExpression('V', 5));
expression.add(new TerminalExpression('I', 1));
expression.interpret(context);
console.log(context.output);

```


## Python

```py
class Context:
    def __init__(self, input):
        self.input = input
        self.output = 0

class AbstractExpression:
    def interpret(self, context):
        pass

class TerminalExpression(AbstractExpression):
    def __init__(self, symbol, value):
        self.symbol = symbol
        self.value = value

    def interpret(self, context):
        if context.input.startswith(self.symbol):
            context.output += self.value
            context.input = context.input[1:]

class NonterminalExpression(AbstractExpression):
    def __init__(self):
        self.expressionList = []

    def interpret(self, context):
        for e in self.expressionList:
            e.interpret(context)

    def add(self, expression):
        self.expressionList.append(expression)

# Usage
context = Context('XXXIX')
expression = NonterminalExpression()
expression.add(TerminalExpression('X', 10))
expression.add(TerminalExpression('V', 5))
expression.add(TerminalExpression('I', 1))
expression.interpret(context)
print(context.output)

```


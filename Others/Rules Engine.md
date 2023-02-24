**Rules Engine in C#**
===

It is an expert-system program, which runs the rules on the data and if any condition matches then it executes the corresponding actions.

```cs
public class Rule<T>
{
    private string name;
    private Func<T, bool> condition;
    private Action action;

    public Rule(string name, Func<T, bool> condition, Action action)
    {
        this.name = name;
        this.condition = condition;
        this.action = action;
    }

    public bool Evaluate(T input)
    {
        return condition(input);
    }

    public void Execute()
    {
        action();
    }
}
```


In this version of the Rule class, the condition field is of type Func<T, bool>, where T is the type of the input data that the rule will evaluate. This allows us to define rules for different types of input data, not just int.

Here's an example of how you can use the updated Rule class with a string input:

```cs
List<Rule<string>> rules = new List<Rule<string>>();

rules.Add(new Rule<string>("String contains 'hello'", (string s) => s.Contains("hello"), () => Console.WriteLine("String contains 'hello'")));

rules.Add(new Rule<string>("String does not contain 'hello'", (string s) => !s.Contains("hello"), () => Console.WriteLine("String does not contain 'hello'")));

Console.WriteLine("Enter a string:");
string input = Console.ReadLine();

foreach (Rule<string> rule in rules)
{
    if (rule.Evaluate(input))
    {
        rule.Execute();
    }
}
```

In this example, we define two Rule<string> objects that evaluate whether a string input contains the word "hello" or not. We then prompt the user to input a string and store it in the input variable.

We evaluate each rule by calling its Evaluate method with the input string. If the rule's condition is satisfied, we execute its action by calling its Execute method.

Note that this implementation is still very simple and doesn't handle more complex rule-based systems. However, it demonstrates how you can use a generic condition to define rules for different types of input data.

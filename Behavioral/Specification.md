**Specification Design pattern**
===

The specification pattern is a particular software design pattern, whereby business rules can be recombined by chaining the business rules together using boolean logic. The pattern is frequently used in the context of domain-driven design.

>A specification pattern outlines a business rule that is combinable with other business rules.

In this pattern, a unit of business logic inherits its functionality from the abstract aggregate Composite Specification class.

```cs
public interface ISpecification
{
    bool IsSatisfiedBy(object candidate);
    ISpecification And(ISpecification other);
    ISpecification AndNot(ISpecification other);
    ISpecification Or(ISpecification other);
    ISpecification OrNot(ISpecification other);
    ISpecification Not();
}

public abstract class CompositeSpecification : ISpecification 
{
    public abstract bool IsSatisfiedBy(object candidate);

    public ISpecification And(ISpecification other) 
    {
        return new AndSpecification(this, other);
    }

    public ISpecification AndNot(ISpecification other) 
    {
        return new AndNotSpecification(this, other);
    }

    public ISpecification Or(ISpecification other) 
    {
        return new OrSpecification(this, other);
    }

    public ISpecification OrNot(ISpecification other) 
    {
        return new OrNotSpecification(this, other);
    }

    public ISpecification Not() 
    {
       return new NotSpecification(this);
    }
}

public class AndSpecification : CompositeSpecification 
{
    private ISpecification leftCondition;
    private ISpecification rightCondition;

    public AndSpecification(ISpecification left, ISpecification right) 
    {
        leftCondition = left;
        rightCondition = right;
    }

    public override bool IsSatisfiedBy(object candidate) 
    {
        return leftCondition.IsSatisfiedBy(candidate) && rightCondition.IsSatisfiedBy(candidate);
    }
}

public class AndNotSpecification : CompositeSpecification 
{
    private ISpecification leftCondition;
    private ISpecification rightCondition;

    public AndNotSpecification(ISpecification left, ISpecification right) 
    {
        leftCondition = left;
        rightCondition = right;
    }

    public override bool IsSatisfiedBy(object candidate) 
    {
        return leftCondition.IsSatisfiedBy(candidate) && rightCondition.IsSatisfiedBy(candidate) != true;
    }
}

public class OrSpecification : CompositeSpecification
{
    private ISpecification leftCondition;
    private ISpecification rightCondition;

    public OrSpecification(ISpecification left, ISpecification right) 
    {
        leftCondition = left;
        rightCondition = right;
    }

    public override bool IsSatisfiedBy(object candidate) 
    {
        return leftCondition.IsSatisfiedBy(candidate) || rightCondition.IsSatisfiedBy(candidate);
    }
}

public class OrNotSpecification : CompositeSpecification
{
    private ISpecification leftCondition;
    private ISpecification rightCondition;

    public OrNotSpecification(ISpecification left, ISpecification right) 
    {
        leftCondition = left;
        rightCondition = right;
    }

    public override bool IsSatisfiedBy(object candidate) 
    {
        return leftCondition.IsSatisfiedBy(candidate) || rightCondition.IsSatisfiedBy(candidate) != true;
    }
}

public class NotSpecification : CompositeSpecification 
{
    private ISpecification Wrapped;

    public NotSpecification(ISpecification x) 
    {
        Wrapped = x;
    }

    public override bool IsSatisfiedBy(object candidate) 
    {
        return !Wrapped.IsSatisfiedBy(candidate);
    }
}
```

With Generics
```cs
public interface ISpecification<T>
{
    bool IsSatisfiedBy(T candidate);
    ISpecification<T> And(ISpecification<T> other);
    ISpecification<T> AndNot(ISpecification<T> other);
    ISpecification<T> Or(ISpecification<T> other);
    ISpecification<T> OrNot(ISpecification<T> other);
    ISpecification<T> Not();
}

public abstract class LinqSpecification<T> : CompositeSpecification<T>
{
    public abstract Expression<Func<T, bool>> AsExpression();
    public override bool IsSatisfiedBy(T candidate) => AsExpression().Compile()(candidate);
}

public abstract class CompositeSpecification<T> : ISpecification<T>
{
    public abstract bool IsSatisfiedBy(T candidate);
    public ISpecification<T> And(ISpecification<T> other) => new AndSpecification<T>(this, other);
    public ISpecification<T> AndNot(ISpecification<T> other) => new AndNotSpecification<T>(this, other);
    public ISpecification<T> Or(ISpecification<T> other) => new OrSpecification<T>(this, other);
    public ISpecification<T> OrNot(ISpecification<T> other) => new OrNotSpecification<T>(this, other);
    public ISpecification<T> Not() => new NotSpecification<T>(this);
}

public class AndSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public AndSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) && right.IsSatisfiedBy(candidate);
}

public class AndNotSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public AndNotSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) && !right.IsSatisfiedBy(candidate);
}

public class OrSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public OrSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) || right.IsSatisfiedBy(candidate);
}
public class OrNotSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> left;
    ISpecification<T> right;

    public OrNotSpecification(ISpecification<T> left, ISpecification<T> right)
    {
        this.left = left;
        this.right = right;
    }

    public override bool IsSatisfiedBy(T candidate) => left.IsSatisfiedBy(candidate) || !right.IsSatisfiedBy(candidate);
}

public class NotSpecification<T> : CompositeSpecification<T>
{
    ISpecification<T> other;
    public NotSpecification(ISpecification<T> other) => this.other = other;
    public override bool IsSatisfiedBy(T candidate) => !other.IsSatisfiedBy(candidate);
}
```

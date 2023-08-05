Interface Segregation Principle
===

Reduce fat interfaces into multiple smaller and more specific client specific interfaces. An interface should be more dependent on the code that calls it than the code that implements it.

## Why
- If a class implements methods that are not needed the caller needs to know about the method implementation of that class. For example if a class implements a method but simply throws then the caller will need to know that this method shouldn't actually be called.

## How
- Avoid fat interfaces. Classes should never have to implement methods that violate the Single responsibility principle.

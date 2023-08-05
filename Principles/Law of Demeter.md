Law of Demeter
===
Don't talk to strangers.

## Why
- It usually tightens coupling
- It might reveal too much implementation details

## How
A method of an object may only call methods of:
- The object itself.
- An argument of the method.
- Any object created within the method.
- Any direct properties/fields of the object.

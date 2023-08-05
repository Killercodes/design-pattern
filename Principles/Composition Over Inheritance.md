Composition Over Inheritance
===

## Why
- Less coupling between classes.
- Using inheritance, subclasses easily make assumptions, and break LSP.

## How
- Test for LSP (substitutability) to decide when to inherit.
- Compose when there is a "has a" (or "uses a") relationship, inherit when "is a".
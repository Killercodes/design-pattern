Rule of three
===

Rule of three (`"Three strikes and you refactor"`) is a code refactoring rule of thumb to decide **when similar pieces of code should be refactored to avoid duplication.** It states that `two instances of similar code do not require refactoring, but when similar code is used three times, it should be extracted into a new procedure.` The rule was popularised by Martin Fowler in Refactoring and attributed to Don Roberts.

Duplication is considered a bad practice in programming because it makes the code harder to maintain. When the rule encoded in a replicated piece of code changes, whoever maintains the code will have to change it in all places correctly.

However, choosing an appropriate design to avoid duplication might benefit from more examples to see patterns in. Attempting premature refactoring risks selecting a wrong abstraction, which can result in worse code as new requirements emerge[2] and will eventually need to be refactored again.

The rule implies that the cost of maintenance outweighs the cost of refactoring and potential bad design when there are three copies, and may or may not if there are only two copies.

# Smalltalk Best Practice Patterns - Javascript

Javascript implementations of the patterns from the Smalltalk Best Practice Patterns book by Kent Beck.

Purchase the book here

## Patterns

### Behaviour

#### Methods

- Composed Method
- Constructor Method
- Constructor Parameter Method
- Shortcut Constructor Method
- Conversion
- Converter Method
- Converter Constructor Method
- Query Method
- Comparing Method
- Reversing Method
- Method Object
- Execute Around Method
- Debug Printing Method
- Method Comment

#### Messages

- Message
- Choosing Message
- Decomposing Message
- Intention Revealing Message
- Intention Revealing Selector
- Dispatched Interpretation
- Double Dispatch
- Mediating Protocol
- Super
- Extending Super
- Modifying Super
- Delegation
- Simple Delegation
- Self Delegation
- Pluggable Behaviour
- Pluggable Selector
- Pluggable Block
- Collecting Parameter

### State

#### Instance Variables

- Common State
- Variable State
- Explicit Initialization
- Lazy Initialization
- Default Value Method
- Constant Method
- Direct Variable Access
- Indirect Variable Access
- Getting Method
- Setting Method
- Collection Accessor Method
- Enumeration Method
- Boolean Property Setting Method
- Role Suggesting Instance Variable Name

#### Temporary Variables

- Temporary Variable
- Collecting Temporary Variable
- Caching Temporary Variable
- Explaining Temporary Variable
- Reusing Temporary Variable
- Role Suggesting Temporary Variable Name

### Collections

#### Classes

- Collection
- OrderedCollection
- RunArray
- Set
- Equality Method
- Hashing Method
- Dictionary
- SortedCollection
- Array
- ByteArray
- Interval

#### Collection Protocol

- IsEmpty
- Includes
- Concatentation
- Enumeration
- Do
- Collect
- State/Reject
- Detect
- Inject:into

#### Collection Idioms

- Duplicate Removing Set
- Temporarily Sorted Collection
- Stack
- Queue
- Searching Literal
- Lookup Cache
- Parsing Stream
- Concatenating Stream
- Simple Superclass Name
- Qualified Subclass Name

### Formatting

- Inline Message Pattern
- Type Suggesting Parameter Name
- Indented Control Flow
- Rectangular Block
- Guard Clause
- Conditional Expression
- Simple Enumeration Parameter
- Cascade
- Yourself
- Interesting Return Patterns

### Reference

## Behaviour

### Composed Method

How do you divide a program into methods?

Divide your program into methods that perform one identifiable task. Keep all of
the operations in a method at the same level of abstraction. This will naturally
result in programs with many small methods, each a few lines long.

Anytime you are sending two or more messages from one object to another in a
single method, you may be able to create a Composed Method in the receiver
that combines those messages.

```javascript
class Controller {
  controlInitialise() {
    console.log('Controller initialising...');
  }

  controlLoop() {
    console.log('Controller looping: 1, 2, 3');
  }

  controlTerminate() {
    console.log('Controller terminated');
  }

  controlActivity() {
    this.controlInitialise();
    this.controlLoop();
    this.controlTerminate();
  }
}

const controller = new Controller();

controller.controlActivity();
// => Controller initialising...
// => Controller looping: 1, 2, 3
// => Controller terminated
```

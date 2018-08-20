# Smalltalk Best Practice Patterns - Javascript

My best efforts of converting the patterns in SmallTalk best practices by Kent Beck in Javascript.

I've been working from (Ruby conversions)[https://github.com/ruben/smalltalk-best-practice-patterns/tree/master/3-behaviour/1-methods] and conversions of the SmallTalk language itself to (Java)[http://users.informatik.haw-hamburg.de/~pfeiffer/pub/javasmalltalksyntax.html] then to Javascript from my own intuition so some of the representations might be off.

PR's are welcomed with suggestions on improvements, fixes and contributions (look for the TODOs).

Purchase your own copy of the book (here)[https://www.amazon.co.uk/Smalltalk-Best-Practice-Patterns-Kent/dp/013476904X].

## Contents

Most patterns are designed to preserve a balance between short-term and long-term needs. The bottlenecks
throughout development come from the limitations in human communication. When you program, you have to think
about how someone will read your code, not just how a computer will interpret it.

Good predictors of whether a Software project is in good shape:

- Once and only once (DRY)
- Lots of little pieces
- Replacing objects
- Moving objects
- Objects that can be easily moved to new contexts
- Ship a couple of systems first
- Then generalise little pieces easily
- Rates of change
- Same level of abstraction
- Don't put two rates of change together
- Avoid this problem by making lots of little pieces

The problems in the construction of objects are universal

1. Name Classes
2. Relate Classes via inheritance and delegation
3. Relate methods in the same class and different classes
4. Name variables

Patterns record these problems and how to approach solving them. They are literacy form of capturing and
transmitting common practice. Each pattern records a recurring problem, how to construct a solution
for the problem and why the solution is appropriate.

## Patterns from the book

### Behaviour

Patterns for methods and messages. The problems you can solve by creating a new method. You will also find method pat- terns scattered throughout the remainder of the patterns, if a particular kind of method (like accessors) is tied to another pattern. The quick reference guide gathers all the method patterns together for easy reference.

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

Patterns for using instance and temporary variables. You will find a discussion of the pros and cons of using accessor methods and direct variable access.

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

The major collection classes and messages in the form of patterns. You will also find common tricks using collections.

#### Classes

A brief introduction to how to use classes, since most of the topics regarding class creation are outside the scope of this book.

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

# Patterns

## Behaviour

#### Composed Method

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

#### Constructor Method

How do you represent instance creation?

Provide methods that create well-formed instances. Pass all required parameters
to them on instance creation.

```Javascript
// TODO
```

#### Constructor Parameter Method

How do you set instance variables from the parameters to a Constructor Method?

Code a single method that sets all the variables. Preface its name with 'set',
then the names of the variables.

```Javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}
```

#### Shortcut Constructor Method

What is the external interface for creating a new object when a Constructor
Method is too wordy?

Represent object creation as a message to one of the arguments to the Constructor
Method. Add no more than three of these Shortcut Constructor Methods per system
you develop.

```Javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
};

// TODO
class Number {
  // original book example used @
  at(y) {
    return new Point(this, y)
  }
};
```

### Conversion

#### Converter Method

How do you represent simple conversion of an object to another object with the same
protocol but different format?

Provide a method in the object to be converted that converts to the new object.
Name the method by prepending “as” to the class of the object.

```Javascript
class Collection {
  asSet() {

  }
}

class Number {
  asFloat() {

  }
}
```

### Converter Constructor Method

You need to implement Conversion to a new kind of object.

How do you represent the conversion of an object to another with a different
protocol?

Make a Constructor Method that takes the object to be converted as an argument.

```Javascript
class Date {
  constructor(string) {
    this._string = string;
  }
}
```

### Query Method

A composed method has had to execute a boolean expression.

How do you represent testing a property of an object?

Provide a method that returns a Boolean. Name it by prefacing the property name
with a form of "be" - is, was, will etc e.g

- isNil
- isControlWanted
- isEmpty

```javascript
class Switch {
  constructor() {
    this._status = 'off';
  }

  makeOn() {
    this._status = 'on';
  }

  makeOff() {
    this._status = 'off'
  }

  isOn() {
    return this._status === 'on' ? true : false;
  }

  isOff() {
    return this._status === 'off' ? true : false;
  }

  status() {
    return this._status
  }
}

const smallSwitch = new Switch()

smallSwitch.makeOn()
const smallSwitchIsOn = smallSwitch.isOn()
// => true
smallSwitch.makeOff()
const smallSwitchIsOn = smallSwitch.isOff()
// => true
smallSwitch.status()
// => off
```

### Comparing Method

How do you order objects with respect to each other?

Put comparing methods in a protocol called 'comparing'.

### Reversing Method

A composed method may not read right because messages are going to too many receivers.
You may have a Cascade that doesn't look quite right because several different
objects need to receive messages.

How do you code a smooth flow of messages?

Code a method on the parameter. Derive its name from the original message. Take
the original receiver as a parameter to the new method. Implement the method by
sending the original message to the original receiver.

```javascript
// TODO
```

### Method Object

You have a method that does not simplify well with Composed Method

How do you code a method where many lines of code share many arguments and
temporary variables?

Create a class named after the method. Give it an instance variable for the receiver of the original method, each argument, and each  temporary variable. Give it a Constructor Method that takes the original receiver and the method arguments. Give it one instance method, #compute, implemented by copying the body of the original method.
Replace the method with one that creates instance of the new class and sends it #compute.

```javascript
class Obligation {
  send_task(task, job) {
    let [not_processed, processed, copied, executed] = []
  }
  // Using composed method we get a method like this
  prepareTask(task, job, notProcessed, processed, copied, executed) {

  }
}

// Method Object
class TaskSender {
  constructor(obligation, task, job) {
    this._obligation = obligation;
    this._task = task;
    this._job = job
  };

  get obligation() {
    return this._obligation
  };

  set obligation(obligation) {
    this._obligation = obligation
  };

  get task() {
    return this._task
  };

  set task(task) {
    this._task = task
  };

  get job() {
    return this._job
  };

  set job(job) {
    this._job = job
  };

  get not_processed() {
    return this._not_processed
  };

  set not_processed(not_processed) {
    this._not_processed = not_processed
  };

  get processed() {
    return this._processed
  };

  set processed(processed) {
    this._processed = processed
  };

  get copied() {
    return this._copied
  };

  set copied(copied) {
    this._copied = copied
  };

  get executed() {
    return this._executed
  };

  set executed(executed) {
    this._executed = executed
  };

  get compute() {

  }
}

class Obligation {
  sendTask(task, job) {
    const taskSender = new TaskSender(task, job);
    return taskSender.compute();
  }
}
```

### Execute Around Method

How do you represent pairs of actions that have to be taken together?

Code a method that takes a Block as an argument. Name the method by appending
"During: aBlock" to the name of the first method that needs to be invoked. In the
body of the Execute Around Method, invoke the first method, evaluate the block,
then invoke the second method.

### Method Comment

Communicate important information that is not obvious from the code in a comment
at the beginning of the method.

Here are examples of information that can be difficult to communicate solely
through the code:

#### Method Dependencies

Sometimes one method must be invoked before another can execute correctly. A
comment can warn the reader not to invoke one without the other. Sometimes
you can use Composed Method or Execute Around Method to communicate the same
information.

#### To-do

I often write comments while I am prototyping to remind myself of some thought
I don't want to lose. 'Look at using a Dictionary later for efficiency', for example.

When I reconsider the thought later, I delete the comment after choosing whether to follow it.

#### Reasons for Change

Particularly on base classes, if you need to change something, the reason for the chance
is often not immediately apparent in the code. This often occurs when changing a method
supplied by a Smalltalk vendor. A comment helps a reader understand why you did what
you did if you can't make the code say it.

## Messages

### Choosing Messages

How do you execute one of several alternatives?

Send a message to one of several different kinds of objects, each of
which executes one alternative.

```Javascript
let responsible = (entry.kindOf(Film) ? entry.producer : entry.author);

// Choosing message
responsible = entry.responsible;

class Film {
  get responsible() {
    return producer
  }
};

class Entry {
  get responsible() {
    return author
  }
}
```

### Decomposing Message

You are using a Message to break a computation into parts.

How do you invoke parts of a computation?

Send several messages to 'self'

```Javascript
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

### Intention Revealing Message

You are using a Message to invoke a computation. You may be hiding the use of
Pluggable Behaviour.

How do you communicate your intent when the implementation is simple?

The one that separates intention (what you want done) from implementation
(how it is done) communicates better to a person.

Send a message to 'self'. Name the message so it communicates what is to be done
rather than how it is to be done. Code in a simple method for the message.

```javascript
class ParagraphEditor {
  highlight(rectangle) {
    return reverse(rectangle)
  }
}

class Collection {
  isEmpty() {
    return size.zero ? true : false;
  }
};

class Number {
  get reciprocal() {
    return 1 / this
  }
};
```
### Intention Revealing Selector

How do you name a method?

Name methods after what it is supposed to accomplish and leave "how" to the
various method bodies.

```Javascript
class Collection {
  // ...
  searchFor(value) {

  }
};

class Collection {
  // ...
  includes(value) {

  }
}
```

### Dispatched Interpretation

How can two objects cooperate when one wishes to conceal its representation?

Have the client send a message to the encoded object. Pass a parameter to
which the encoded object will send decoded messages.

```javascript
// TODO
```

### Extending Super

How do you add to a superclass implementation of a method?

Override the method and send a message to "super" in the overriding method.

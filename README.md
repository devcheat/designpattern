 __Design-pattern__
===

## What are Design Patterns?

> Design patterns are solutions to software design problems you find again and again in real-world application development. Patterns are about reusable designs and interactions of objects.

A design pattern is a proven design solution to a common problem faced by software developers. Design patterns became popular with the rise of Object Oriented Analysis and Design (OOAD). Design patterns are designed to help developers deliver higher quality, more easily maintained software products in less time and at lower cost.
Design patterns are:

- **encapsulated** - They embody design knowledge regarding collaboration of classes and objects, distribution of
- **responsibility**, and other design issues.
- **object-oriented** - They incorporate OOAD principles—e.g., low coupling, high cohesion.
- **reusable** - They are adaptable, flexible, general solutions to classes of problems with broad applicability. Design patterns simplify the task facing the developer.

## How did design patterns originate?
The history of design patterns is quite short. They originated as an architectural concept proposed by Christopher Alexander in the late nineteen seventies (ca. 1977). Kent Beck and Ward Cunningham experimented with the notion of applying patterns to computer programming in the late nineteen eighties (ca. 1987). Design patterns did not become widely used in computer science until after Design Patterns: Elements of Reusable Object-Oriented Software by the so-called Gang of Four was published in 1994.

## How are design patterns classified?
Design patterns are categorized to facilitate learning and extending them. They are classified in terms of the underlying problems they address, i.e. according to usage.
The three main design pattern categories are:

- **Behavioral design patterns** - characterize the manner of class and object interaction and how responsibilities are distributed among them.
- **Creational design patterns** - address the object creation process. Creational design patterns encapsulate knowledge about how, when, and by whom an instance is created.
- **Structural design patterns** - address the composition of classes and objects. Design patterns vary both in granularity and level of abstraction.

## Creational Patterns

- __Abstract Factory__	Creates an instance of several families of classes
- __Builder__	Separates object construction from its representation
- __Factory Method__	Creates an instance of several derived classes
- __Prototype__	A fully initialized instance to be copied or cloned
- __Singleton__	A class of which only a single instance can exist

## Structural Patterns
- Adapter	Match interfaces of different classes
- Bridge	Separates an object’s interface from its implementation
- Composite	A tree structure of simple and composite objects
- Decorator	Add responsibilities to objects dynamically
- Facade	A single class that represents an entire subsystem
- Flyweight	A fine-grained instance used for efficient sharing
- Proxy	An object representing another object

## Behavioral Patterns
- Chain of Resp.	A way of passing a request between a chain of objects
- Command	Encapsulate a command request as an object
- Interpreter	A way to include language elements in a program
- Iterator	Sequentially access the elements of a collection
- Mediator	Defines simplified communication between classes
- Memento	Capture and restore an object's internal state
- Observer	A way of notifying change to a number of classes
- State	Alter an object's behavior when its state changes
- Strategy	Encapsulates an algorithm inside a class
- Template Method	Defer the exact steps of an algorithm to a subclass
- Visitor	Defines a new operation to a class without change

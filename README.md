# Basic Design Patterns

A collection of essential design patterns for software development using practical examples, such Singleton, Factory Method, Observer, and more, to help developers write more efficient and maintainable code.

## Contents

- [Patterns](#patterns)
- [Contribution Guidelines](CONTRIBUTING.md)

## Patterns

### Creational

1. [Abstract Factory](/patterns/abstract-factory/README.md) - Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
1. [Builder](/patterns/builder/README.md) - Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
1. [Factory Method](/patterns/factory-method/README.md) - Creates objects without specifying the exact class of object that will be created.
1. [Protoype](/patterns/prototype/README.md) - Creates new objects by copying an existing object, known as the prototype.
1. [Singleton](/patterns/singleton/README.md) - Ensures a class has only one instance and provides a global point of access to it.

### Structural

1. [Adapter](/patterns/adapter/README.md) - Allows incompatible interfaces to work together by converting one interface into another.
1. [Bridge](/patterns/bridge/README.md) - Decouples an abstraction from its implementation so that the two can vary independently.
1. [Composite](/patterns/composite/README.md) - Composes objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions uniformly.
1. [Decorator](/patterns/decorator/README.md) - Adds behavior or responsibilities to an object dynamically without altering its structure.
1. [Facade](/patterns/facade/README.md) - Provides a simplified interface to a larger body of code, making it easier to use.
1. [Flyweight](/patterns/flyweight/README.md) - Reduces memory usage by sharing common parts of the state between multiple objects.
1. [Proxy](/patterns/proxy/README.md) - Provides a surrogate or placeholder for another object to control access to it.

### Behavioral

1. [Chain of Responsibility](/patterns/chain-of-responsibility/README.md) - Passes a request along a chain of handlers until it is handled or reaches the end of the chain.
1. [Command](/patterns/command/README.md) - Encapsulates a request as an object, thereby allowing for parameterization and queuing of requests.
1. [Interpreter](/patterns/interpreter/README.md) - Defines a representation for a language's grammar and provides an interpreter to process sentences in that language.
1. [Iterator](/patterns/iterator/README.md) - Provides a way to access elements of a collection sequentially without exposing its underlying representation.
1. [Mediator](/patterns/mediator/README.md) - Defines an object that encapsulates how a set of objects interact, promoting loose coupling.
1. [Memento](/patterns/memento/README.md) - Captures and restores an object's internal state without violating encapsulation.
1. [Observer](/patterns/observer/README.md) - Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.
1. [State](/patterns/state/README.md) - Allows an object to alter its behavior when its internal state changes.
1. [Strategy](/patterns/strategy/README.md) - Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
1. [Template Method](/patterns/template-method/README.md) - Defines the skeleton of an algorithm in a method, deferring some steps to subclasses.
1. [Visitor](/patterns/visitor/README.md) - Separates an algorithm from the objects it operates on by allowing new operations to be added without modifying the objects.

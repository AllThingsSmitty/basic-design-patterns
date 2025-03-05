# Visitor

The Visitor pattern allows you to add further operations to objects without having to modify them. It separates an algorithm from the object structure on which it operates.

### Example

```js
// Visitor Interface
class Visitor {
  visitConcreteElementA(element) {}
  visitConcreteElementB(element) {}
}

// Concrete Visitor
class ConcreteVisitor1 extends Visitor {
  visitConcreteElementA(element) {
    console.log(`ConcreteVisitor1: ${element.operationA()}`);
  }

  visitConcreteElementB(element) {
    console.log(`ConcreteVisitor1: ${element.operationB()}`);
  }
}

class ConcreteVisitor2 extends Visitor {
  visitConcreteElementA(element) {
    console.log(`ConcreteVisitor2: ${element.operationA()}`);
  }

  visitConcreteElementB(element) {
    console.log(`ConcreteVisitor2: ${element.operationB()}`);
  }
}

// Element Interface
class Element {
  accept(visitor) {}
}

// Concrete Elements
class ConcreteElementA extends Element {
  accept(visitor) {
    visitor.visitConcreteElementA(this);
  }

  operationA() {
    return "ConcreteElementA";
  }
}

class ConcreteElementB extends Element {
  accept(visitor) {
    visitor.visitConcreteElementB(this);
  }

  operationB() {
    return "ConcreteElementB";
  }
}
```

### Explaining the code

1. **Visitor Interface:**

   - This is an abstract class that defines the interface for visitors. It declares visit methods for each type of concrete element.

2. **Concrete Visitors:**

   - These classes implement the Visitor interface. They provide specific implementations for visiting `ConcreteElementA` and `ConcreteElementB`.

3. **Element Interface:**

   - This is an abstract class that declares the `accept` method, which takes a visitor as an argument.

4. **Concrete Elements:**

   - These classes implement the Element interface. They define the `accept` method to call the appropriate `visit` method on the visitor. They also have specific operations (`operationA` and `operationB`).

### Usage

```js
const elements = [new ConcreteElementA(), new ConcreteElementB()];

const visitor1 = new ConcreteVisitor1();
const visitor2 = new ConcreteVisitor2();

elements.forEach((element) => {
  element.accept(visitor1);
  element.accept(visitor2);
});
```

- This code creates instances of the concrete elements and visitors., then iterates over the elements, allowing each visitor to visit each element.

### Summary

The Visitor pattern allows you to define new operations on objects without changing the classes of the elements on which it operates. In this example, `ConcreteVisitor1` and `ConcreteVisitor2` can perform operations on `ConcreteElementA` and `ConcreteElementB` without modifying their classes. The `accept` method in each element class ensures that the correct visit method is called on the visitor. This pattern is useful for adding new operations to a class hierarchy without altering the existing code.

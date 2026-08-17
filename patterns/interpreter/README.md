# Interpreter Pattern

The Interpreter pattern is for situations where you need to evaluate expressions in a small domain-specific language. You define a grammar as a set of classes, then compose them to represent and interpret any valid expression.

### Example

```js
class Context {
  constructor(input = "") {
    this.input = input;
    this.output = 0;
  }
}

class Expression {
  interpret(context) {
    throw new Error("This method should be overridden!");
  }
}

class NumberExpression extends Expression {
  constructor(number) {
    super();
    this.number = number;
  }

  interpret(context) {
    context.output += this.number;
  }
}

class AddExpression extends Expression {
  constructor(left, right) {
    super();
    this.left = left;
    this.right = right;
  }

  interpret(context) {
    this.left.interpret(context);
    let leftResult = context.output;
    context.output = 0;
    this.right.interpret(context);
    let rightResult = context.output;
    context.output = leftResult + rightResult;
  }
}

class SubtractExpression extends Expression {
  constructor(left, right) {
    super();
    this.left = left;
    this.right = right;
  }

  interpret(context) {
    this.left.interpret(context);
    let leftResult = context.output;
    context.output = 0;
    this.right.interpret(context);
    let rightResult = context.output;
    context.output = leftResult - rightResult;
  }
}
```

### Explaining the code

**Classes and Their Roles**

1. **`Context` Class**

   - The `Context` class holds the input and output for the interpretation process. It initializes with an input and sets the output to 0.

2. **`Expression` Class**

   - The `Expression` class is an abstract class that defines the `interpret` method, which must be overridden by subclasses.

3. **`NumberExpression` Class**

   - The `NumberExpression` class represents a number in the language. It overrides the `interpret` method to add its number to the context's output.

4. **`AddExpression`** Class

   - The `AddExpression` class represents an addition operation. It interprets its left and right expressions, then adds their results.

5. **`SubtractExpression`** Class

   - The `SubtractExpression` class represents a subtraction operation. It interprets its left and right expressions, then subtracts the right result from the left result.

### Usage

```js
let context = new Context();

let expression = new AddExpression(
  new NumberExpression(5),
  new SubtractExpression(new NumberExpression(10), new NumberExpression(3))
);

expression.interpret(context);
console.log(context.output); // Output: 12
```

- A `Context` object is created.
- An `AddExpression` is created, which adds 5 to the result of subtracting 3 from 10.
- The `interpret` method is called on the `AddExpression`, which processes the expressions and updates the context's output.
- The final output is logged, which is 12.

### Summary

Each class represents one grammar rule, and you build expressions by composing them into a tree. When you call `interpret()`, it walks that tree recursively to produce a result. The pattern shines for things like math parsers, search query languages, or rule engines: anywhere you're evaluating structured expressions.

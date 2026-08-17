# State Pattern

The State pattern lets an object behave differently depending on its current state, without cluttering the object itself with conditionals. Each state gets its own class, and the object just delegates to whichever one is active.

### Example

```js
// State interface
class State {
  handle(context) {
    throw new Error("This method must be overridden!");
  }
}

// Concrete States
class ConcreteStateA extends State {
  handle(context) {
    console.log("State A handling request.");
    context.setState(new ConcreteStateB());
  }
}

class ConcreteStateB extends State {
  handle(context) {
    console.log("State B handling request.");
    context.setState(new ConcreteStateA());
  }
}

// Context
class Context {
  constructor(state) {
    this.setState(state);
  }

  setState(state) {
    console.log(`State changed to ${state.constructor.name}`);
    this.state = state;
  }

  request() {
    this.state.handle(this);
  }
}
```

### Explaining the code

1. **State Interface**

   - The `State` class defines an interface for encapsulating the behavior associated with a particular state of the `Context`.

2. **Concrete States**

   - `ConcreteStateA` and `ConcreteStateB` are concrete implementations of the `State` interface.
   - Each concrete state implements the `handle` method, which performs an action and transitions the context to the next state.

3. **Context**

   - The `Context` class maintains an instance of a `State` subclass that defines the current state.
   - The `setState` method changes the current state and logs the state transition.
   - The `request` method delegates the request to the current state's `handle` method.

### Usage

```js
const context = new Context(new ConcreteStateA());
context.request(); // State A handling request. State changed to ConcreteStateB
context.request(); // State B handling request. State changed to ConcreteStateA
```

- A `Context` object is created with an initial state (`ConcreteStateA`).
- Calling `request()` on the context triggers the current state's `handle` method, which performs an action and transitions to the next state.

### Summary

State transitions happen inside the state classes themselves: `ConcreteStateA.handle()` decides to transition to B, and vice versa. The `Context` never contains transition logic; it just calls `request()` and lets the active state figure out what happens next. This is what keeps `Context` clean when the number of states grows.

# Abstract Factory Pattern

The Abstract Factory pattern gives you a way to create families of related objects without locking your code to specific classes. Think of it as a factory for factories: swap the factory, and everything it produces changes with it.

### Example

```js
// Abstract Product
class Button {
  render() {
    throw new Error("This method should be overridden!");
  }
}
```

The `Button` class is an abstract product. It defines a method `render` that must be overridden by any concrete product that extends this class.

```js
// Concrete Product
class WindowsButton extends Button {
  render() {
    console.log("Rendering a button in Windows style.");
  }
}

class MacOSButton extends Button {
  render() {
    console.log("Rendering a button in MacOS style.");
  }
}
```

`WindowsButton` and `MacOSButton` are concrete products that extend the `Button` class. They provide specific implementations of the `render` method.

```js
// Abstract Factory
class GUIFactory {
  createButton() {
    throw new Error("This method should be overridden!");
  }
}
```

The `GUIFactory` class is an abstract factory. It defines a method `createButton` that must be overridden by any concrete factory that extends this class.

```js
// Concrete Factory
class WindowsFactory extends GUIFactory {
  createButton() {
    return new WindowsButton();
  }
}

class MacOSFactory extends GUIFactory {
  createButton() {
    return new MacOSButton();
  }
}
```

`WindowsFactory` and `MacOSFactory` are concrete factories that extend the `GUIFactory` class. They provide specific implementations of the `createButton` method, returning instances of `WindowsButton` and `MacOSButton`, respectively.

```js
// Client Code
function createUI(factory) {
  const button = factory.createButton();
  button.render();
}
```

The `createUI` function is the client code that uses a factory to create and render a button. It takes a factory as an argument, calls the `createButton` method on the factory to get a button, and then calls the `render` method on the button.

### Usage

```js
const windowsFactory = new WindowsFactory();
createUI(windowsFactory);

const macFactory = new MacOSFactory();
createUI(macFactory);
```

### Summary

`createUI` doesn't know or care whether it's building for Windows or macOS; it just asks the factory for a button and calls `render()`. Swap the factory, and the whole UI switches. That's the payoff: client code stays clean while the factory absorbs all the platform-specific details.

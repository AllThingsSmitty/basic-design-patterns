# Command Pattern

The Command pattern wraps a request in an object, which means you can pass it around, queue it, log it, or reverse it (things you can't do when you just call a method directly). The classic use case is undo/redo.

### Example

```js
// Command Interface
class Command {
  execute() {}
  undo() {}
}

// Concrete Command
class LightOnCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.on();
  }

  undo() {
    this.light.off();
  }
}

class LightOffCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.off();
  }

  undo() {
    this.light.on();
  }
}

// Receiver
class Light {
  on() {
    console.log("The light is on");
  }

  off() {
    console.log("The light is off");
  }
}

// Invoker
class RemoteControl {
  setCommand(command) {
    this.command = command;
  }

  pressButton() {
    this.command.execute();
  }

  pressUndo() {
    this.command.undo();
  }
}
```

### Explaining the code

1. **Command Interface:**

   - This is an abstract class that defines the `execute` and `undo` methods. Concrete command classes will implement these methods.

2. **Concrete Commands:**

   - `LightOnCommand`: This command turns the light on by calling the `on` method of the `Light` class and can undo this action by calling the `off` method.
   - `LightOffCommand`: This command turns the light off by calling the `off` method of the `Light` class and can undo this action by calling the `on` method.

3. **Receiver:**

   - The `Light` class is the receiver that performs the actual operations (`on` and `off`).

4. **Invoker:**
   - The `RemoteControl` class acts as the invoker. It holds a command and can execute or undo it via the `pressButton` and `pressUndo` methods.

### Usage

```js
const light = new Light();
const lightOn = new LightOnCommand(light);
const lightOff = new LightOffCommand(light);

const remote = new RemoteControl();

remote.setCommand(lightOn);
remote.pressButton(); // The light is on
remote.pressUndo(); // The light is off

remote.setCommand(lightOff);
remote.pressButton(); // The light is off
remote.pressUndo(); // The light is on
```

The client code creates instances of the `Light`, `LightOnCommand`, and `LightOffCommand` classes. It then uses the `RemoteControl` to execute and undo commands.

### Summary

`RemoteControl` has no idea what a `Light` is; it just holds a command and fires it. Swap the command and the remote works with anything. This loose coupling is what makes the pattern so useful for toolbars, macro systems, or any UI where actions need to be reversible or re-playable.

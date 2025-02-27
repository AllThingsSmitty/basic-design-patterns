# Mediator Pattern

The Mediator pattern is used to reduce the complexity of communication between multiple objects or classes. Instead of having objects communicate directly with each other, they communicate through a mediator object.

```js
class Mediator {
  constructor() {
    this.colleagues = [];
  }

  register(colleague) {
    this.colleagues.push(colleague);
    colleague.setMediator(this);
  }

  send(message, sender) {
    this.colleagues.forEach((colleague) => {
      if (colleague !== sender) {
        colleague.receive(message);
      }
    });
  }
}

class Colleague {
  constructor(name) {
    this.name = name;
    this.mediator = null;
  }

  setMediator(mediator) {
    this.mediator = mediator;
  }

  send(message) {
    console.log(`${this.name} sends: ${message}`);
    this.mediator.send(message, this);
  }

  receive(message) {
    console.log(`${this.name} receives: ${message}`);
  }
}
```

### Explaining the code

1. **Mediator Class:**

   - **Constructor:** Initializes an empty array `colleagues` to keep track of registered colleagues.
   - **`register(colleague)`:** Adds a colleague to the `colleagues` array and sets the mediator for that colleague.
   - **`send(message, sender)`:** Sends a message to all colleagues except the sender. It iterates over the `colleagues` array and calls the `receive` method on each colleague that is not the sender.

2. **Colleague Class:**

   - **Constructor(name):** Initializes the colleague with a `name` and sets the `mediator` to `null`.
   - **`setMediator(mediator)`:** Sets the mediator for the colleague.
   - **`send(message)`:** Sends a message through the mediator. It logs the message being sent and calls the `send` method on the mediator.
   - **`receive(message)`:** Logs the message received by the colleague.

### Usage

```js
const mediator = new Mediator();

const colleague1 = new Colleague("Colleague1");
const colleague2 = new Colleague("Colleague2");
const colleague3 = new Colleague("Colleague3");

mediator.register(colleague1);
mediator.register(colleague2);
mediator.register(colleague3);

colleague1.send("Hello, World!");
colleague2.send("Hi there!");
colleague3.send("Good day!");
```

- An instance of the `Mediator` class is created.
- Three instances of the `Colleague` class are created with names "Colleague1", "Colleague2", and "Colleague3".
- Each colleague is registered with the mediator using the `register` method.
- Each colleague sends a message using the `send` method, which is then relayed to the other colleagues by the mediator.

### Summary

The Mediator design pattern helps manage communication between multiple objects by centralizing the communication logic in a mediator object. This reduces the dependencies between the objects, making the system easier to maintain and extend. In this example, the `Mediator` class handles the communication between instances of the `Colleague` class, ensuring that messages are sent to all colleagues except the sender.

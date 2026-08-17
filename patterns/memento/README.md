# Memento Pattern

The Memento pattern lets an object save and restore its own state without exposing that state to the outside world. It's the backbone of undo/redo: the object stays in control of what gets snapshotted and how it's restored.

### Example

```js
class Memento {
  constructor(state) {
    this.state = state;
  }

  getState() {
    return this.state;
  }
}

// Originator
class Originator {
  constructor() {
    this.state = "";
  }

  setState(state) {
    this.state = state;
    console.log(`State set to: ${this.state}`);
  }

  saveStateToMemento() {
    return new Memento(this.state);
  }

  getStateFromMemento(memento) {
    this.state = memento.getState();
    console.log(`State restored to: ${this.state}`);
  }
}

// Caretaker
class Caretaker {
  constructor() {
    this.mementoList = [];
  }

  add(memento) {
    this.mementoList.push(memento);
  }

  get(index) {
    return this.mementoList[index];
  }
}
```

### Explaining the code

**Classes and Methods**

1.  **Memento**

- **Purpose:** Stores the state of the `Originator`.
- **Constructor:** Takes a `state` parameter and assigns it to the instance.
- **`getState()`:** Returns the stored state.

2. **Originator**

   - **Purpose:** Creates and stores states in `Memento` objects.
   - **Constructor:** Initializes the `state` to an empty string.
   - **`setState(state)`:** Sets the current state and logs it.
   - **`saveStateToMemento()`:** Creates a new `Memento` with the current state.
   - **`getStateFromMemento(memento)`:** Restores the state from a given `Memento` and logs it.

3. **Caretaker**

   - **Purpose:** Manages the `Memento` objects.
   - **Constructor:** Initializes an empty list to store `Memento` objects.
   - **add(memento):** Adds a `Memento` to the list.
   - **get(index):** Retrieves a `Memento` from the list by index.

### Usage

```js
const originator = new Originator();
const caretaker = new Caretaker();

originator.setState("State #1");
originator.setState("State #2");
caretaker.add(originator.saveStateToMemento());

originator.setState("State #3");
caretaker.add(originator.saveStateToMemento());

originator.setState("State #4");

console.log("Current State:", originator.state);
originator.getStateFromMemento(caretaker.get(0));
console.log("First saved State:", originator.state);
originator.getStateFromMemento(caretaker.get(1));
console.log("Second saved State:", originator.state);
```

- Create instances of `Originator` and `Caretaker`.
- Set various states in the `Originator`.
- Save states to `Memento` objects and store them in the `Caretaker`.
- Restore states from `Memento` objects using the `Caretaker`.

### Summary

The three-role split keeps things clean: `Originator` knows its own state, `Memento` holds a snapshot of it, and `Caretaker` manages the list of snapshots. The caretaker never peeks inside a memento; it just stores and retrieves them. That's what makes this encapsulation-friendly compared to other undo approaches.

# Flyweight Pattern

The Flyweight pattern reduces memory by sharing data that's common across many objects. Instead of each object storing its own copy of the same values, they all point to a single shared instance.

### Example

```js
class Flyweight {
  constructor(make, model, processor) {
    this.make = make;
    this.model = model;
    this.processor = processor;
  }
}

class FlyweightFactory {
  constructor() {
    this.flyweights = {};
  }

  get(make, model, processor) {
    const key = `${make}-${model}-${processor}`;
    if (!this.flyweights[key]) {
      this.flyweights[key] = new Flyweight(make, model, processor);
    }
    return this.flyweights[key];
  }

  getCount() {
    return Object.keys(this.flyweights).length;
  }
}

class Computer {
  constructor(make, model, processor, memory, tag, factory) {
    this.flyweight = factory.get(make, model, processor);
    this.memory = memory;
    this.tag = tag;
  }
}

class ComputerCollection {
  constructor(factory) {
    this.computers = {};
    this.count = 0;
    this.factory = factory;
  }

  add(make, model, processor, memory, tag) {
    this.computers[tag] = new Computer(make, model, processor, memory, tag, this.factory);
    this.count++;
  }

  get(tag) {
    return this.computers[tag];
  }

  getCount() {
    return this.count;
  }
}
```

### Explaining the code

1. **`Flyweight`:**

   - Represents the shared part of the object. In this case, it includes`make`, `model`, and `processor`.
   - Constructor initializes these properties.

2. **`FlyweightFactory`:**

   - Manages the flyweight objects.
   - Contains a method `get` to retrieve an existing flyweight or create a new one if it doesn't exist.
   - `getCount` method returns the number of unique flyweights created.

3. **`Computer`:**

   - Represents the full object, which includes both shared (flyweight) and unique parts.
   - Constructor receives the `factory` instance and calls `factory.get()` to retrieve or create the shared flyweight. Unique properties like `memory` and `tag` are stored separately.

4. **`ComputerCollection`:**

   - Manages a collection of `Computer` objects.
   - `add` method adds a new computer to the collection.
   - `get` method retrieves a computer by its tag.
   - `getCount` method returns the number of computers in the collection.

### Usage

```js
const factory = new FlyweightFactory();
const computers = new ComputerCollection(factory);

computers.add("Dell", "XPS", "Intel", "16GB", "1");
computers.add("Dell", "XPS", "Intel", "8GB", "2");
computers.add("HP", "Envy", "AMD", "16GB", "3");
computers.add("HP", "Envy", "AMD", "8GB", "4");

console.log(`Computers: ${computers.getCount()}`); // 4
console.log(`Flyweights: ${factory.getCount()}`);  // 2 (Dell-XPS-Intel, HP-Envy-AMD)
```

- A `FlyweightFactory` instance is created and passed into `ComputerCollection`, which threads it through to each `Computer`.
- Four computers are added: two Dell XPS (Intel) and two HP Envy (AMD), each with different memory configs.
- Even though there are 4 computers, only 2 flyweights are created: the shared make/model/processor data is stored once and reused.

### Summary

With 4 computers but only 2 flyweights, you can see the pattern working: make, model, and processor are stored once per unique combination, while memory and tag (the parts that actually differ) stay on each individual `Computer`. This scales well when you have thousands of objects with lots of shared state.

# Prototype Pattern

The Prototype pattern creates new objects by cloning an existing one rather than constructing from scratch. This is handy when object creation is expensive or when you want new instances to start life as a copy of a known-good object.

### Example

```js
// Define a constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Shared method on the prototype
Person.prototype.greet = function () {
  console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
};

// Clone method: the key piece of the Prototype pattern
Person.prototype.clone = function () {
  return Object.assign(Object.create(Person.prototype), this);
};

// Create an original instance
const person1 = new Person("Alice", 30);
```

### Explaining the code

1. **Constructor Function:**

   - The `Person` function initializes new objects with `name` and `age` properties.

2. **Prototype Method:**

   - `greet` is added to `Person.prototype` so all instances share one copy of the method rather than each carrying their own. This is JavaScript's native approach to shared behavior.

3. **Clone Method:**

   - `clone` creates a shallow copy of the instance using `Object.assign` into a new object that inherits from `Person.prototype`. This is what actually makes it the Prototype pattern: new objects come from cloning, not from calling a constructor with fresh data.

### Usage

```js
person1.greet(); // Hello, my name is Alice and I am 30 years old.

const person2 = person1.clone();
person2.name = "Bob";
person2.age = 25;

person1.greet(); // Hello, my name is Alice and I am 30 years old. (unchanged)
person2.greet(); // Hello, my name is Bob and I am 25 years old.
```

- `person2` starts as an exact copy of `person1`, then gets its own values. The original is untouched.

### Summary

Use the Prototype pattern when spinning up a fresh object from a constructor is too heavy; cloning a pre-configured instance is faster and keeps the setup logic in one place. It also works well when you need variations of a base object without subclassing.

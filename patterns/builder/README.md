# Builder Pattern

The Builder pattern separates the construction of a complex object from its representation. This allows the same construction process to create different representations of the object.

## When to Use

✅ **Use Builder for:**

- Objects with many optional parameters
- Objects that require step-by-step construction
- Creating different variations of the same object
- Configuration objects with fluent interfaces
- Complex initialization logic

❌ **Avoid Builder for:**

- Simple objects with few required parameters
- When you'd rather use default arguments
- Objects that don't need customization

## Example

```js
// The Car class represents a car with various properties such as make, model, year, color, and engine. It uses a builder object to initialize these properties.
class Car {
  constructor(builder) {
    this.make = builder.make;
    this.model = builder.model;
    this.year = builder.year;
    this.color = builder.color;
    this.engine = builder.engine;
  }

  // Returns a string representation of the car
  toString() {
    return `${this.year} ${this.make} ${this.model} in ${this.color} with a ${this.engine} engine`;
  }
}
```

The `CarBuilder` class helps in building a `Car` object step by step. It allows setting various properties of the car and then building the final `Car` object.

```js
// CarBuilder class helps in building a Car object step by step
class CarBuilder {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }

  // Sets the year of the car
  setYear(year) {
    this.year = year;
    return this;
  }

  // Sets the color of the car
  setColor(color) {
    this.color = color;
    return this;
  }

  // Sets the engine type of the car
  setEngine(engine) {
    this.engine = engine;
    return this;
  }

  // Builds and returns a Car object
  build() {
    return new Car(this);
  }
}
```

## Explaining the code

1. **Fluent Interface:** The `CarBuilder` class uses a fluent interface, allowing method chaining. Each setter method returns the builder object itself (`this`), enabling chaining multiple method calls in a single statement.
2. **Separation of Concerns:** The builder pattern separates the construction of a complex object (`Car`) from its representation, allowing the same construction process to create various representations.
3. **Independent snapshots:** The `Car` constructor copies values from the builder, so later builder changes do not change an already-built car. The sample does not make the car's properties immutable; callers can still assign to them.

## Usage

```js
const car = new CarBuilder("Toyota", "Camry")
  .setYear(2021)
  .setColor("Red")
  .setEngine("V6")
  .build();

// Output the car details
console.log(car.toString());
// Output: 2021 Toyota Camry in Red with a V6 engine
```

## Real-World Example

```js
class UserProfileBuilder {
  constructor(username) {
    this.username = username;
    this.email = "";
    this.avatar = "default.jpg";
    this.bio = "";
    this.verified = false;
  }

  setEmail(email) {
    this.email = email;
    return this;
  }

  setAvatar(avatar) {
    this.avatar = avatar;
    return this;
  }

  setBio(bio) {
    this.bio = bio;
    return this;
  }

  setVerified(verified) {
    this.verified = verified;
    return this;
  }

  build() {
    return {
      username: this.username,
      email: this.email,
      avatar: this.avatar,
      bio: this.bio,
      verified: this.verified,
      createdAt: new Date(),
    };
  }
}

// Usage
const userProfile = new UserProfileBuilder("alice")
  .setEmail("alice@example.com")
  .setAvatar("alice.jpg")
  .setBio("Software Developer")
  .setVerified(true)
  .build();

console.log(userProfile);
// {
//   username: "alice",
//   email: "alice@example.com",
//   avatar: "alice.jpg",
//   bio: "Software Developer",
//   verified: true,
//   createdAt: 2024-02-05T...
// }
```

## Common Mistakes

❌ **Problem:** Builder doesn't validate required fields.

```js
const car = new CarBuilder("Toyota", "Camry").setYear(2021).build(); // No engine or color specified, incomplete car

console.log(car.toString()); // "2021 Toyota Camry in undefined with a undefined engine"
```

✅ **Solution:** Add validation in the build() method:

```js
build() {
  if (!this.year) {
    throw new Error("Year is required");
  }
  if (!this.color) {
    throw new Error("Color is required");
  }
  if (!this.engine) {
    throw new Error("Engine is required");
  }
  return new Car(this);
}
```

❌ **Problem:** Reusing a mutable builder changes the values used by later builds.

```js
const builder = new CarBuilder("Toyota", "Camry").setColor("Red").setEngine("V6");

const car1 = builder.setYear(2021).build();
const car2 = builder.setYear(2022).build(); // car1.year remains 2021; car2.year is 2022
```

✅ **Solution:** Start a new builder when you need an independent construction sequence, or use an immutable builder API. The builder above intentionally mutates itself; its `Car` constructor copies the values when `build()` is called.

## Summary

In this example, the `Car` class is constructed using the `CarBuilder` class. The `CarBuilder` class provides methods to set optional parameters and a `build()` method to create the `Car` object. The Builder pattern is useful for constructing complex objects with many optional parameters. It makes code more readable, maintainable, and flexible when dealing with objects that have many configuration options.

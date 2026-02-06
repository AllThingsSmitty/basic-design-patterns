# Factory Method Pattern

The Factory Method pattern lets a class defer instantiation to subclasses. It defines an interface for creating a single object, but lets subclasses decide which class to instantiate.

## When to Use

✅ **Use Factory Method for:**

- Creating objects of different types based on conditions
- Hiding object creation logic from clients
- Reducing coupling between classes
- Centralizing object creation
- When many similar objects need custom initialization

❌ **Avoid Factory Method for:**

- Simple objects with just one type (use direct constructor)
- Cases where a simple config object would work
- When you don't have multiple types to create

## Example

```js
// Class for Car
class Car {
  constructor({ doors = 4, state = "brand new", color = "silver" } = {}) {
    this.doors = doors;
    this.state = state;
    this.color = color;
  }

  describe() {
    return `${this.state} car with ${this.doors} doors in ${this.color}`;
  }
}
```

The `Car` class initializes a car object with properties `doors`, `state`, and `color`. Default values are provided using destructuring and default parameters.

```js
// Class for Truck
class Truck {
  constructor({ doors = 2, state = "used", color = "blue" } = {}) {
    this.doors = doors;
    this.state = state;
    this.color = color;
  }

  describe() {
    return `${this.state} truck with ${this.doors} doors in ${this.color}`;
  }
}
```

Similar to the `Car` class, the `Truck` class initializes a truck object with properties `doors`, `state`, and `color`. Default values are also provided using destructuring and default parameters.

```js
// Factory class
class VehicleFactory {
  // Method to create a vehicle based on the provided options
  createVehicle(options) {
    switch (options.vehicleType) {
      // If vehicleType is "car", create a new Car
      case "car":
        return new Car(options);
      // If vehicleType is "truck", create a new Truck
      case "truck":
        return new Truck(options);
      // If vehicleType is not recognized, return null
      default:
        throw new Error(`Unknown vehicle type: ${options.vehicleType}`);
    }
  }
}
```

## Explaining the code

- The `createVehicle` method is added to the `VehicleFactory` class.
- It takes an options object as an argument and uses a switch statement to determine which type of vehicle to create based on the `vehicleType` property.
- If `vehicleType` is "car", it creates and returns a new `Car` object.
- If `vehicleType` is "truck", it creates and returns a new `Truck` object.
- If `vehicleType` is not recognized, it now throws an error (better than returning null).

## Usage

```js
const factory = new VehicleFactory();

// Create a car with specified options
const car = factory.createVehicle({
  vehicleType: "car",
  doors: 4,
  color: "red",
  state: "new",
});

// Create a truck with specified options
const truck = factory.createVehicle({
  vehicleType: "truck",
  doors: 2,
  color: "black",
  state: "used",
});

console.log(car.describe()); // new car with 4 doors in red
console.log(truck.describe()); // used truck with 2 doors in black
```

## Real-World Example

```js
class UserFactory {
  createUser(role) {
    switch (role) {
      case "admin":
        return { name: "Admin User", permissions: ["read", "write", "delete"] };
      case "editor":
        return { name: "Editor User", permissions: ["read", "write"] };
      case "viewer":
        return { name: "Viewer User", permissions: ["read"] };
      default:
        throw new Error(`Unknown role: ${role}`);
    }
  }
}

// Usage
const factory = new UserFactory();
const admin = factory.createUser("admin");
const viewer = factory.createUser("viewer");

console.log(admin.permissions); // ["read", "write", "delete"]
console.log(viewer.permissions); // ["read"]
```

## Common Mistakes

❌ **Problem:** Returning null for unknown types causes runtime errors downstream.

```js
createVehicle(options) {
  switch (options.vehicleType) {
    case "car":
      return new Car(options);
    default:
      return null; // Dangerous! Caller might call methods on null
  }
}

// Later:
const vehicle = factory.createVehicle({ vehicleType: "bike" });
vehicle.describe(); // Crash: Cannot read property of null
```

✅ **Solution:** Throw an error to fail fast and clearly:

```js
createVehicle(options) {
  switch (options.vehicleType) {
    case "car":
      return new Car(options);
    default:
      throw new Error(`Unknown vehicle type: ${options.vehicleType}`);
  }
}
```

❌ **Problem:** Factory becomes bloated with too many types.

```js
class VehicleFactory {
  createVehicle(options) {
    switch (options.vehicleType) {
      case "car":
        return new Car(options);
      case "truck":
        return new Truck(options);
      case "bike":
        return new Bike(options);
      case "plane":
        return new Plane(options);
      case "boat":
        return new Boat(options);
      // ... 20 more cases
    }
  }
}
```

✅ **Solution:** Use a registry pattern for better scalability:

```js
class ScalableVehicleFactory {
  constructor() {
    this.vehicles = new Map();
    this.vehicles.set("car", Car);
    this.vehicles.set("truck", Truck);
    this.vehicles.set("bike", Bike);
  }

  registerVehicle(type, VehicleClass) {
    this.vehicles.set(type, VehicleClass);
  }

  createVehicle(options) {
    const VehicleClass = this.vehicles.get(options.vehicleType);
    if (!VehicleClass) {
      throw new Error(`Unknown vehicle type: ${options.vehicleType}`);
    }
    return new VehicleClass(options);
  }
}
```

## Summary

This pattern allows the instantiation logic to be deferred to subclasses, promoting flexibility and scalability in object creation. The Factory Method helps reduce coupling between classes and centralizes object creation logic, making code more maintainable and easier to extend when new types are needed.

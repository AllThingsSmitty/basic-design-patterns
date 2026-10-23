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
// Creator class: subclasses decide which product to construct
class VehicleFactory {
  createVehicle() {
    throw new Error("Subclasses must implement createVehicle()");
  }
}

class CarFactory extends VehicleFactory {
  createVehicle(options) {
    return new Car(options);
  }
}

class TruckFactory extends VehicleFactory {
  createVehicle(options) {
    return new Truck(options);
  }
}
```

## Explaining the code

- `VehicleFactory` declares the factory method `createVehicle`.
- `CarFactory` and `TruckFactory` override that method to construct their respective products.
- Client code uses a concrete factory without deciding product construction through a type switch.

## Usage

```js
// Each concrete factory chooses the product type
const car = new CarFactory().createVehicle({
  doors: 4,
  color: "red",
  state: "new",
});

const truck = new TruckFactory().createVehicle({
  doors: 2,
  color: "black",
  state: "used",
});

console.log(car.describe()); // new car with 4 doors in red
console.log(truck.describe()); // used truck with 2 doors in black
```

## Related Example: Simple Factory

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

## Common Simple Factory Mistakes

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

The Factory Method defers product construction to subclasses, reducing coupling between client code and concrete products. The switch-based examples above this summary are simple factories; unlike Factory Method, they choose products in one factory method based on a type value.

# Singleton Pattern

The Singleton pattern ensures that a class has only one instance and provides a global point of access to it. This is useful when exactly one object is needed to coordinate actions across the system.

## When to Use

✅ **Use Singleton for:**

- Database connections or connection pools
- Application configuration/settings
- Logging systems
- Cache managers
- Theme/UI state managers

❌ **Avoid Singleton for:**

- Stateful objects in tests (hard to reset between tests)
- Data that varies per user/session
- Objects you need multiple instances of in the future
- When dependency injection would work better

## Example

```js
class Singleton {
  // Constructor function for the Singleton class
  constructor() {
    // Check if an instance already exists
    if (!Singleton.instance) {
      // If no instance exists, assign the current instance to Singleton.instance
      Singleton.instance = this;
    }
    // Return the single instance
    return Singleton.instance;
  }

  // Example method for the Singleton class
  someMethod() {
    console.log("Singleton method called");
  }
}
```

## Explaining the code

**Class Declaration:** The `Singleton` class is declared.

**Constructor:** The constructor checks if an instance of the class already exists.

- `if (!Singleton.instance)`: This condition checks if `Singleton.instance` is `undefined` or `null`.
- `Singleton.instance = this`: If no instance exists, the current instance (`this`) is assigned to `Singleton.instance`.
- `return Singleton.instance`: The constructor returns the single instance of the class.

**Method:** The `someMethod` is a simple method that logs a message to the console.

## Usage

```js
// Create the first instance of the Singleton class
const instance1 = new Singleton();
// Attempt to create a second instance of the Singleton class
const instance2 = new Singleton();

// Check if both instances are the same
console.log(instance1 === instance2); // true

// Call the method on the first instance
instance1.someMethod(); // Singleton method called
```

**Instance Creation:**

- `const instance1 = new Singleton()`: Creates the first instance of the `Singleton` class.
- `const instance2 = new Singleton()`: Attempts to create a second instance, but due to the `Singleton` pattern, it returns the same instance as `instance1`.

**Instance Comparison:**

- `console.log(instance1 === instance2)`: This logs true because both `instance1` and `instance2` refer to the same instance.

**Method Call:**

- `instance1.someMethod()`: Calls the `someMethod` on the singleton instance, logging "Singleton method called" to the console.

## Real-World Example

```js
class Logger {
  constructor() {
    if (!Logger.instance) {
      this.logs = [];
      Logger.instance = this;
    }
    return Logger.instance;
  }

  log(message) {
    const timestamp = new Date().toISOString();
    this.logs.push(`[${timestamp}] ${message}`);
    console.log(`[${timestamp}] ${message}`);
  }

  getLogs() {
    return this.logs;
  }
}

// Usage
const logger1 = new Logger();
const logger2 = new Logger();

logger1.log("Application started");
logger2.log("User logged in");

console.log(logger1 === logger2); // true (same instance)
console.log(logger1.getLogs().length); // 2 (both logged to same instance)
```

## Common Mistakes

❌ **Problem:** Singleton can be hard to test because you can't easily reset state between tests.

```js
// Tests fail because state persists
it("should start with empty logs", () => {
  const logger = new Logger();
  expect(logger.getLogs()).toHaveLength(0); // Fails if previous test added logs
});
```

✅ **Solution:** Add a method to reset state during testing:

```js
class Logger {
  // ... existing code ...

  reset() {
    this.logs = [];
  }
}

// In tests:
beforeEach(() => {
  logger.reset();
});
```

❌ **Problem:** The basic pattern above can still be bypassed:

```js
// Someone could still do this:
Logger.instance = null;
const fakeLogger = new Logger(); // Creates new instance, bypassing singleton
```

✅ **Solution:** Use a more robust pattern (optional, for production code):

```js
class RobustLogger {
  constructor() {
    throw new Error("Use RobustLogger.getInstance() instead");
  }

  static getInstance() {
    if (!RobustLogger.instance) {
      RobustLogger.instance = Object.create(RobustLogger.prototype);
      RobustLogger.instance.logs = [];
    }
    return RobustLogger.instance;
  }

  log(message) {
    const timestamp = new Date().toISOString();
    this.logs.push(`[${timestamp}] ${message}`);
  }
}

// Usage (prevents direct instantiation)
const logger = RobustLogger.getInstance();
```

## Summary

The Singleton pattern ensures that a class has only one instance and provides a global point of access to that instance. This is particularly beneficial in scenarios where a single shared resource is needed throughout an application. By controlling the instantiation process, the Singleton pattern helps manage resources efficiently and ensures consistent behavior across the application. However, be mindful of testing implications and consider using dependency injection for better testability in large applications.

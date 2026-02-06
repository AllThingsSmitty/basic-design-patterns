# Decorator Pattern

The Decorator pattern allows behavior to be added to individual objects, dynamically, without affecting the behavior of other objects from the same class.

## When to Use

✅ **Use Decorator for:**

- Adding features to objects without subclassing
- Combining behaviors (logging, authentication, caching)
- Creating variations of objects dynamically
- Wrapping objects with cross-cutting concerns
- More flexible than inheritance

❌ **Avoid Decorator for:**

- Simple inheritance scenarios
- Performance-critical code (extra wrapper objects)
- When only a few variations exist

## Example

```js
// Base component
class Coffee {
  cost() {
    return 5;
  }

  getDescription() {
    return "Coffee";
  }
}

// Decorator base class
class CoffeeDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }

  cost() {
    return this.coffee.cost();
  }

  getDescription() {
    return this.coffee.getDescription();
  }
}

// Concrete Decorators
class MilkDecorator extends CoffeeDecorator {
  cost() {
    return this.coffee.cost() + 1;
  }

  getDescription() {
    return this.coffee.getDescription() + ", Milk";
  }
}

class SugarDecorator extends CoffeeDecorator {
  cost() {
    return this.coffee.cost() + 0.5;
  }

  getDescription() {
    return this.coffee.getDescription() + ", Sugar";
  }
}

class VanillaDecorator extends CoffeeDecorator {
  cost() {
    return this.coffee.cost() + 0.75;
  }

  getDescription() {
    return this.coffee.getDescription() + ", Vanilla";
  }
}
```

## Explaining the code

1. **Base Component:** The `Coffee` class represents the base component with a `cost` method that returns the base price of a coffee, which is 5.
2. **Decorator Base Class:** The `CoffeeDecorator` class is a base decorator class that takes a `coffee` object and delegates the cost method to it. This class is designed to be extended by concrete decorators.
3. **Concrete Decorators:**
   - **`MilkDecorator`**
     - The `MilkDecorator` class extends `CoffeeDecorator` and adds the cost of milk (1) to the base coffee cost.
     - Also updates the description to show milk was added.
   - **`SugarDecorator`**
     - The `SugarDecorator` class extends `CoffeeDecorator` and adds the cost of sugar (0.5) to the base coffee cost.
     - Also updates the description to show sugar was added.
   - **`VanillaDecorator`**
     - The `VanillaDecorator` class extends `CoffeeDecorator` and adds the cost of vanilla (0.75) to the base coffee cost.

## Usage

```js
let myCoffee = new Coffee();
console.log(`${myCoffee.getDescription()} - $${myCoffee.cost()}`); // Coffee - $5

myCoffee = new MilkDecorator(myCoffee);
console.log(`${myCoffee.getDescription()} - $${myCoffee.cost()}`); // Coffee, Milk - $6

myCoffee = new SugarDecorator(myCoffee);
console.log(`${myCoffee.getDescription()} - $${myCoffee.cost()}`); // Coffee, Milk, Sugar - $6.5

myCoffee = new VanillaDecorator(myCoffee);
console.log(`${myCoffee.getDescription()} - $${myCoffee.cost()}`); // Coffee, Milk, Sugar, Vanilla - $7.25
```

- Initially, `myCoffee` is a plain `Coffee` object with a cost of 5.
- Then, `myCoffee` is wrapped with a `MilkDecorator`, increasing the cost to 6 and adding "Milk" to description.
- Next, `myCoffee` is wrapped with a `SugarDecorator`, increasing the cost to 6.5 and adding "Sugar" to description.
- Finally, `myCoffee` is wrapped with a `VanillaDecorator`, increasing the cost to 7.25 and adding "Vanilla" to description.

## Real-World Example

```js
// Real-world: wrapping functions with logging and error handling
class DataFetcher {
  fetchData(url) {
    console.log(`Fetching from ${url}`);
    return { data: "sample data" };
  }
}

class LoggingDecorator {
  constructor(fetcher) {
    this.fetcher = fetcher;
  }

  fetchData(url) {
    console.log(`[LOG] Starting fetch at ${new Date().toISOString()}`);
    const result = this.fetcher.fetchData(url);
    console.log(`[LOG] Fetch completed at ${new Date().toISOString()}`);
    return result;
  }
}

class CachingDecorator {
  constructor(fetcher) {
    this.fetcher = fetcher;
    this.cache = new Map();
  }

  fetchData(url) {
    if (this.cache.has(url)) {
      console.log(`[CACHE] Returning cached data for ${url}`);
      return this.cache.get(url);
    }

    const result = this.fetcher.fetchData(url);
    this.cache.set(url, result);
    return result;
  }
}

// Usage: compose decorators
let fetcher = new DataFetcher();
fetcher = new LoggingDecorator(fetcher);
fetcher = new CachingDecorator(fetcher);

fetcher.fetchData("https://api.example.com/data");
// [LOG] Starting fetch...
// Fetching from https://api.example.com/data
// [LOG] Fetch completed...

fetcher.fetchData("https://api.example.com/data");
// [CACHE] Returning cached data for https://api.example.com/data
```

## Common Mistakes

❌ **Problem:** Decorators only wrap behavior, not replacing entire implementation.

```js
class Coffee {
  cost() {
    return 5;
  }
}

class MilkDecorator extends CoffeeDecorator {
  cost() {
    return this.coffee.cost() + 1; // Always delegates, can't override completely
  }
}
```

✅ **Solution:** This is actually correct behavior for Decorator pattern. If you need to replace entire implementation, use Strategy or Inheritance instead.

❌ **Problem:** Decorators don't inherit all public methods of wrapped object.

```js
class Coffee {
  cost() {
    return 5;
  }
  getDescription() {
    return "Coffee";
  }
  brew() {
    console.log("Brewing...");
  }
}

class MilkDecorator extends CoffeeDecorator {
  // Only decorates cost(), but getDescription() and brew() are also needed!
}

const coffee = new MilkDecorator(new Coffee());
coffee.brew(); // Works because delegated in decorator
```

✅ **Solution:** Implement all methods in decorator or use a Proxy for transparent wrapping:

```js
function decorateWithLogging(obj) {
  return new Proxy(obj, {
    get(target, prop) {
      console.log(`Accessed ${String(prop)}`);
      return target[prop];
    },
  });
}
```

❌ **Problem:** Memory overhead from many nested decorators.

```js
let coffee = new Coffee();
for (let i = 0; i < 100; i++) {
  coffee = new MilkDecorator(coffee); // Creates 100 wrapper objects!
}
```

✅ **Solution:** Be mindful of decorator depth in production code.

## Summary

The Decorator pattern is used to dynamically add additional features to objects without modifying their original structure. This pattern is useful for extending the functionality of objects in a flexible and reusable way. Decorators can be stacked and composed in any order, providing great flexibility. The pattern follows the Open/Closed Principle: objects are open for extension (adding decorators) but closed for modification (no changes to original class).

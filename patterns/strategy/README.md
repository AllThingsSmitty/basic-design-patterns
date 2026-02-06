# Strategy Pattern

The Strategy pattern allows you to define a family of algorithms, encapsulate each one as a class, and make them interchangeable. This pattern lets the algorithm vary independently from the clients that use it.

## When to Use

✅ **Use Strategy for:**

- Different algorithms for the same task
- Avoiding large if/else chains
- Runtime algorithm selection
- Payment methods, sorting algorithms, shipping costs
- Reducing coupling between components

❌ **Avoid Strategy for:**

- Single algorithm (unnecessary complexity)
- Simple boolean conditionals
- When inheritance would be simpler

## Example

```js
// Strategy interface (base class)
class Strategy {
  execute(a, b) {
    throw new Error("This method should be overridden!");
  }
}

// Concrete strategies
class AddStrategy extends Strategy {
  execute(a, b) {
    return a + b;
  }
}

class SubtractStrategy extends Strategy {
  execute(a, b) {
    return a - b;
  }
}

class MultiplyStrategy extends Strategy {
  execute(a, b) {
    return a * b;
  }

class DivideStrategy extends Strategy {
  execute(a, b) {
    if (b === 0) {
      throw new Error("Division by zero!");
    }
    return a / b;
  }
}

// Context
class Calculator {
  constructor(strategy) {
    this.strategy = strategy;
  }

  setStrategy(strategy) {
    this.strategy = strategy;
  }

  executeStrategy(a, b) {
    return this.strategy.execute(a, b);
  }
}
```

## Explaining the code

1. **Strategy Interface:** The `Strategy` class defines an interface with an `execute` method that must be overridden by concrete strategy classes.
2. **Concrete Strategies:** These classes extend the `Strategy` class and implement the `execute` method with specific algorithms.
3. **Context:** The `Calculator` class acts as the context that uses a Strategy object.
   - **`constructor(strategy)`:** Initializes the calculator with a specific strategy.
   - **`setStrategy(strategy)`:** Allows changing the strategy at runtime.
   - **`executeStrategy(a, b)`:** Executes the current strategy's algorithm.

## Usage

```js
const calculator = new Calculator(new AddStrategy());
console.log(calculator.executeStrategy(5, 3)); // Output: 8

calculator.setStrategy(new SubtractStrategy());
console.log(calculator.executeStrategy(5, 3)); // Output: 2

calculator.setStrategy(new MultiplyStrategy());
console.log(calculator.executeStrategy(5, 3)); // Output: 15

calculator.setStrategy(new DivideStrategy());
console.log(calculator.executeStrategy(6, 3)); // Output: 2
```

## Real-World Example

```js
// Different payment strategies
class PaymentStrategy {
  pay(amount) {
    throw new Error("Pay method not implemented");
  }
}

class CreditCardPayment extends PaymentStrategy {
  constructor(cardNumber, cvv) {
    super();
    this.cardNumber = cardNumber;
    this.cvv = cvv;
  }

  pay(amount) {
    console.log(
      `Paid $${amount} using credit card ending in ${this.cardNumber.slice(-4)}`,
    );
    return true;
  }
}

class PayPalPayment extends PaymentStrategy {
  constructor(email) {
    super();
    this.email = email;
  }

  pay(amount) {
    console.log(`Paid $${amount} using PayPal account ${this.email}`);
    return true;
  }
}

class CryptoCurrencyPayment extends PaymentStrategy {
  constructor(walletAddress) {
    super();
    this.walletAddress = walletAddress;
  }

  pay(amount) {
    console.log(
      `Paid $${amount} using cryptocurrency to wallet ${this.walletAddress}`,
    );
    return true;
  }
}

class ShoppingCart {
  constructor() {
    this.items = [];
    this.paymentStrategy = null;
  }

  addItem(item, price) {
    this.items.push({ item, price });
  }

  getTotal() {
    return this.items.reduce((total, item) => total + item.price, 0);
  }

  setPaymentMethod(strategy) {
    this.paymentStrategy = strategy;
  }

  checkout() {
    if (!this.paymentStrategy) {
      throw new Error("Payment method not set");
    }
    return this.paymentStrategy.pay(this.getTotal());
  }
}

// Usage
const cart = new ShoppingCart();
cart.addItem("Laptop", 999);
cart.addItem("Mouse", 25);

// Pay with credit card
cart.setPaymentMethod(new CreditCardPayment("4532-1234-5678-9010", "123"));
cart.checkout(); // Paid $1024 using credit card ending in 9010

// Pay with PayPal
cart.setPaymentMethod(new PayPalPayment("user@example.com"));
cart.checkout(); // Paid $1024 using PayPal account user@example.com

// Pay with crypto
cart.setPaymentMethod(
  new CryptoCurrencyPayment("1A1z7agoat2Bt89zVkUYyfHLXfPf94s1US"),
);
cart.checkout(); // Paid $1024 using cryptocurrency to wallet 1A1z7agoat2Bt89zVkUYyfHLXfPf94s1US
```

## Common Mistakes

❌ **Problem:** Throwing errors instead of providing default behavior in base Strategy.

```js
class Strategy {
  execute(a, b) {
    throw new Error("This method should be overridden!");
  }
}

// Developers might forget to override, causing runtime errors
class BadStrategy extends Strategy {
  // Forgot to implement execute()
}

const calculator = new Calculator(new BadStrategy());
calculator.executeStrategy(5, 3); // Crash: Error thrown
```

✅ **Solution:** Use optional methods or provide sensible defaults:

```js
class Strategy {
  // Optional: provide default implementation or remove entirely
  execute(a, b) {
    return a; // or just don't define it
  }
}

// Or use TypeScript interfaces for compile-time checking
```

❌ **Problem:** Over-engineering with too many strategies.

```js
// Anti-pattern: 50+ strategy classes for minor variations
class CalculateShippingCost {
  // Strategy for ground shipping in US
  // Strategy for ground shipping in Canada
  // Strategy for air shipping in US
  // Strategy for air shipping in Canada
  // ... (dozens more)
}
```

✅ **Solution:** Keep strategies focused and combinable:

```js
// Simpler: use configuration instead
function calculateShipping({ method, destination, weight }) {
  const baseCosts = { ground: 5, air: 20 };
  const multipliers = { US: 1, Canada: 1.5 };
  const weightFee = weight * 0.1;

  return baseCosts[method] * multipliers[destination] + weightFee;
}
```

❌ **Problem:** Strategies depend on each other or share state.

```js
class TaxStrategy {
  constructor(calculator) {
    this.calculator = calculator; // Tight coupling!
  }

  calculate(amount) {
    // Depends on calculator, hard to test in isolation
  }
}
```

✅ **Solution:** Keep strategies independent and pure:

```js
class TaxStrategy {
  calculate(amount, taxRate) {
    return amount * taxRate; // No dependencies, easy to test
  }
}
```

## Summary

The code implements the Strategy pattern to perform different arithmetic operations (addition, subtraction, multiplication, division) using interchangeable strategy objects. The `Calculator` class uses these strategies to execute the desired operation, allowing for flexible and dynamic changes to the algorithm used at runtime. This pattern eliminates large if/else chains and makes it easy to add new strategies without modifying existing code, following the Open/Closed Principle (open for extension, closed for modification).

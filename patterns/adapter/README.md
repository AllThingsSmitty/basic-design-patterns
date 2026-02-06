# Adapter Pattern

The Adapter pattern allows incompatible interfaces to work together by providing a wrapper that translates calls from one interface to another.

## When to Use

✅ **Use Adapter for:**

- Integrating third-party libraries
- Working with legacy code
- Wrapping incompatible APIs
- Data format conversions
- Cross-platform compatibility

❌ **Avoid Adapter for:**

- Simple type conversions (use built-in methods)
- When you can modify source code
- Over-engineering simple problems

## Example

```js
// Old interface
class OldCalculator {
  constructor() {
    this.operations = function (term1, term2, operation) {
      switch (operation) {
        case "add":
          return term1 + term2;
        case "sub":
          return term1 - term2;
        default:
          return NaN;
      }
    };
  }
}

// New interface
class NewCalculator {
  add(term1, term2) {
    return term1 + term2;
  }

  sub(term1, term2) {
    return term1 - term2;
  }
}

// Adapter Class
class CalculatorAdapter {
  constructor() {
    this.newCalculator = new NewCalculator();
  }

  operations(term1, term2, operation) {
    switch (operation) {
      case "add":
        return this.newCalculator.add(term1, term2);
      case "sub":
        return this.newCalculator.sub(term1, term2);
      default:
        return NaN;
    }
  }
}
```

## Explaining the code

1. **`OldCalculator` Class:**
   - This class represents the old interface for a calculator.
   - It has a method `operations` defined within the constructor that takes two terms and an operation type (`add` or `sub`).
   - Depending on the operation type, it either adds or subtracts the terms.

2. **`NewCalculator` Class:**
   - This class represents the new interface for a calculator.
   - It has two methods: `add` and `sub`, which perform addition and subtraction respectively.
   - This is incompatible with code expecting the old `operations` method.

3. **`CalculatorAdapter` Class:**
   - This class acts as an adapter to bridge the old interface (`OldCalculator`) with the new interface (`NewCalculator`).
   - It creates an instance of `NewCalculator` and uses its methods within the `operations` method to perform the required calculations.
   - This allows code that uses the old interface to work with the new interface without modification.

## Usage

```js
const oldCalc = new OldCalculator();
console.log(oldCalc.operations(10, 5, "add")); // 15

const newCalc = new NewCalculator();
console.log(newCalc.add(10, 5)); // 15

const adaptedCalc = new CalculatorAdapter();
console.log(adaptedCalc.operations(10, 5, "add")); // 15
```

- Instances of `OldCalculator`, `NewCalculator`, and `CalculatorAdapter` are created.
- The `operations` method of `OldCalculator` and `CalculatorAdapter`, as well as the add method of `NewCalculator`, are demonstrated with example inputs.

## Real-World Example

```js
// Legacy payment system (old interface)
class LegacyPaymentProcessor {
  processPayment(cardNumber, amount) {
    console.log(`Processing $${amount} with card ${cardNumber}`);
    return { success: true, transactionId: "legacy-123" };
  }
}

// New payment system (new interface)
class ModernPaymentGateway {
  authorize(paymentDetails) {
    console.log(`Authorizing payment: $${paymentDetails.amount}`);
    return { authorized: true, authCode: "AUTH-456" };
  }

  capture(authCode, amount) {
    console.log(`Capturing $${amount} with auth code ${authCode}`);
    return { captured: true, receiptId: "REC-789" };
  }
}

// Adapter to make new gateway work with old interface
class PaymentAdapter {
  constructor(gateway) {
    this.gateway = gateway;
  }

  processPayment(cardNumber, amount) {
    // Convert old interface to new interface
    const paymentDetails = {
      cardNumber: cardNumber,
      amount: amount,
    };

    try {
      const authResult = this.gateway.authorize(paymentDetails);
      if (authResult.authorized) {
        const captureResult = this.gateway.capture(authResult.authCode, amount);
        return {
          success: captureResult.captured,
          transactionId: captureResult.receiptId,
        };
      }
    } catch (error) {
      return { success: false, error: error.message };
    }
  }
}

// Usage: Legacy code works with new gateway through adapter
const oldProcessor = new LegacyPaymentProcessor();
const newGateway = new ModernPaymentGateway();
const adapter = new PaymentAdapter(newGateway);

// Old code still works
const result = adapter.processPayment("4532-1234-5678-9010", 99.99);
console.log(result);
// {
//   success: true,
//   transactionId: "REC-789"
// }
```

## Common Mistakes

❌ **Problem:** Adapter becomes too complex because the interfaces are too different.

```js
class BadAdapter {
  constructor(incompatibleLib) {
    this.lib = incompatibleLib;
  }

  adapt(input) {
    // Trying to adapt something fundamentally incompatible
    // Results in complex, hard-to-maintain code
    // ...hundreds of lines of transformation logic...
  }
}
```

✅ **Solution:** If interfaces are too different, reconsider the design:

- Use a Facade instead
- Redesign one of the components
- Use a more specialized pattern

❌ **Problem:** Adapter hides errors instead of translating them.

```js
class BadAdapter {
  adapt(input) {
    try {
      return this.service.process(input);
    } catch (error) {
      return null; // Hides the error!
    }
  }
}
```

✅ **Solution:** Translate errors properly:

```js
class GoodAdapter {
  adapt(input) {
    try {
      return this.service.process(input);
    } catch (error) {
      throw new Error(`Adaptation failed: ${error.message}`);
    }
  }
}
```

❌ **Problem:** Adapter creates performance overhead for simple conversions.

```js
// Overkill: creating adapter for simple number parsing
class StringToNumberAdapter {
  constructor(stringValue) {
    this.stringValue = stringValue;
  }

  getNumber() {
    return parseInt(this.stringValue, 10);
  }
}

const num = new StringToNumberAdapter("42").getNumber();
```

✅ **Solution:** Use direct conversion for simple cases:

```js
// Simple is better
const num = parseInt("42", 10);
```

## Summary

The Adapter pattern is used here to allow the `OldCalculator` interface to work with the `NewCalculator` interface by providing a `CalculatorAdapter` that translates the method calls. This pattern is useful for integrating new components into existing systems without modifying the existing code. It's especially valuable when dealing with legacy systems, third-party libraries, or APIs that you can't modify directly. The Adapter acts as a "translator" between incompatible interfaces, making integration smooth and non-intrusive.

# Chain-of-Responsibility Pattern

The Chain-of-Responsibility pattern passes a request along a chain of handlers, where each one either handles it or forwards it to the next. This example simulates an ATM dispensing bills across different denominations.

### Example

```js
class Request {
  constructor(amount) {
    this.amount = amount;
  }

  get(bill) {
    let count = Math.floor(this.amount / bill);
    this.amount -= count * bill;
    console.log(`Dispense ${count} $${bill} bills`);
  }
}

class Dispenser {
  constructor(bill, next) {
    this.bill = bill;
    this.next = next;
  }

  handle(request) {
    request.get(this.bill);
    if (this.next) {
      this.next.handle(request);
    }
  }
}
```

### Explaining the code

**Classes and Methods**

1. **Request Class:**

   - **Constructor:** Initializes the `amount` of money requested.
   - **`get(bill)`:** Calculates how many bills of a certain denomination can be dispensed from the current amount. It then reduces the amount by the total value of the dispensed bills and logs the number of bills dispensed.

2. **Dispenser Class:**
   - **Constructor:** Initializes the dispenser with a specific bill denomination (`bill`) and a reference to the next dispenser in the chain (next).
   - **`handle(request)`:** Dispenses the bills of its denomination by calling `request.get(this.bill)`. If there is a next dispenser in the chain, it passes the request to the next dispenser.

**Chain of Responsibility**

The dispensers are linked largest-to-smallest so the ATM always tries to give the fewest bills:

- `dispenser50` handles $50 bills first.
- `dispenser20` handles $20 bills next.
- `dispenser10` handles $10 bills next.
- `dispenser5` handles the remaining $5 bills last.

**Execution Flow**

1. A `Request` object is created with an amount of $85.
2. The `handle` method of `dispenser50` is called with the request.
3. `dispenser50` dispenses as many $50 bills as possible and passes the remainder to `dispenser20`.
4. `dispenser20` dispenses as many $20 bills as possible and passes the remainder to `dispenser10`.
5. `dispenser10` dispenses as many $10 bills as possible and passes the remainder to `dispenser5`.
6. `dispenser5` dispenses the remaining amount in $5 bills.

### Usage

```js
const dispenser5 = new Dispenser(5, null);
const dispenser10 = new Dispenser(10, dispenser5);
const dispenser20 = new Dispenser(20, dispenser10);
const dispenser50 = new Dispenser(50, dispenser20);

const request = new Request(85);
dispenser50.handle(request);
```

### Output

The console will display the number of bills dispensed for each denomination:

```
Dispense 1 $50 bills
Dispense 1 $20 bills
Dispense 1 $10 bills
Dispense 1 $5 bills
```

### Summary

Each dispenser handles its denomination and passes the remainder down the chain. That separation of concerns is the whole point of Chain-of-Responsibility: adding a new denomination (say, $100 bills) means plugging in a new handler at the front, with zero changes to the rest of the chain.

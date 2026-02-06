# Observer Pattern

The Observer pattern is a behavioral design pattern where an object (the subject) maintains a list of its dependents (observers) and notifies them of any state changes, usually by calling one of their methods.

## When to Use

✅ **Use Observer for:**

- Event listeners and handlers
- Pub/Sub messaging systems
- Real-time data updates (stock prices, notifications)
- Form validation feedback
- State management in applications

❌ **Avoid Observer for:**

- Simple one-time callbacks (use Promises)
- Synchronous operations only (consider async patterns)
- When unsubscribing isn't guaranteed (causes memory leaks)

## Example

```js
// Subject
class Subject {
  constructor() {
    this.observers = [];
  }

  subscribe(observer) {
    this.observers.push(observer);
  }

  unsubscribe(observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }

  notify(data) {
    this.observers.forEach((observer) => {
      if (observer && typeof observer.update === "function") {
        observer.update(data);
      }
    });
  }
}

// Observer
class Observer {
  update(data) {
    console.log(`Observer received data: ${data}`);
  }
}
```

## Explaining the code

**Classes and Methods**

1. **Subject Class**
   - **Constructor:** Initializes an empty array `observers` to keep track of the observers.
   - **`subscribe(observer)`:** Adds an observer to the `observers` array.
   - **`unsubscribe(observer)`:** Removes an observer from the `observers` array by filtering it out.
   - **`notify(data)`:** Calls the `update` method on each observer in the `observers` array, passing the data to them. Now includes safety checks.

2. **Observer Class**
   - **`update(data)`:** A method that logs the received data to the console. This method is intended to be overridden by concrete observer implementations.

## Usage

```js
const subject = new Subject();

const observer1 = new Observer();
const observer2 = new Observer();

subject.subscribe(observer1);
subject.subscribe(observer2);

subject.notify("Hello Observers!"); // Both observers will log the data

subject.unsubscribe(observer1);

subject.notify("Hello again!"); // Only observer2 will log the data
```

- **Creating Instances:**
  - A `Subject` instance is created.
  - Two `Observer` instances are created.
- **Subscribing Observers:**
  - Both observers are subscribed to the subject.
- **Notifying Observers:**
  - The subject notifies all subscribed observers with the message "Hello Observers!".
  - The first observer is unsubscribed.
  - The subject notifies the remaining observers with the message "Hello again!".

## Real-World Example

```js
class NewsChannel {
  constructor(name) {
    this.name = name;
    this.subscribers = [];
  }

  subscribe(subscriber) {
    this.subscribers.push(subscriber);
    console.log(`${subscriber.name} subscribed to ${this.name}`);
  }

  unsubscribe(subscriber) {
    this.subscribers = this.subscribers.filter((s) => s !== subscriber);
    console.log(`${subscriber.name} unsubscribed from ${this.name}`);
  }

  publishNews(news) {
    console.log(`\n${this.name} is publishing news...`);
    this.subscribers.forEach((subscriber) => {
      subscriber.receiveNews(news);
    });
  }
}

class Subscriber {
  constructor(name) {
    this.name = name;
  }

  receiveNews(news) {
    console.log(`${this.name} received: "${news}"`);
  }
}

// Usage
const channel = new NewsChannel("Tech Daily");
const subscriber1 = new Subscriber("Alice");
const subscriber2 = new Subscriber("Bob");

channel.subscribe(subscriber1);
channel.subscribe(subscriber2);

channel.publishNews("New JavaScript features released!");

channel.unsubscribe(subscriber1);

channel.publishNews("React 19 is out!");
// Only Bob receives the second update
```

## Common Mistakes

❌ **Problem:** Memory leaks from unsubscribed observers.

```js
// Observers keep holding references even after unsubscribe
const observer = new Observer();
subject.subscribe(observer);
subject.unsubscribe(observer);

observer = null; // Might still be referenced if not properly unsubscribed
```

✅ **Solution:** Ensure observers are properly unsubscribed, or use a pattern that returns an unsubscribe function:

```js
class SafeSubject {
  constructor() {
    this.observers = new Set(); // Use Set for easier tracking
  }

  subscribe(observer) {
    this.observers.add(observer);

    // Return an unsubscribe function
    return () => {
      this.observers.delete(observer);
    };
  }

  notify(data) {
    this.observers.forEach((observer) => {
      try {
        observer.update(data);
      } catch (error) {
        console.error("Observer error:", error);
      }
    });
  }
}

// Usage with automatic cleanup
const unsubscribe = subject.subscribe(observer);
unsubscribe(); // Guaranteed cleanup
```

❌ **Problem:** Errors in one observer break notifications for others.

```js
class BrokenObserver {
  update(data) {
    throw new Error("Observer is broken!");
  }
}

subject.notify(data); // One broken observer stops all others from being notified
```

✅ **Solution:** Wrap observer calls in try-catch:

```js
notify(data) {
  this.observers.forEach((observer) => {
    try {
      if (observer && typeof observer.update === "function") {
        observer.update(data);
      }
    } catch (error) {
      console.error("Error notifying observer:", error);
      // Continue notifying others instead of stopping
    }
  });
}
```

## Summary

This code demonstrates the Observer pattern where a `Subject` can notify multiple `Observer` instances about changes. Observers can subscribe to or unsubscribe from the subject. When the subject's state changes, it notifies all subscribed observers by calling their `update` method. This pattern is useful for implementing distributed event handling systems. Always remember to unsubscribe when observers are no longer needed to prevent memory leaks, and handle errors gracefully to ensure one failing observer doesn't break the entire notification chain.

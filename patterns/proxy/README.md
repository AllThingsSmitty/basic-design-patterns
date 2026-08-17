# Proxy Pattern

The Proxy pattern puts a stand-in in front of the real object. The caller interacts with the proxy, which can add access control, logging, caching, or lazy loading before (and after) delegating to the actual implementation.

### Example

```js
class RealSubject {
  request() {
    console.log("RealSubject: Handling request.");
  }
}

class Proxy {
  constructor(realSubject) {
    this.realSubject = realSubject;
  }

  request() {
    if (this.checkAccess()) {
      this.realSubject.request();
      this.logAccess();
    }
  }

  checkAccess() {
    console.log("Proxy: Checking access prior to firing a real request.");
    // Simulate access check
    return true;
  }

  logAccess() {
    console.log("Proxy: Logging the time of request.");
  }
}
```

### Explaining the code

1. **`RealSubject` Class:**

   - This class represents the real object that performs the actual work. It has a `request` method that logs a message indicating it is handling the request.

2. **Proxy Class:**

   - This class acts as a proxy to the `RealSubject`. It holds a reference to an instance of `RealSubject` and controls access to it.
   - The `request` method in the `Proxy` class first checks access by calling `checkAccess`. If access is granted, it forwards the request to the `RealSubject` and then logs the access by calling `logAccess`.

### Usage

```js
const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);

proxy.request();
```

- An instance of `RealSubject` is created.
- An instance of `Proxy` is created, with the `RealSubject` instance passed to its constructor.
- The `request` method is called on the `Proxy` instance. This triggers the access check, forwards the request to the `RealSubject` if access is granted, and logs the access.

### Summary

`RealSubject` doesn't know it's being proxied; it just handles requests. All the cross-cutting concerns (access checks, logging) live in the proxy. This keeps each class focused on one job, and you can add or change proxy behavior without ever touching the underlying implementation.

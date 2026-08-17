# Iterator Pattern

The Iterator pattern lets you walk through a collection without needing to know how it's stored internally (array, linked list, tree). The collection hands you an iterator, and you just call `next()`.

### Example

```js
class Iterator {
  constructor(items) {
    this.items = items;
    this.index = 0;
  }

  next() {
    return this.items[this.index++];
  }

  hasNext() {
    return this.index < this.items.length;
  }
}

class IterableCollection {
  constructor(items) {
    this.items = items;
  }

  createIterator() {
    return new Iterator(this.items);
  }
}
```

### Explaining the code

1. **Iterator Class**

   - **Constructor:** Initializes the `items` array and sets the starting `index` to 0.
   - **`next()` Method:** Returns the current item and increments the index.
   - **`hasNext()` Method:** Checks if there are more items to iterate over.

2. **IterableCollection Class**
   - **Constructor:** Initializes the `items` array.
   - **`createIterator()` Method:** Creates and returns a new `Iterator` instance for the collection.

### Usage

```js
// Usage
const collection = new IterableCollection(["item1", "item2", "item3"]);
const iterator = collection.createIterator();

while (iterator.hasNext()) {
  console.log(iterator.next());
}
```

- **Creating Collection:** An instance of `IterableCollection` is created with an array of items.
- **Creating Iterator:** An iterator is created for the collection using `createIterator()`.
- **Iterating:** A `while` loop is used to iterate over the collection, printing each item until there are no more items.

### Summary

The iterator keeps all traversal state (the current index) out of the collection itself. That means you can have multiple iterators on the same collection at the same time without them interfering. Worth noting: in modern JavaScript you'd usually use a generator or the built-in `Symbol.iterator` protocol, but rolling your own like this makes the mechanics easy to see.

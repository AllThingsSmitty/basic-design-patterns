# Composite Pattern

The Composite pattern allows you to compose objects into tree structures to represent part-whole hierarchies. It lets clients treat individual objects and compositions of objects uniformly.

## When to Use

✅ **Use Composite for:**

- Tree structures (files/folders, org charts, UI components)
- Part-whole hierarchies
- Recursive structures with uniform treatment
- DOM trees, menu hierarchies
- When clients should treat individual and composite objects the same

❌ **Avoid Composite for:**

- Flat lists (use simple arrays)
- When individual and composite objects need different treatment
- Performance-critical shallow structures

## Example

```js
// Component
class Component {
  constructor(name) {
    this.name = name;
  }

  add(component) {
    throw new Error("Method not implemented");
  }

  remove(component) {
    throw new Error("Method not implemented");
  }

  display(depth) {
    throw new Error("Method not implemented");
  }
}

// Leaf
class Leaf extends Component {
  constructor(name) {
    super(name);
  }

  add(component) {
    console.warn(
      `Cannot add to leaf "${this.name}". Leaves cannot have children.`,
    );
  }

  remove(component) {
    console.warn(
      `Cannot remove from leaf "${this.name}". Leaves have no children.`,
    );
  }

  display(depth) {
    console.log("-".repeat(depth) + this.name);
  }
}

// Composite
class Composite extends Component {
  constructor(name) {
    super(name);
    this.children = [];
  }

  add(component) {
    this.children.push(component);
  }

  remove(component) {
    const index = this.children.indexOf(component);
    if (index !== -1) {
      this.children.splice(index, 1);
    }
  }

  display(depth) {
    console.log("-".repeat(depth) + this.name);
    for (const child of this.children) {
      child.display(depth + 2);
    }
  }
}
```

## Explaining the code

1. **Component Class:**
   - This is an abstract class that defines the interface for all objects in the composition.
   - It has methods `add`, `remove`, and `display` which are meant to be overridden by subclasses.
   - By default, these methods throw an error indicating they are not implemented.

2. **Leaf Class:**
   - This class represents the leaf objects in the composition. A leaf has no children.
   - It overrides the `add` and `remove` methods to warn that these operations are not supported on leaves.
   - The `display` method prints the name of the leaf with a specific depth.

3. **Composite Class:**
   - This class represents the composite objects that can have children.
   - It maintains a list of child components.
   - The `add` method adds a child component to the list.
   - The `remove` method removes a child component from the list.
   - The `display` method prints the name of the composite and then recursively calls `display` on its children, increasing the depth.

## Usage

```js
const root = new Composite("root");
const branch1 = new Composite("branch1");
const branch2 = new Composite("branch2");
const leaf1 = new Leaf("leaf1");
const leaf2 = new Leaf("leaf2");
const leaf3 = new Leaf("leaf3");

root.add(branch1);
root.add(branch2);
branch1.add(leaf1);
branch2.add(leaf2);
branch2.add(leaf3);

root.display(1);
```

- A tree structure is created with a root composite, two branch composites, and three leaf nodes.
- The `add` method is used to build the tree structure.
- The `display` method is called on the root to print the entire structure.

### Output

The `display` method will produce the following output:

```
-root
--branch1
----leaf1
--branch2
----leaf2
----leaf3
```

This output shows the hierarchical structure of the composite tree, with each level of depth indicated by a series of dashes.

## Real-World Example

```js
class FileSystem {
  constructor(name) {
    this.name = name;
    this.size = 0;
  }

  getSize() {
    return this.size;
  }
}

class File extends FileSystem {
  constructor(name, size) {
    super(name);
    this.size = size;
  }

  getSize() {
    return this.size;
  }

  display(depth) {
    console.log("-".repeat(depth) + `📄 ${this.name} (${this.size}KB)`);
  }
}

class Directory extends FileSystem {
  constructor(name) {
    super(name);
    this.children = [];
  }

  add(item) {
    this.children.push(item);
  }

  remove(item) {
    this.children = this.children.filter((child) => child !== item);
  }

  getSize() {
    return this.children.reduce((total, child) => total + child.getSize(), 0);
  }

  display(depth) {
    console.log("-".repeat(depth) + `📁 ${this.name}/ (${this.getSize()}KB)`);
    for (const child of this.children) {
      child.display(depth + 2);
    }
  }
}

// Usage
const root = new Directory("home");
const documents = new Directory("Documents");
const photos = new Directory("Photos");

const file1 = new File("resume.pdf", 250);
const file2 = new File("letter.doc", 50);
const file3 = new File("vacation.jpg", 2500);
const file4 = new File("family.jpg", 1800);

root.add(documents);
root.add(photos);

documents.add(file1);
documents.add(file2);

photos.add(file3);
photos.add(file4);

root.display(0);
// 📁 home/ (4600KB)
// --📁 Documents/ (300KB)
// ----📄 resume.pdf (250KB)
// ----📄 letter.doc (50KB)
// --📁 Photos/ (4300KB)
// ----📄 vacation.jpg (2500KB)
// ----📄 family.jpg (1800KB)

console.log(`Total size: ${root.getSize()}KB`); // Total size: 4600KB
```

## Common Mistakes

❌ **Problem:** Silently failing when operations aren't supported on leaves confuses developers.

```js
class Leaf extends Component {
  add(component) {
    console.log("Cannot add to a leaf"); // Silent failure, no error
  }
}

const leaf = new Leaf("leaf");
leaf.add(new Leaf("child")); // No error thrown, but nothing happened
// Developer doesn't know if this succeeded or failed!
```

✅ **Solution:** Throw errors or warn clearly:

```js
class Leaf extends Component {
  add(component) {
    throw new Error(
      `Cannot add child to leaf "${this.name}". Leaves cannot have children.`,
    );
  }
}

// Now mistakes are obvious
const leaf = new Leaf("leaf");
try {
  leaf.add(new Leaf("child"));
} catch (error) {
  console.error(error); // Clear failure
}
```

❌ **Problem:** Forgetting to implement methods in subclasses causes runtime errors.

```js
class Component {
  display(depth) {
    throw new Error("Method not implemented");
  }
}

class MyComponent extends Component {
  // Forgot to implement display()
}

const comp = new MyComponent();
comp.display(0); // Runtime error
```

✅ **Solution:** Use TypeScript interfaces or document requirements:

```js
// Document clearly what must be implemented
class Component {
  /**
   * @required All subclasses must implement this method
   */
  display(depth) {
    throw new Error("Method not implemented");
  }
}
```

❌ **Problem:** Complex tree operations are hard to debug.

```js
// Hard to find bugs in deep trees
root.display(0); // If something is wrong, hard to trace where
```

✅ **Solution:** Add helpful debugging methods:

```js
class Component {
  getPath() {
    return this.name; // Show where this component is
  }

  validate() {
    // Check component integrity
    if (!this.name) throw new Error("Component must have a name");
  }
}
```

## Summary

The tree is built by adding branches and leaves to the appropriate composites, and the structure is displayed starting from the root with a depth of 1. The Composite pattern is useful for representing hierarchical structures and allows for flexible and uniform treatment of individual objects and compositions. This pattern is especially valuable when building file systems, UI hierarchies, organization charts, or any tree-like data structure where the same operations should work on both leaf nodes and composite (branch) nodes.

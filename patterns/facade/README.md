# Facade Pattern

The Facade pattern puts a simple, clean interface in front of a messy or complex subsystem. Callers don't need to know the internals; they just use the facade.

### Example

```js
// Subsystem 1: CPU class with methods to freeze, jump to a position, and execute instructions
class CPU {
  // Freezes the CPU
  freeze() {
    console.log("Freezing CPU...");
  }

  // Jumps to a specific position in memory
  jump(position) {
    console.log(`Jumping to position ${position}...`);
  }

  // Executes instructions
  execute() {
    console.log("Executing instructions...");
  }
}

// Subsystem 2: Memory class with a method to load data at a specific position
class Memory {
  // Loads data into a specific position in memory
  load(position, data) {
    console.log(`Loading data '${data}' at position ${position}...`);
  }
}

// Subsystem 3: HardDrive class with a method to read data from a specific LBA (Logical Block Addressing)
class HardDrive {
  // Reads data from a specific LBA and returns the data
  read(lba, size) {
    console.log(`Reading ${size} bytes from LBA ${lba}...`);
    return "data";
  }
}

// Facade: ComputerFacade class to simplify the interaction with the subsystems
class ComputerFacade {
  constructor() {
    // Initialize subsystems
    this.cpu = new CPU();
    this.memory = new Memory();
    this.hardDrive = new HardDrive();
  }

  // Method to start the computer by coordinating the subsystems
  start() {
    // Freeze the CPU
    this.cpu.freeze();
    // Load data from the hard drive into memory
    this.memory.load(0, this.hardDrive.read(0, 1024));
    // Jump to the start position in memory
    this.cpu.jump(0);
    // Execute instructions
    this.cpu.execute();
  }
}
```

### Explaining the code

**Subsystem Classes:**

- **`CPU`:** Represents the CPU with methods to freeze, jump to a memory position, and execute instructions.
- **`Memory`:** Represents the memory with a method to load data at a specific position.
- **`HardDrive`:** Represents the hard drive with a method to read data from a specific Logical Block Addressing (LBA).

**Facade Class:**

- **ComputerFacade:** Simplifies the interaction with the subsystems by providing a higher-level interface. It initializes instances of the `CPU`, `Memory`, and `HardDrive` classes and provides a `start` method to coordinate their actions.

### Usage

```js
const computer = new ComputerFacade();
computer.start();
```

- Creates an instance of `ComputerFacade` and calls the `start` method to start the computer, which internally coordinates the actions of the `CPU`, `Memory`, and `HardDrive` subsystems.

### Summary

Instead of making callers juggle `CPU`, `Memory`, and `HardDrive` themselves, `ComputerFacade.start()` handles the coordination. If the boot sequence ever changes, you update one method, not every caller. That's the value: one clean entry point that absorbs the complexity.

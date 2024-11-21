# Loading Bar Library for AFlat

This is a lightweight, multithreaded console loading bar library for the AFlat programming language. It provides an efficient and visually clear progress bar in the terminal, displaying dynamic updates during program execution.

## Installation

Install the library in your AFlat project using:

```bash
aflat install https://github.com/DeForestt/loading_bar
```

## Features

- **Multithreaded Execution:** The loading bar runs in a separate thread, ensuring it does not block the main thread.
- **Shared Memory Synchronization:** Uses shared memory to synchronize the progress and state between threads.
- **Dynamic Progress Display:** Outputs a hash-based (`#`) bar that grows as the operation progresses, along with a percentage display.

## Usage

### Example Code

Here's how to use the library in your project:

```aflat
.needs <std>
import {createLoadingBar} from "./src/loading_bar" under lb;

int main() {
    let bar = lb.createLoadingBar(100, 20);  // Create a loading bar with 100 steps and 20-character width
    bar.run();  // Start the loading bar in a separate thread

    for int i = 0; i < 100; i++ {
        bar.update();  // Increment the progress
        sleep(1);      // Simulate work
    };

    bar.kill();  // Stop the loading bar after work is complete
    return 0;
};
```

### Example Output

When executed, the loading bar appears in the terminal and updates dynamically as progress is made:

```
Loading: [######                  ] 30%
```

## API Documentation

### `createLoadingBar(int max, int resolution = 20) -> LoadingBar`
Creates a new instance of the `LoadingBar`.

- `max`: Total steps required to complete the progress bar.
- `resolution`: Width of the loading bar in characters (default: 20).

### Class: `LoadingBar`

#### Methods

1. **`run()`**
   Starts the loading bar in a separate thread. This method continuously updates the loading bar's display.

2. **`update()`**
   Updates the progress of the loading bar by one step. Call this method from the main thread to reflect progress.

3. **`kill()`**
   Stops the loading bar thread gracefully. Should be called when the loading process completes or needs to stop.

4. **`isKilled() -> bool`**
   Returns `true` if the loading bar has been stopped using the `kill()` method.

## How It Works

- **Multithreading:** The `run()` method spawns a thread to handle the bar's display independently of the main program.
- **Hash-Based Display:** Progress is visually represented by a growing sequence of hashes (`#`) within the defined resolution.
- **Shared Memory:** Synchronizes the current progress and state (`kill` flag) between the main thread and the bar's thread for thread-safe operation.

## Customization

- **Resolution:** Customize the width of the bar by specifying a `resolution` when creating it, e.g., `createLoadingBar(100, 50)`.
- **Thread Safety:** Built-in shared memory ensures the bar is safe to use in concurrent environments.

## Contributing

Contributions are welcome! Feel free to fork the repository, implement your improvements, and submit a pull request. If you're planning significant changes, please open an issue to discuss them.

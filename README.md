# Data Structures and Algorithms Using C++

A personal, from-scratch collection of classic data structures and algorithms implemented in modern C++ templates. Every structure and algorithm is written as a standalone, generic, header-only component and paired with a small runnable demo program, making the repository useful both as a learning reference and as a set of drop-in headers.

## Purpose

This repository is an educational implementation library. The goal is to understand each data structure and algorithm by building it directly — no STL containers standing in for the thing being demonstrated — while keeping the code reusable:

- **Generic** — nearly everything is a `template <typename T>`, so the containers and algorithms work with any comparable element type.
- **Header-only** — each component lives entirely in a `.h` file under `inc/` (or `utils/`), so using one is just an `#include`.
- **Namespaced** — code is organized under three namespaces: `ds` (data structures), `algo` (algorithms), and `utils` (helpers).
- **Self-documenting** — each header opens with a comment describing the structure/algorithm and, where relevant, its time and space complexity.
- **Demonstrated** — every component has a matching `*Demo.cpp` in `src/` with a `main()` that exercises it.

## What's included

### Data structures (`namespace ds`)

**Heaps** (`inc/linearStructures/heap/`)

- `MinHeap` — binary min-heap with array/size constructors.
- `MaxHeap` — binary max-heap.
- `TopKHeap` — maintains the top *K* elements seen in a stream.
- `LeastKHeap` — maintains the least *K* elements seen in a stream.
- `MedianHeap` — running median using a two-heap (min/max) design.

**Lists** (`inc/linearStructures/list/`)

- `SinglyLinkedList` — singly linked list.
- `DoublyLinkedList` — doubly linked list.
- `Stack` — LIFO stack (includes a `toArray()` helper).
- `Queue` — FIFO queue.

**Automatons** (`inc/automatons/`)

- `Trie` — prefix tree supporting `insert`, `search`, and `startsWith`.
- `AhoCorasickAutomaton` — multi-pattern string matcher. *Work in progress (currently a stub).*

### Algorithms (`namespace algo`)

**Sorting** (`inc/sorting/`)

- `insertionSort`
- `selectionSort`
- `shellSort`
- `mergeSort`
- `quickSort` — randomized pivot, Lomuto partition scheme.
- `heapSort`

**Selection & subarray**

- `quickSelect` (`inc/quickSelect.h`) — *k*-th order statistic in expected linear time.
- `maxContiguousSubArray` (`inc/maxContiguousSubArray.h`) — maximum subarray (divide and conquer).
- `kadane` (`inc/kadane.h`) — maximum subarray in O(n) / O(1) via Kadane's algorithm.

**Heapify primitives**

- `minHeapify` (`inc/minHeapify.h`)
- `maxHeapify` (`inc/maxHeapify.h`)

### Utilities (`namespace utils`, `utils/`)

- `swap.h` — generic in-place swap.
- `checkMinHeapProperty.h` / `checkMaxHeapProperty.h` — validation helpers used by the heap demos.

## Project layout

```
.
├── inc/                     # Header-only implementations
│   ├── automatons/          # Trie, Aho–Corasick (WIP)
│   ├── linearStructures/
│   │   ├── heap/            # Min/Max/TopK/LeastK/Median heaps
│   │   └── list/            # Singly/Doubly linked lists, Stack, Queue
│   ├── sorting/             # insertion, selection, shell, merge, quick, heap
│   ├── kadane.h
│   ├── maxContiguousSubArray.h
│   ├── maxHeapify.h  minHeapify.h
│   └── quickSelect.h
├── src/                     # One *Demo.cpp per component (each has main())
├── utils/                   # swap + heap-property checkers
├── Makefile                 # Builds every demo into ./bin
└── .gitignore               # Ignores ./bin and ./logs
```

## Requirements

- A C++ compiler. The `Makefile` uses **clang++**; **g++** works equally well.
- **GNU Make**.
- A POSIX-like shell (Linux/macOS, or WSL/MSYS2 on Windows).

No external libraries are required (the Makefile links `-lm` only).

## Building

The `Makefile` compiles each demo in `src/` into its own executable under `bin/`.

Build everything (the `all` target creates `bin/` first, then builds all demos):

```bash
make all      # or simply: make
```

Build a single demo (for example the quicksort or trie demo):

```bash
make init         # create ./bin once (only needed if it doesn't exist yet)
make quickSortDemo
make Trie
```

Available targets mirror the demos: `minHeapifyDemo`, `maxHeapifyDemo`, `quickSelectDemo`,
`minHeapDemo`, `maxHeapDemo`, `maxContiguousDemo`, `kadaneDemo`, `heapSortDemo`,
`insertionSortDemo`, `quickSortDemo`, `mergeSortDemo`, `selectionSortDemo`, `shellSortDemo`,
`TopKdemo`, `LeastKdemo`, `medianDemo`, `DoublyListDemo`, `SinglyListDemo`, `StackDemo`,
`QueueDemo`, `Trie`, and `test`.

Clean all build output:

```bash
make clean
```

> **Note:** `make init` runs `mkdir bin`, so if `bin/` already exists, run individual targets directly rather than re-running `init`. A `make clean` followed by `make all` always works from a clean state.

## Running

Executables are written to `bin/`. Run any of them directly:

```bash
./bin/quickSortDemo
./bin/Trie
./bin/medianDemo
```

Most demos seed random input and print the structure/result to standard output (for
example, the stack demo pushes random integers and prints the stack; the trie demo inserts
words and prints `search` / `startsWith` results).

## Using a component in your own code

Because everything is header-only and templated, you only need to include the header and
add the relevant directory to your include path. For example, to use the min-heap:

```cpp
#include "MinHeap.h"

int main() {
    ds::MinHeap<int> heap;
    // ...
}
```

```bash
clang++ -Wall -g main.cpp \
  -I inc/linearStructures/heap -I inc -I utils -o main
```

The include paths for each component match those in the `Makefile` — check the corresponding
target for the exact `-I` flags a given header needs (some headers include others, e.g. the
heaps include the heapify primitives and `Trie` includes `Stack`).

## Status

Actively maintained as a learning project. Most structures and algorithms are complete and
demonstrated; `AhoCorasickAutomaton` is a placeholder marked `TODO`.

## Author

**Ritesh Saha** — [@Ritesh7766](https://github.com/Ritesh7766)

## License

Released under the [MIT License](LICENSE).

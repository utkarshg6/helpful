# Rust

## Benefits of Using Rust

- Memory Safe
  - Chromium Project has 70% vulnerabilities related to Memory.
  - C/C++ doesn’t provide Memory Safety
  - Python, JS provides Memory Safety but uses Garbage Collector
  - Rust provides Memory Safety without using Garbage Collector.
  - Rust achieves this functionality by making the language, thus the code won’t compile if any vulnerability found.
- No Null Types or No Null Pointers
  - Null Pointers in C are used as `*ptr = NULL`.
  - Rust uses it’s rich type system to represent the absence of a value.
- No Exceptions 
  - Rust Provides No Direct Referencing, No Pointers and No Pointer Exceptions.
- Modern Package Manager
  - It uses Package Manager 
- No Data Race
  - Game Changer for writing Asynchronous Code



## Rust Basic Commands

- Create New Boilerplate Project

  ```bash
  cargo new {project-name}
  ```

- Build a Project (Installing Dependencies and Compiling)

  ```bash
  cargo build
  ```

- Run a Project

  ```bash
  cargo run
  ```

- 
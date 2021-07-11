# Rust

## Benefits of Using Rust

- Memory Safe
  - Chromium Project has 70% vulnerabilities related to Memory.
  - C/C++ doesn’t provide Memory Safety
  - Python, JS provides Memory Safety but uses Garbage Collector
  - Rust provides Memory Safety without using Garbage Collector.
  - Rust achieves this functionality by making the language typed, thus the code won’t compile if any vulnerability will exist.
- No Null Types or No Null Pointers
  - Null Pointers in C are used as `*ptr = NULL`.
  - Rust uses it’s rich type system to represent the absence of a value.
- No Exceptions 
  - Rust Provides No Direct Referencing, No Pointers and No Pointer Exceptions.
- Modern Package Manager
  - It uses Package Manager 
- No Data Race
  - A data race occurs when:
    - two or more threads in a **single process** access the same memory location concurrently, and
    - at least one of the accesses is for writing, and
    - the threads are not using any exclusive locks to control their accesses to that memory
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

- We can install `cargo-expand` to use cargo libraries system wide.

  ```zsh
  cargo install cargo-expand
  ```

- The cargo expand command

  ```zsh
  cargo expand
  ```

- List the rustup toolchain

  ```zsh
  rustup toolchain list
  ```

- Install rustup toolchain

  ```zsh
  rustup toolchain install nightly-x86_64-unknown-linux-gnu
  ```

## Memory Management

  These are the crucial parts of the Memory Management:

  - The Stack
    - It stores the variables created by each function.
    - It is a special region in process memory.
    - For each call of a function, a new stack frame is allocated on top of the current one.
    - This gives scope to the function.
    - The size of each variable should be known at compile time.
    - When a function exits, it’s stack frame is released.
    - IMP: Stack has a limited size.
    - If our program reaches end of the stack due to infinite recursion, it’ll cause the problem of Stack Overflow.
    - We do not have to manually de-allocate the memory on stack.
  - The Heap
    - Heap is **NOT** automatically managed. 
    - It means we’ll have to manually allocate and de-allocate the memory.
    - It has no size restrictions but is limited by the physical memory of the system.
    - It is accessible by any function, anywhere in the program.
    - Heap allocations are expensive and we should avoid them whenever possible.
    - If we allocate the memory and forget to deallocate it, the information will still exist on the heap even if the execution of the function ha ended and removed from stack, this is known as memory leak.
    - Hence, always deallocate the memory that has been allocated in heap.
  - Pointers
  - Smart Pointers
    - They solves the problem of memory leak as seen in allocating space in heap.
    - They use a wrapper when declaring the variable.
    - When the variable goes out of scope it will automatically deallocate the memory.

## Data Types

Basic Data Types

- Booleans
  - Represented by `bool`, has two values `true` and `false`.
- Characters
  - Represented by `char`, it always has space of `4` bytes or `32` bits.
- Integers
  - `u` means unsigned (only positive), `i` means signed (both positive & negative)
  - The size ranges from `8` bits to `128` bits.
  - Examples, `u8`, `i8`, `u128`, `i128`
  - Rust also supports, `usize` and `isize` which means that it will take up space according to the architecture whether it is 32 bit or 64 bit.
- Floats	
  - It has `f32` and `f64`, two floating data types for size `32` and `64`.



## Syntax

- The “Hello, World!” Program

  - Main function is the first function that gets called.
  - `println!()` is not a function but a macro
  - Macros contain an `!` mark.

  ```rust
  fn main() {
      println!("Hello, world!");
  }
  ```

- Variables

  - All variables in rust are immutable by default.

  ```rust
  let variable_name = name_of_function(100); // It'll figure out the data type from function's return type
  ```

- Mutable Variable

  ```rust
  let mut variable_name = name_of_function(100); // Notice new mut keyword
  ```

  

- Simple Function

  ```rust
  fn name_of_function(input_variable: {input_data_type}) -> {output_data_type} {
      return output_variable;
  }
  ```

- Simple Function without `return` keyword.

  - You can remove semicolon and return keyword to return a variable.
  - It will implicitly tell to return this variable.

  ```rust
  fn name_of_function(input_variable: {input_data_type}) -> {output_data_type} {
      output_variable
  }
  ```

## Concepts

### Macros

- Macros contain an `!` mark. For example, `println!()`.

- Macros are used for Meta Programming.

- It’s a way of writing code that writes more code.

- A macro can be called with a variable number of parameters with different types everytime.

- The downside is that Macros definition is more complex than function definitions.

- It is much harder to read and maintain.

- `println!()` is a macro because it can receive variable number of arguments.

  ```rust
  println!("Number: {}", 100);
  ```

- We can use `cargo expand` command to see what the macros of our code expand into.



### Standard Library

- It is an external crate.

- This crate is provided to every rust project.

- You can visit it [here](https://doc.rust-lang.org/std/#modules).

- To import

  ```rust
  use std::io;
  ```

- To use

  ```rust
  let mut input = String::new(); // Create a New String (String is a type of Smart Pointer)
  io::stdin().read_line(&mut input);
  ```

### Ownership

- Each value in rust is owned by a variable.

- When the owner gets out of scope, the value will be deallocated.

- There can only be **ONE** owner at a time.

- This means that no two pointers can point to two values.

- Ownership Transfer

  ```rust
  let mut input = String::new(); // String is a Smart Pointer, so is a part of heap
  let mut s = input; // Since String is on heap, so it's ownership gets transferred from input to s
  ```

- Value Copied

  ```rust
  let a = 5; // Variable a is a number, so it is a part of stack.
  let b = a; // Since 5 is on stack, so it's value gets copied to b.
  ```

- Why we can’t pass `input` variable directly to `io`.

  ```rust
  let mut input = String::new(); // String is allocated to heap, it can not be copied but transferred
  io::stdin().read_line(input); // FAIL: We tried to transfer ownership, rust don't like it
  io::stdin().read_line(&mut input); // SUCCESS: We are now borrowing instead of transferring ownership
  ```

### References

- It allows to refer to a variable, instead of transferring ownership.

- By adding `&` operator before a variable inside functions argument declaration specifies that the particular argument will have it’s reference stored instead of transferring ownership.

  ```rust
  // An example of expecting ownership transfer
  fn some_fn(s : String) {
      ...
  }
  
  some_fn(my_string);
  
  // An example of expecting a reference
  fn some_fn(s: &String) {
      ...
  }
  
  some_fn(&my_string);
  
  // An example of expecting a *mutable* reference
  fn some_fn(s: &mut String) {
      ...
  }
  
  some_fn(&mut my_string);
  ```

- The process of passing as a reference is called *borrowing*.

- In a scope, we can have as many immutable reference we want or a single mutable reference.

- The compiler won’t allow us to have an immutable borrow and mutable borrow at the same time.

  ```rust
  let s1 = &input;
  let s2 = &input;
  
  println!("{} {}", s1, s2); // This will work because we can pass as many immutable variable as we want.
  
  let mut s1 = &mut input;
  let s2 = &input;
  
  println!("{} {}", s1, s2); // This will fail because we can only pass only one mutable variable at the same time.
  ```

- This limitation allows mutation in a very controlled fashion.

- This prevents data races at compile time. A very powerful feature.

- Smart feature of Rust Compiler

  ```rust
  // FAIL: Using immutable borrow after mutable referencing
  let mut input = String::new(); // Mutable Variable
  let s1 = &input; // Immutable Borrow
  
  some_fn(&mut input); // Mutable Referencing
  println!("{}", s1); // Use of immutable borrow
  
  // SUCCESS: Finished all immutable borrowing task before mutable referencing
  let mut input = String::new(); // Mutable Variable
  let s1 = &input; // Immutable Borrow
  
  println!("{}", s1); // Use of immutable borrow
  some_fn(&mut input); // Mutable Referencing
  ```

### `unwrap()`

- If we call unwrap it will make sure that if the result was an error, it will terminate our program, if it is a success it will yield the contents of it’s results.

- We use it while taking input.

  ```rust
  io::stdin().read_line(&mut input).unwrap();
  ```


### `trim()`

- This function removes all whitespaces from a string.

  ```rust
  let trimmed_string = input_string.trim();
  ```

  

### `dbg!()`

- This is a debug macro, we can pass any variable inside it to see debug logs in console.

  ```rust
  let my_variable = 12.0;
  dbg!(my_variable)
  ```

### `parse()`

- This function will parse the variable to different data type.

- We’ll only have to provide the data type to the variable and the `parse()` will call the required parsing function.

- The `parse()` returns a Result instead of parsed variable since it has a chance of failure. For Example, if we try to convert a string into floats which contains alphabets.

- So, we use `unwrap()` along with parse, telling the compiler to stop the execution in case of failure.

  ```rust
  let weight: f32 = input.trim().parse().unwrap();
  ```

  

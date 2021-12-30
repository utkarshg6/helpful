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

### Basic Data Types

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

### Advanced Data Types

- Strings

  - They are of two types:

    - `String` - A smart pointer.
    - `&str` - Also known as string slice is an immutable reference to a part of string.

  - Defining a String

    ```rust
    let string = String::new("127.0.0.1:8080"); // It can grow and shrink
    let string_literal = "1234"; // It's memory is fixed at runtime
    ```

  - Slicing a string

    ```rust
    let string = String::from("127.0.0.1:8080");
    let string_slice = &string[10..14]; // We can also use &string[10..]

    // We can also use
    let string_slice = &string[10..]; // Give me everything after 10th byte not character
    let string_slice = &string[..12]; // Give me everything upto 12th byte not character
    ```

  - Rust uses utf encoding. So, prefer not to not pass integer values for slicing, as the slice function slices on the basis of bytes instead of characters.

    ```rust
    let string = String::from("😀😃😄😁");
    let string_slice = &string[..4];
    ```

    For this slice instead of returning 4 emojis, the rust will return 1 emoji because it takes 4 bytes to store an emoji.

    ```zsh
    string_slice = "😀"
    ```

  - Strings in rust can dynamically grow or shrink.

  - We can borrow an entire string by using this syntax

    ```rust
    let string = String::from("127.0.0.1:8080");
    let string_borrow: &str = &string;
    ```

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

- A macro can be called with a variable number of parameters with different types every time.

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

- Simply put, if a variable passed directly only with it’s name, it is ownership transfer but if we use `&` or `&mut` along with variable name, then it is called borrowing.

  ```rust
  // An example of expecting ownership transfer
  fn some_fn(s : String) {
      ...
  }

  some_fn(my_string);

  // An example of expecting a reference (borrowing)
  fn some_fn(s: &String) {
      ...
  }

  some_fn(&my_string);

  // An example of expecting a *mutable* reference (borrowing)
  fn some_fn(s: &mut String) {
      ...
  }

  some_fn(&mut my_string);
  ```

- The process of passing as a reference is called _borrowing_.

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

## OOPS

### Instance

- Creating a new instance

  ```rust
  let server = Server::new("127.0.0.1:8080");
  ```

- Calling a function of an instance

  ```rust
  server.run();
  ```

### Associated Functions

- Associated functions are those functions that we call directly on the struct rather than on an instance of it.
- We use :: syntax to access the associated functions.
- The `new` keyword is an example of associated functions.

### The `struct`

- It is like classes in other programming language.

- We only declare the variables and their associated data types inside the block of struct.

- We define the functions related to the struct in a different block with keyword `impl`.

  ```rust
  struct Server {
  	// variable_name: data_type
      addr: String,
  }
  ```

### The Struct Functions or Methods

#### Constructor

- Constructors in rust are called `new`

- They are defined under the `impl` block.

  ```rust
  impl Server {
      fn new(addr: String) -> Server {
          Server {
              addr: addr
          }
      }

      ...
  }
  ```

- We can improve the way we write constructors by renaming the struct name with the alias `Self` and passing one word in the case of common variable names.

  ```rust
  impl Server {
      // Notice we replaced Server with Self everywhere
      fn new(addr: String) -> Self {
          Self {
              // We only need to pass *addr* as it is common in
              // constructor argument and in the struct declaration
              addr
          }
      }

      ...
  }
  ```

#### Other Functions

- We can pass `self` as an argument to the functions of an struct.

- The `self` keyword gives the total ownership of the instance of a struct.

- It works similar to the `this` keyword in JavaScript.

- We can also pass `&self` and `&mut self` as an argument to a function.

  ```rust
  impl Server {
      fn new(addr: String) -> Self {
  		...
      }

      fn run(self) {
  		...
      }
  }
  ```

## The `enums`

- It is a special type and has finite number of values.
- The possible number of it’s values are called it’s variants.
- The total size that enum will allocate for it’s variant will be equal to the memory allocation of it’s largest variant. It works similar to unions in C.

### Defining an enum

```rust
enum Method {
    GET,
    DELETE,
    POST,
    PUT,
	...
}
```

### Storing a value in a variable

- Assumption: `::` acts similar to destructuring in javascript.

```rust
let get = Method::GET;
let delete = Method::DELETE;
```

### Controlling the value enums will have

```rust
 enum Method {
      GET, // Value 1
      DELETE, // Value 2
      POST = 5, // Value 5
      PUT, // Value 6
  }
```

### Passing Data Type to Enum

- Passing the data type to enum

  ```rust
   enum Method {
      GET(String),
      ...
  }
  ```

- Declaring enum

  ```rust
  let get = Method::GET("abcd".to_string());
  ```

## Useful Features

### The `Option` enum

- If a possible variable has a possibility case where you don't want to assign it's value, we can use Option there.
- Since, Rust doesn't supports `null` values, so we use this enum to provide an option that it'll either contain a value of none or of it's data type.
- We don't need to import it, it's available everywhere.

```rust
struct Request {
    path: String,
    query_string: Option<String>, // Now query_string can have empty value.
    method: String,
}
```

## Modules

### Defining a module

- We can use `mod` to modularize a chunk of code and give it a scope.
- Everything is private by default inside a module, we need to add `pub` to every `struct`, submodules and `fn` definition to make a module publicly accessible.
- Every file in rust is treated as a module.

```rust
mod server {
    struct Server {
        ...
    }

    impl Server {
        ...
    }
}
```

- Calling a child component

```rust
<module-name>::<name-of-publicly-accessible-tem>

// Example
server::Server::new("127.0.0.1:8080".to_string());
```

- Calling a sibling component (ask parent)

```rust
super::<module-name>::<name-of-publicly-accessible-item>

// Example
super::method::Method
```

## Loops

- Infinite Loop

```rust
// Classical Method

while True {
...
}

// In Rust
loop {
...
}
```

- Labeled Loops and breaking them

```rust
'outer:loop {
	loop {
		break 'outer;
	}
}
```

## Tuples

- Immutable, contains any data type. Also, used to return multiple variables.

```rust
let tuple = (5, "a", listener)

// Receiving multiple variables from a function return
let (stream, addr) = res.unwrap();
```

### Match Expression

- The `match` expression, matches all the possible enums that will come out of a variable.
- For example, we can use it for any result. A result has two enums `Ok()` and `Err()`.
- `match` is not limited to only enums but can also be used for switch case statements.

```rust
match listener.accept() {
    Ok((stream, _)) => {}
    Err(e) => {
        println!("Failed to establish a connection: {}", e)
    }
    // This Below code will match all the remaning enums, similar to default statement in switch cases.
    // _ => {}
}
```

### Arrays

- Mutable, only one data type.

```rust
let a = [1, 2, 3, 4];

// A Function to receive an array

// Method 1
fn arr(a: [u8; 5]) {
    // We need to provide the size beforehand
	// Rust can't handle arbitrary size at runtime
}

arr(a)

// Method 2
fn arr(a: &[u8]) {
    // We now need to prevent the size,
	// since rust knows the size of pointers
}

arr(&a)

// &a is
```

## Run Debugger

- Traverse to `debug` folder

  ```
  cd target/debug
  ```

- Run debugger

  ```
  gdb server
  ```

- Display the code along with line numbers

  ```
  list
  ```

- Set Breakpoints (`b filename:line`)

  ```
  b main.rs:7
  ```

- Running with breakpoints

  ```
  r
  ```

- Getting local variables

  ```
  info locals
  ```

## Advanced Features

- The `unimplemented!()` macro
- We can call it inside any function where we want to suppress errors.
- In Rust the code don’t even compile if it contains error but when this macro is written inside a function which has no code inside of it The code will compile but will cause panic when the code runs and that particular function is called.

### Formatting Tricks

- `{:?}` formats the "next" value passed to a formatting macro, and supports anything that implements Debug.

### Netcat commands

- Pipe some data to the server.

```zsh
echo <String> | netcat <IP Address> <Port>
// echo "TEST" | netcat 127.0.0.1 8080
```

### What is a trait?

- A trait in Rust is a group of methods that are defined for a particular type.
- A trait tells the Rust compiler about functionality a particular type has and can share with other types. We can use traits to define shared behavior in an abstract way. We can use trait bounds to specify that a generic type can be any type that has certain behavior.

### Try From

This is a trait used for type conversions _safely_. That means it is able to handle the possiblity of error while converting between types.

- Importing Try Form trait

```rust
use std::convert::TryFrom;
```

- Using TryFrom

```rust
impl TryFrom<&[u8]> for Request {
    // &[u8] is a byte slice
    // for keyword is used to extend the Request data type
	// This code block coverts byte array into Request.

    // If we implement TryFrom, TryInto automatically gets implemented.
	// ONLY RUST FEATURE (ORF)
    type Error = String;

    fn try_from(buf: &[u8]) -> Result<Self, Self::Error> {
        unimplemented!()
    }
}
```

### Importing Modules and Functions

```rust
// Standard Libraries
use std::io::Read;

// Within Codebase
use crate::http::Request; // here crate means root folder
use server::Server; // Import Server object from file server.rs
use http::Method; // Import Method object from http (could be a file or folder).
```

### Exporting Modules and Functions

```rust
// Define a `mod.rs` file inside the folder of a module

pub use method::Method; // Exposing only enum Method
pub use request::Request; // Exposing only struct Request

pub mod method; // Exposing the whole submodule method
pub mod request; // Exposing the whole submodule request
```

### The `or` function

- This function is used to handle the result and error variables.
- If the result is ok, then it will return the unwrapped result, if error happens, it will return the error passed in the or function.
- There is a question mark in the end, which is a shorthand for match block, if we don’t use qn mark, we’ll have to still use the match block with or fn.

```rust
// Traditional Method
match str::from_utf8(buf) {
    Ok(request) => {},
    Err(_) => return Err(ParseError::InvalidEncoding)
}

// Using or function
str::from_utf8(buf).or(Err(ParseError::InvalidEncoding))?;
```

#### Difference between Question Mark and Match Pattern

- Question Mark will try to convert the error type that it receives, if it does not match the error type, that the function is supposed to return.

### The `ok_or` function

- It is similar to `or` function. The difference is that if the output is not an error, it will first unwrap then wrap again inside a result, which has ok and error.

### Loops

- Rust offers `loop` which, is similar to while loop in python.

```rust
loop {
    // do something iteratively
}
```

- It also offers traditional for loop.

```rust
for c in name.chars() {
    // c variable stores one charater per iteration
}
```

- Enumeration

```rust
for (i, v) in request.chars().enumerate() {
    // i is index, v is variable
}
```

### The ` if let` method

```rust
// Method 1
match path.find('?') {
    Some(i) => {
        query_string = Some(&path[i+1..]);
        path = &path[..i];
    }
    None => {}
};


// Method 2
let q = path.find('?');
if q.is_some() {
    let i = q.unwrap();
    query_string = Some(&path[i+1..]);
    path = &path[..i];
}

// The if let method
if let Some(i) = path.find('?') {
    query_string = Some(&path[i+1..]);
    path = &path[..i];
}
```

### Lifetimes

- Let’s consider we have string slices pointing to some parts of buffer.
- What will happen to those string slices if we _deallocate_ the buffer (free the memory or remove contents of buffer)?
- Basically the string slices will point to the dead memory (or empty memory).
- This is known as **Dangling References** or **Use After Free**.
- Another way to re pharse it is to say, “The string slices has a longer lifetime than a buffer”.
- In, C/C++, Dangling References can happen on runtime.
- In High Level Languages, Garbage Collector prevents Dangling References by not allowing to deallocate the buffer.
- But Rust guarantees no Dangling References, without using a Garbage Collector.
- Rust compiler statically checks all the references in our code and ensure that there is no Dangling References.
- It means that there are no performance penalties unlike high level languages.
- Rust requires us to annotate the Lifetime Specifiers so that the compiler is able to statically check the references.

```rust
// The example of a lifetime

pub struct Request<'a> {
    path: &'a str,
    query_string: Option<&'a str>,
    method: Method,
}
```

- It does not mean that we define the lifetimes of a particular variable. Instead we help the rust compiler to identify that two entities share the same lifetime.

### Runtime Detection of a Function (Dynamic Dispatch)

- A dynamic dispatch is a way to tell rust to identify a correct function for whichever trait the user passes as argument.
- Consider a macro Write, it has it's implementation for Vectors, TcpStream etc.
- Now we want to create a function that is generic, and use the write function implemented by individual trait when we pass the trait.

- It's implementation with dynamic dispatch. (`dyn`).

```rust
pub fn send(&self, stream: &mut dyn Write) -> IoResult<()> {
    let body = match &self.body {
        Some(b) => b,
        None => ""
    };

    write!(
        stream,
        "HTTP/1.1 {} {}\r\n\r\n{}",
        self.status_code,
        self.status_code.reason_phrase(),
        body
    )
}
```

### Compiletime Detection of a Function (Static Dispatch)

- A static dispatch is similar to Dynamic dispatch, the difference is that recognizes the correct function during compile time instead on the runtime.
- It is used with the `impl` keyword.
- It actually reads the code during compile time and then chooses the desired function from the code that we have written.

```rust
pub fn send(&self, stream: &mut impl Write) -> IoResult<()> {
  ...
}
```

### Environment Variables

- The environment variables in rust can be used using a standard library.

```rust
use std::env;

fn main() {
    let public_path = env::var("PUBLIC_PATH").unwrap();
}
```

- There is a macro to retrieve the project's root directory.

```rust
let default_path = env!("CARGO_MANIFEST_DIR");
```

### Piping Expanded code to a file using vscode

- This command helps to save a single expanded file to a new file.

```zsh
rust expand | code -
```


### File System operations

- It converts the contents of a file into a string.

```rust
let path = "path/to/file"
fs::read_to_string(path)
```

### Convert result into option

- Ok()  -> Some()
- Err() -> None

```
fs::read_to_string(path).ok()
```
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

- Rust offers `loop` which is similar to while loop in python.

```rust
loop {
    // do something iteratively
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

  ```zsh
  cd target/debug
  ```

- Run debugger

  ```zsh
  gdb server
  ```

- Display the code along with line numbers

  ```zsh
  list
  ```

- Set Breakpoints (`b filename:line`)

  ```zsh
  b main.rs:7
  ```

- Running with breakpoints

  ```zsh
  r
  ```

- Getting local variables

  ```zsh
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

### The `if let` method

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

- Ok() -> Some()
- Err() -> None

```rust
fs::read_to_string(path).ok()
```

### New Learning Concepts

- Assigning a value to a variable is called _binding_.
- At default, bindings are immutable (not changeable).
- To make bindings immutable we need to add `mut` keyword before variable name.
- Rust uses curly braces `({})` to define a scope. A binding defined within a scope can't escape from it.

```rust
let a = 1;
dbg!(a); // 1
{
    // Here, we re-bind `a` to a new value, which is still immutable.
    // This technique is called _shadowing_. The new binding is constrained to
    // this anonymous scope. Outside this scope, the previous binding still
    // applies.
    let a = 2;
    let b = 3;
    dbg!(a, b); // 2, 3
}
// can't use `b` anymore because it is out of scope
// dbg!(b);

// The shadowed `a` in the inner scope above has fallen out of scope,
// leaving us with our original binding.
dbg!(a); // 1
```

- Outer doc comments are formed with the keyword ///, which acts identically to the // keyword. They apply to the item which follows them, such as a function:

```rust
/// The `add` function produces the sum of its arguments.
fn add(x: i32, y: i32) -> i32 { x + y }
```

- Inner doc comments are formed with the keyword //!, which acts identically to the // keyword. They apply to the item enclosing them, such as a module:

```rust
mod my_cool_module {
    //! This module is the bee's knees.
}
```

### enums

- Enums, short for enumerations, are a type that limits all possible values of some data.
- The possible values of an enum are called variants.
- Enums also work well with match and other control flow operators to help you express intent in your Rust programs.

### structs

- structs are same as classes in python.
- It is often useful to group a collection of items together, and handle those groups as units. In Rust, we call such a group a struct, and each item one of the struct's fields.
- A struct defines the general set of fields available, but a particular example of a struct is called an instance.
- Structs come in three flavors: structs with named fields, tuple structs, and unit structs.

### crates

- In rust, packages are called crates.

### tuples

- Tuples are a lightweight way to group a fixed set of arbitrary types of data together.
- A tuple doesn't have a particular name; naming a data structure turns it into a struct.
- A tuple's fields don't have names; they are accessed by means of destructuring or by position.

```rust
// pointless but legal
let unit = ();
// single element
let single_element = ("note the comma",);
// two element
let two_element = (123, "elements can be of differing types");

```

- Access by destructuring

```rust
let (elem1, _elem2) = two_element;
assert_eq!(elem1, 123);
```

- Access by position

```rust
let notation = single_element.0;
assert_eq!(notation, "note the comma");
```

- Tuple Structs

- Like normal structs, these are named types; unlike
  normal structs, they have anonymous fields.
- Their syntax is very similar to normal tuple syntax. It is
  legal to use both destructuring and positional access.

```rust
struct TupleStruct(u8, i32);
let my_tuple_struct = TupleStruct(123, -321);
let neg = my_tuple_struct.1;
let TupleStruct(byte, _) = my_tuple_struct;
assert_eq!(neg, -321);
assert_eq!(byte, 123);
```

### Field Visibility

- All fields of anonymous tuples are always public.
- However, fields of tuple structs have individual
  visibility which defaults to private, just like fields of standard structs.
- You can make the fields
  public with the `pub` modifier, just as in a standard struct.

```rust
// fails due to private fields
mod tuple {
  pub struct TupleStruct(u8, i32);
}

fn main() {
  let _my_tuple_struct = tuple::TupleStruct(123, -321);
}
```

```rust
// succeeds: fields are public
mod tuple {
  pub struct TupleStruct(pub u8, pub i32);
}

fn main() {
  let _my_tuple_struct = tuple::TupleStruct(123, -321);
}
```

### The Spread Operator (..)

- Creating Instances From Other Instances With Struct Update Syntax.
- It’s often useful to create a new instance of a struct that uses most of an old instance’s values but changes some. You can do this using _struct update syntax_.

- Initially:

```rust
    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    };
```

- After:

```rust
    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
```

### Iterator, Enumerator, Map, Filter

- Rust has the above functions inside the Iterator Trait.
- It is also possible to use them together.

```rust
  let iterator = iter.enumerate()
                     .filter(|(i, val)| i % 2 == 0)
                     .map(|(i, val)| val)

```

### Adding elements with their frequency to a HashMap using entry

- You can follow this [link](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.entry).

```rust
use std::collections::HashMap;

let mut letters = HashMap::new();

for ch in "a short treatise on fungi".chars() {
    let counter = letters.entry(ch).or_insert(0);
    *counter += 1;
}

assert_eq!(letters[&'s'], 2);
assert_eq!(letters[&'t'], 3);
assert_eq!(letters[&'u'], 1);
assert_eq!(letters.get(&'y'), None);
```

### Attributes

- Attributes are metadata about pieces of Rust code.

```rust
// Syntax
// InnerAttribute :
   # ! [ Attr ]

// OuterAttribute :
   # [ Attr ]
```

#### The `derive` attribute

- It is applied to `struct` or `enum`.

## Rust Tests

Fun Fact: In Rust, each test is run in a new thread, and when the main thread sees that a test thread has died, the test is marked as failed.

- You can create a boilerplate rust project that automatically has tests using the following commmand:

  ```zsh
  cargo new <project-name> --lib
  ```

- A `test` in Rust is a function that’s annotated with the test attribute.
- The bodies of test functions typically perform these three actions:

  1. Set up any needed data or state.
  2. Run the code you want to test.
  3. Assert the results are what you expect.

- Two attributes to keep in mind:

  1. `#[test]` - To change a function into a test function, add `#[test]` on the line before `fn`.
  2. `#[should_panic]` - To declare before each test function that if this function panics then it is working correctly.

- After adding the attribute `#[test]`, the rust compiler is ready to run `cargo test`.

- When you run the command, behind the scenes Rust builds a test runner binary that runs the functions annotated with the `test` attribute.

- You can write the functions that are not tests inside the tests module, for example a helper function. So, the only way for the Rust to know whether a function is a test function is through the `#[test]` attribute.

- Here is the table for the logging statistics:

  | Statistic    | Meaning                                                                      |
  | ------------ | ---------------------------------------------------------------------------- |
  | Passed       | Passing Tests                                                                |
  | Failed       | Failing Tests                                                                |
  | Ignored      | Tests that were ignored due to `#[ignore]` attribute.                        |
  | Measured     | This is for benchmark tests that measure performance. (only in nightly Rust) |
  | Filtered Out | While running specific tests, the left out tests are called `filtered`.      |

- The next part, `Doc-tests <project-name>`, is for the reults of any documentation tests.
- The tests fail when something in the test function panics.

- Here is the table for the macros you mauy use for assertion:

  | Assertion Macro | Use Case                                          | Argument(s)            |
  | --------------- | ------------------------------------------------- | ---------------------- |
  | `assert!()`     | If the condition is true then passes else panics. | Condition              |
  | `panic!()`      | Panics or fails the test with a message if given. | Message                |
  | `assert_eq!()`  | Passes if equal else panics. (`==`)               | (actual, expected)     |
  | `assert_ne!()`  | Passes if not equal else panics. (`!=`)           | (actual, not_expected) |

- In rust the convention doesn't matter, we can either use actual as first argument or as second. It is the programmer's convention.

- In case we are writing the tests in a module inside the same file then we'll need to use the `super::*;` inside the tests module to pull all the outside code of the current file.

```rust
//Filename: src/lib.rs
fn do_something() {
  ...
}


#[cfg(test)]
mod tests {
    // The line below will pull all the code of outer module inside.
    use super::*;

    #[test]
    fn test_do_something() {
      ...
    }
}
```

- For structs and enums that you define, you’ll need to implement `PartialEq` to assert that values of those types are equal or not equal.

  ```rust
  #[derive(PartialEq, Debug)]
  struct Rectangle {
      width: u32,
      height: u32,
  }

  #[cfg(test)]
  mod tests {
      use super::*;

      // This test will only work if we'll add #[derive(PartialEq)] to the struct or enum.
      #[test]
      fn rectangle_is_of_same_size() {
          let rectangle1 = Rectangle {
              width: 8,
              height: 7,
          };
          let rectangle2 = Rectangle {
              width: 8,
              height: 7,
          };

          assert_eq!(rectangle1, rectangle2);
      }
  }
  ```

- You’ll need to implement `Debug` to the struct or enum if you want to see the logs that say `(left != right)`.

  ```zsh
  ---- tests::rectangle_is_of_same_size stdout ----
  thread 'tests::rectangle_is_of_same_size' panicked at 'assertion failed: `(left == right)`
    left: `Rectangle { width: 8, height: 7 }`,
   right: `Rectangle { width: 8, height: 8 }`', src/lib.rs:56:9
  ```

- The `assert!()` macro also allows the message to show in case the test fails.

  ```rust
  assert!(
    result.contains("something"),
    "The result doesn't contain something. This was the actual result: {}",
    result
  )
  ```

- There is an attribute named `#[should_panic]`, that you can write before any test function to declare that if this function panics then it is working correctly.

  ```rust
  #[cfg(test)]
  mod tests {
      use super::*;

      #[test]
      #[should_panic]
      fn greater_than_100() {
          Guess::new(200);
      }
  }
  ```

- This is the note that appears in case the function doesn't panics.

  ```zsh
  note: test did not panic as expected
  ```

- To make the `![should_panic]` attribute more precise we can add the `expected` parameter and pass a string to it such that the string is a substring of the relevant panic message.

  ```rust
  impl Guess {
      pub fn new(value: i32) -> Guess {
          if value < 1 {
              panic!(
                  "Guess value must be greater than or equal to 1, got {}.",
                  value
              );
          } else if value > 100 {
              panic!(
                  "Guess value must be less than or equal to 100, got {}.",
                  value
              );
          }

          Guess { value }
      }
  }

  #[cfg(test)]
  mod tests {
      use super::*;

      // The below test only passes if both the two conditions satisfies:
      // 1. Code should panic
      // 2. The string passed in expected parameter is a substring of the panic message.
      #[test]
      #[should_panic(expected = "Guess value must be less than or equal to 100")]
      fn greater_than_100() {
          Guess::new(200);
      }
  }
  ```

- There is also an alternative approach possible to use `Ok()` and `Err()` inside a test.

  ```rust
  #[cfg(test)]
  mod tests {
      #[test]
      fn it_works() -> Result<(), String> {
          if 2 + 2 == 4 {
              Ok(())
          } else {
              Err(String::from("two plus two does not equal four"))
          }
      }
  }
  ```

- Pros: The only upside of writing tests such that they return a `Result<T, E>` enables you to use the question mark (`?`) operator in the body of tests, which can be a convenient way to write tests that should fail if any operation within them returns an Err variant.

- Cons: If we write tests in above manner than we cannot use `#[should_panic]` attribute because we can use `Err()`.

### Controlling Tests

- Command Line Options either go to:

  1. `cargo test` - Run this command for help, `cargo test --help`
  2. The resulting test binary - Run this command for help, `cargo test -- --help`.

- In simple terms, if you want to pass something into tests binary you'll have to seperate them with `--`.

#### Threading while running Tests

- By default, Rust runs tests in different threads.

- In case the test depends on each other. For example, tests perform read, write, and assert to a same file. If run concurrently, the file will get corrupt and the tests might fail.

- To run tests on a single thread, one by one we can do it by:

  ```zsh
  cargo test -- --test-threads=1
  ```

- This command will prevent parallelism, though the tests will take longer to run but it'll save us from the above problem.

#### Printing while running tests

- By default, when we run `cargo test`, the compiler never prints the statements.

- To display print statements we can run this command:

  ```zsh
  cargo test -- --show-output
  ```

#### Running Specific Tests

- To run specific tests either 1 or more, we can do that by specifying the name of test.

  - For One Test:

    ```zsh
    cargo test <name-of-test>
    ```

  - For more than one test (e.g. add_two, add_three, then substring add):

    ```zsh
    cargo test <substring-of-tests-name>
    ```

- When we run specific tests, the remaining tests will fall into the category of `filtered out`, you may see that in the logs.

- Also note that the module in which a test appears becomes part of the test’s name, so we can run all the tests in a module by filtering on the module’s name.

#### Ignoring Tests

- Instead of filtering tests, you may like to ignore the tests by adding `#[ignore]` attribute above each test that you'd like to ignore.

- Now, you may use the following commands to run the tests.

  - To run the tests ignoring ones that use `#[ignore]` attribute:

    ```zsh
    cargo test
    ```

  - To run only the ignored tests:

    ```zsh
    cargo test -- --ignored
    ```

  - To run both ignored and not ignored tests:

    ```zsh
    cargo test -- --include-ignored
    ```

### Test Organization

- The Rust community thinks about tests in terms of two main categories **Unit Tests** and **Integration tests**.

| Unit Tests                                                     | Integration tests                                               |
| -------------------------------------------------------------- | --------------------------------------------------------------- |
| Small and Focused                                              | Large and Broad                                                 |
| Internal                                                       | External                                                        |
| Tests One Module                                               | Tests Multiple Modules                                          |
| Can Test Private Interfaces                                    | Only Tests Public Interfaces                                    |
| Testing Internally such that external code may not possibly do | Testing like some external code would do                        |
| Lives inside `src` directory inside each module                | Lives in `tests` directory right outside `src` directory        |
| Module named tests inside each module with `#[cfg(test)]`      | Different files inside `tests` directory without `#[cfg(test)]` |

#### The Tests Module and #[cfg(test)]

- This annotation tells Rust to only run this module on `cargo test`, not when you run `cargo build`.
- Thus, the functions following this annotation are never part of compiled result, hereby saving some space.
- Only used for unit tests, since integration tests are different directory they don't need to use it.
- An example worth noting:

  ```rust
  // Both code and unit test lives in the same file.

  pub fn public_fn() {
    ...
  }

  fn private_fn() {
    ...
  }

  // cfg stands for configuration
  #[cfg(test)]
  mod tests {
    // You'll use this line to pull code from outside this module but inside this file.
    use super::*;

    // A helper function
    fn helper() {
      ...
    }

    // Only this fn will be considered test, unlike above fn
    #[test]
    fn it_works() {
        // Can test both private and public functions.
        ...
    }
  }
  ```

#### Integration tests

- For integration test, we create a new directory `target` beside the `src` directory.
- Each file in the tests directory is compiled as its own separate crate.
- Treating each integration test file as its own crate is useful to create separate scopes that are more like the way end users will be using your crate.
- However, this means files in the tests directory don’t share the same behavior as files in src do.
- The file structure of integration tests are:

  ```rust
  rust_project
  ├── src
  |  └── lib.rs
  ├── target
  |  ├── ...
  |  └── ...
  ├── tests
  |  ├── common
  |  |  └── mod.rs // contains helper functions for tests
  |  └── integration_test.rs // contains integration tests
  ├── Cargo.lock
  └── Cargo.toml
  ```

- The helper functions lives inside file `tests/common/mod.rs`.
- This is a naming convention that rust uses to prevent functions inside this file not to appear in output logs of tests.
- Files in subdirectories of the tests directory don’t get compiled as separate crates or have sections in the test output.
- It looks something like this:

  ```rust
  pub fn setup() {
      // setup code specific to your library's tests would go here
  }
  ```

- The `integration_test.rs` looks similar to this:

  ```rust
  // Each file in the tests directory is a separate crate, so we need to bring our library into each test crate’s scope
  use adder;

  // Bring the common functions
  mod common;

  // No need to add `#[cfg(test)]` attribute, since we are in the tests directory.

  #[test]
  fn it_adds_two() {
      common::setup();
      assert_eq!(4, adder::add_two(2));
  }
  ```

- To run all the tests in a particular integration test file, use:

  ```zsh
  cargo test --test <integration-test-filename>
  ```

- We can't write integration tests for binary crates, the rust projects that only contains a `src/main.rs` file and doesn’t have a `src/lib.rs` file.
- The reason is that we cannot bring functions defined in the `src/main.rs` file into scope of files in `tests` directory with a use statement.
- Only library crates expose functions that other crates can use; binary crates are meant to be run on their own.
- Though, if a project contains both `src/lib.rs` and `sr/main.rs`, we can write integration tests for the important functionality inside `src/lib.rs` using the `use` keyword.
- If the important functionality works, the small amount of code in the `src/main.rs` file will work as well, and that small amount of code doesn’t need to be tested.

### Tests Output Log

- The output logs section has three parts:
  1. Unit Tests
  2. Integration Tests
  3. Doc Tests

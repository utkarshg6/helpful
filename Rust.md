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
  - It uses Package Manager named _Cargo_.
- No Data Race
  - A data race occurs when:
    - two or more threads in a **single process** access the same memory location concurrently, and
    - at least one of the accesses is for writing, and
    - the threads are not using any exclusive locks to control their accesses to that memory
  - Game Changer for writing Asynchronous Code

## The `"Hello, World!"` program

Fun Fact: Rust is an ahead-of-time compiled language, meaning you can compile a program and give the executable to someone else, and they can run it even without having Rust installed.

- After [installing Rust](#installation), you may follow the following steps.

- Create a project folder, cd into it and create `main.rs` file:

  ```zsh
  mkdir hello_world
  cd hello_world
  touch main.rs
  ```

- Add the following program inside of it:

  ```rust
  // Filename: main.rs
  fn main() {
      println!("Hello, world!");
  }
  ```

- Some facts regarding the above code.

  - Main function is the first function that gets called.
  - `println!()` is not a function but a _macro_.
  - Macros contain an `!` mark.

- Compile and run the file:

  ```zsh
  rustc main.rs
  ```

  - For Linux and macOS:

    ```zsh
    ./main
    ```

  - For Windows:

    ```zsh
    .\main
    ```

- Alternatively, you may use the package manager [Cargo](#cargo) to create new boilerplate projects.

## Variables and Mutability

### Variables

- By default variables are immutable in Rust.
- Its advantages includes memory safety and easy concurrency.

- Example of immutable variable:

  ```rust
  let x = 5;
  ```

- Example of mutable variable:

  ```rust
  let mut x = 5;
  x = 6;
  ```

### Constants

- First, you aren’t allowed to use mut with constants.
- Constants aren’t just immutable by default—they’re always immutable.
- Constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.
- Constants may be set only to a constant expression, not the result of a value that could only be computed at runtime.

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

- Rust’s naming convention for constants is to use all uppercase with underscores between words.
- See the [Rust Reference’s section on constant evaluation](https://doc.rust-lang.org/reference/const_eval.html) for more information on what operations can be used when declaring constants.

### Shadowing

- You can declare a new variable with the same name as a previous variable, it's called _shadowing_.

  ```rust
  fn main() {
      let x = 5; // Binding x to value 5

      let x = x + 1; // Declaring a variable named x again, thereby performing shadowing

      {
          let x = x * 2; // Shadowing x again, but within the scope
          println!("The value of x in the inner scope is: {}", x);
      }

      println!("The value of x is: {}", x);
  }

  // Output:
  // The value of x in the inner scope is: 12
  // The value of x is: 6
  ```

- In shadowing, we can make a few transformations on a value but have the variable be immutable, unlike `let mut`.

- You can perform type conversions and still keep the same name.

  ```rust
  // Shadowing compiles without errors
  let spaces = "   ";
  let spaces = spaces.len();

  // It's not possible to change type of a mutable Variables
  let mut spaces = "   ";
  spaces = spaces.len(); // This line won't compile
  ```

### Shadowing Vs Mutable Variables

| Shadowing                                                              | Mutable Variables                                           |
| ---------------------------------------------------------------------- | ----------------------------------------------------------- |
| Transform a variable but still keep it as immutable.                   | We only make transformations after making variable mutable. |
| We need to declare variable with `let` everytime we perform shadowing. | We need to declare variable with only `let mut` once.       |
| It is possible to change the type of variable and keep the same name.  | It is not possible to change the type of mutable variable.  |

## Data Types

- Every value in Rust is of a certain _data type_, that makes Rust a _statically typed language_.
- It means Rust needs to know the type of all variables at compile time.
- Rust data types has two subsets:
  1. Scalar
  2. Compound

### Scalar Data Types

- Booleans
  - Represented by `bool`, has two values `true` and `false`.
- Characters
  - Represented by `char`, it always has space of `4` bytes or `32` bits instead of 1 byte.
  - Characters are UTF-8 encoded, thus supports `'z', 'ℤ', '😻'`.
  - Characters use single quotes and string uses double quotes.
- Integers

  - `u` means unsigned (only positive), `i` means signed (both positive & negative)
  - The size ranges from `8` bits to `128` bits.
  - Range of Unsigned Integers is:
    $$ [0, 2^n - 1] $$
  - Range of Signed Integer is:
    $$ [- 2^{n-1}, 2^{n-1} - 1]$$
  - Examples, `u8`, `i8`, `u128`, `i128`
  - Rust also supports, `usize` and `isize` which means that it will take up space according to the architecture whether it is 32 bit or 64 bit.
  - You may also represent integer literals in the below forms:
    | Number literals | Example | Integer |
    | ----------- | ----------- | ----------- |
    | Decimal | 98_222 | 98222 |
    | Hex | 0xff | 255 |
    | Octal | 0o77 | 63 |
    | Binary | 0b1111_0000 | 240 |
    | Byte (u8 only) | b'A' | 65 |
  - Integer types default to `i32`.
  - To read how Interger Overflow works in Rust, please follow [this link](https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-overflow).
  - Division of integers gives floored value, `3 / 2 == 1`.

- Floats
  - It has `f32` and `f64`, two floating data types for size `32` and `64`.
  - Floating types default to `f64`.
  - Division of floats give fractional result, `3.0 / 2. 0 = 1.5`.

### Compound Data Types

- There are two compound data types in Rust:

  1. Tuples
  2. Arrays

#### Tuples

- They can store number of values with different data types.
- They can't grow or shrink once declared.
- Tuples can be declared as follows:

  ```rust
  let tup_with_types: (i32, f64, u8) = (500, 6.4, 1);
  let tup = (500, 6.4, 1);

  // Destructuring Tuples
  let (x, y, z) = tup;

  // Destructuing Tuples using .
  let x: (i32, f64, u8) = (500, 6.4, 1);
  let five_hundred = x.0;
  let six_point_four = x.1;
  let one = x.2;

  // Unit type tuple with unit value
  let unit_tup = ();
  ```

#### Array

- They can store number of values with same data type.
- They can't grow or shrink once declared as their memory is allocated on stack.
- If you want a similar data structure that can grow or shrink then use Vectors.
- If you access an index of array that is greater than it's length, it'll result in `'index out of bounds'`.
- In other low level languages, this check is not done and they return an invalid memory.
- Arrays can be declared as follows:

  ```rust
  // Simple array declaration
  let a = [1, 2, 3, 4, 5];

  // Declaring array with type and size
  let a: [i32; 5] = [1, 2, 3, 4, 5];

  // Declaring same value 3 for 5 elements
  let a = [3; 5];

  // Accessing Values of array
  let a = [1, 2, 3, 4, 5];
  let first = a[0];
  let second = a[1];
  ```

#### Slicing an array

- It is possible to slice an array in Rust.

  ```rust
  let a = [1, 2, 3, 4, 5];

  let slice = &a[1..3]; // It is of type &[i32]
  ```

#### Ranges

- You can create a range with `..` operator.
- The following are equal:
  - `1..5` ~ `1..=4`
  - `0..4` ~ `..4`
  - `1..len` ~ `1..`
  - `0..len` ~ `..`

### Advanced Data Types

#### Strings

- They are represented in three types:

  - `String` - A smart pointer.
  - `&String` - Reference to a String.
  - `&str` - String Slice

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

  - Rust uses UTF-8 encoding. So, prefer not to not pass integer values for slicing, as the slice function slices on the basis of bytes instead of characters.

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

- This is how `let s1 = String::from("hello");` is stored in Rust:

  - Left - Parts of String that are stored on the stack:
    1. Pointer: Points to the memory that holds the contents of the string.
    2. Length: How much memory, in bytes, the contents of the String is currently using.
    3. Capacity: The total amount of memory, in bytes, that the String has received from the allocator.
  - Right - The memory on the heap that holds the contents.

  ![Image](https://doc.rust-lang.org/book/img/trpl04-01.svg)

## String Slicing

- Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.
- A slice is a kind of reference, so it does not have ownership.
- Slices are represented by `&str` and are _immutable_.

  ```rust
      let s = String::from("hello world");

      let hello = &s[0..5]; // or you can use &s[..5]
      let world = &s[6..11];
  ```

- This will throw compile-time error:

  ```rust
  // FAIL: You cannot clear the memory, to which some reference already exists
  fn main() {
      let mut s = String::from("hello world");

      let word = first_word(&s); // Returns a &str, which is a referenc to s

      s.clear(); // error!

      println!("the first word is: {}", word);
  }
  ```

- Thus, String Slices helps us write secure code by protecting the references to a string.
- Also string literals `let string_literal = "hello";`, are string slices `&str`, and are immutable.

Note: It is expected that the String only contains ASCII characters, because in case of UTF-8, if we try to slice between a multibyte character, it'll cause an error.

- The correct way to use referencing is discussed int the section, ["Which is better `&String` or `&str`?"](#which-is-better-string-or-str).

- Difference between String, String Literal, String Slice

  | Property          | String                                      | String Literal                 | String Slice                       | Reference to String              |
  | ----------------- | ------------------------------------------- | ------------------------------ | ---------------------------------- | -------------------------------- |
  | Definition        | `let string = String::from("some_string");` | `let string_literal = "1234";` | `let string_slice = &string[1..3]` | `let string_reference = &string` |
  | Representation    | `String`                                    | `&str`                         | `&str`                             | `&String`                        |
  | Mutable           | :white_check_mark:                          | :x:                            | :x:                                | :x:                              |
  | Memory Management | Heap (but deallocates when out of scope)    | Heap (Points to binary)        | Heap (Points to Binary)            | Heap                             |
  | Use Cases         | Taking Input, or any String Manipulation    | Defining Constant Strings      | Slicing and Borrowing              | Borrowing                        |

## Functions

- We define a function in Rust by entering `fn` followed by a function name and a set of parentheses.
- The curly brackets tell the compiler where the function body begins and ends.
- The entry function to a rust's code is the `main` function.
- Rust doesn’t care where you define your functions, only that they’re defined somewhere.
- Here's an example of functions in rust:

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}

// Function with Parameter (or argument)
fn function_with_parameters(x: i32) {
    println!("The value of x is: {}", x);
}

// Function with two parameters
fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {}{}", value, unit_label);
}

// Functions with a return value
// In rust, functions return last expression implicitly
fn five() -> i32 {
    5 // An Expression
}

// Functions returning through classical return keyword
// We use return keyword when we need to return early from a function
fn five() -> i32 {
    return 5; // A statement
}

// This will also work
fn plus_one(x: i32) -> i32 {
    x + 1
}

// Fail: Statement means, this function returns anything, expressed by (), a unit type
fn plus_one(x: i32) -> i32 {
    x + 1;
}
```

### Statements and Expressions

- In Rust, function bodies are made up of a series of statements optionally ending in an expression.

#### Statements

- They are instructions that perform some action and do not return a value.
- They are just a standalone unit of execution.
- Creating a variable and assigning itself a value is a statement.

  ```rust
  let y = 6;
  ```

- Function definitions are also statements; the entire function is a statement in itself.

  ```rust
  fn main() {
      let y = 6;
  }
  ```

- Statements do not return values. Therefore, you can’t assign a let statement to another variable:

  ```rust
  fn main() {
      let x = (let y = 6); // FAIL : Statements doesn't return anything
  }
  ```

- In some languages, you can write `x = y = 6` and have both `x` and `y` have the value `6`; that is **not the case** in Rust.

#### Expressions

- They do not end with a semicolon, unlike statements.
- They evaluate into a resulting value.
- They are a combination of values and functions that are combined by the compiler to create a new value.
- The following things are considered as an expression:
  1. A simple math operation
  2. Calling a function
  3. Calling a macro
  4. A new scope block created with curly brackets
- A simple math operation is an expression:

  ```rust
  5 + 6
  ```

- In the below statement the standalone `6` is an expression.

  ```rust
  let x = 6; // A statement containing an expression
  ```

- A scope block is an expression:

  ```rust
  {
      let x = 3;
      x + 1
  }
  ```

- The above scope block will return `4` and can now become a part of a statement.

  ```rust
  fn main() {
      // The below statement contains the computed value of an expression
      let y = {
          let x = 3; // Statements end with a semicolon
          x + 1 // Expressions do not end with semicolon
      };

      println!("The value of y is: {}", y);
  }
  ```

## Control Flow

### `if` expressions

- The code inside an `if` block is called an arm, similar to match.

  ```rust
  fn main() {
      let number = 3;

      if number < 5 {
          println!("condition was true"); // An arm
      } else {
          println!("condition was false");
      }
  }
  ```

- You can only pass a `bool` to the `if` expression

  ```rust
  // FAIL: number is of type integer and not bool
  fn main() {
      let number = 3;

      if number {
          println!("number was three");
      }
  }

  // This works since it's a condition
  fn main() {
      let number = 3;

      if number != 0 {
          println!("number was something other than zero");
      }
  }
  ```

- Rust only executes the block for the first true condition, and once it finds one, it doesn't even check the rest:

  ```rust
  fn main() {
      let number = 6;

      if number % 4 == 0 {
          println!("number is divisible by 4");
      } else if number % 3 == 0 {
          println!("number is divisible by 3"); // Only this statement will run
      } else if number % 2 == 0 {
          println!("number is divisible by 2");
      } else {
          println!("number is not divisible by 4, 3, or 2");
      }
  }
  ```

- Conditionals in Single Line:

  ```rust
  // This Works
  fn main() {
      let condition = true;
      let number = if condition { 5 } else { 6 };

      println!("The value of number is: {}", number);
  }

  // FAIL: Different data types integer and string
  fn main() {
    let condition = true;

    let number = if condition { 5 } else { "six" };

    println!("The value of number is: {}", number);
  }
  ```

### Loops

- Rust has three kinds of loops:

  1. `loop` - Infinite Loop, uses `break` and `continue`
  2. `while` - Breaks when the condition doesn't meet.
  3. `for` - Faster and easier to use for iterators and classical for loops.

- Simple Infinite Loop:

  ```rust
  loop {
    // Do something iteratively
  }
  ```

- Named Loop

  ```rust
  'outer:loop {
   loop {
    break 'outer;
   }
  }
  ```

- Named Loop with different breaks:

  ```rust
  'oulter_loop: loop {
    loop {
      if condition {
        break 'oulter_loop; // Breaks Outer Loop
      }

      if some_other_condition {
        break; // Breaks Inner Loop
      }
    }
  }
  ```

- Returning values in loops:

  ```rust
  fn main() {
      let mut counter = 0;

      let result = loop {
          counter += 1;

          if counter == 10 {
              break counter * 2;
          }
      };

      println!("The result is {}", result);
  }
  ```

- The `while` loop:

  ```rust
  fn main() {
      let mut number = 3;

      // Prevents the use of break, by including the condition with while
      while number != 0 {
          println!("{}!", number);

          number -= 1;
      }

      println!("LIFTOFF!!!");
  }
  ```

- The `for` loop:

  ```rust
  // Last item in exclusive, or 0..10 === 0..=9
  for x in 0..10 {
      println!("{}", x); // x: i32
  }
  ```

- The `for` loop for iterator:

  ```rust
  fn main() {
      let a = [10, 20, 30, 40, 50];

      for element in a {
          println!("the value is: {}", element);
      }
  }
  ```

- A for loop for iterating characters in String

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

- The `for` loop in reverse:

  ```rust
  fn main() {
      for number in (1..4).rev() {
          println!("{}!", number);
      }
      println!("LIFTOFF!!!");
  }

  // Output:
  // 3!
  // 2!
  // 1!
  // LIFTOFF!!!
  ```

## Ownership

- _Ownership_ is a set of rules that governs how a Rust program manages memory.
- Some languages have garbage collection that constantly looks for no-longer used memory as the program runs; in other languages, the programmer must explicitly allocate and free the memory.
- Rust uses a third approach: memory is managed through a system of ownership with a set of rules that the compiler checks.
- If any of the rules are violated, the program won’t compile.
- None of the features of ownership will slow down your program while it’s running.

### The stack and the heap

- Both the stack and the heap are parts of memory available to your code to use at runtime

- Differences between Stack and Heap

| Stack                                                          | Heap                                                                                       |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Value is stored in order _Last In, First Out_.                 | Value is stored at an empty spot and a _pointer_ is returned.                              |
| More Organized                                                 | Less Organized                                                                             |
| Operations are _push_ and _pop_.                               | Process of storing data on heap is called allocating.                                      |
| All stored data is of fixed size.                              | Stored data can be of dynamic size.                                                        |
| New items are stored on top of stack, hence pushing is faster. | New items are stored after searching for right place to store, hence allocating is slower. |

- Pushing values onto the stack is not considered allocating. Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack, but when you want the actual data, you must follow the pointer.
- Think of heap as being seated at a restaurant.

  - When you enter, you state the number of people in your group, and the staff finds an empty table that fits everyone and leads you there. If someone in your group comes late, they can ask where you’ve been seated to find you.
  - Consider a server at a restaurant taking orders from many tables. It’s most efficient to get all the orders at one table before moving on to the next table. Taking an order from table A, then an order from table B, then one from A again, and then one from B again would be a much slower process. By the same token, a processor can do its job better if it works on data that’s close to other data (as it is on the stack) rather than farther away (as it can be on the heap). Allocating a large amount of space on the heap can also take time.

- When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap) and the function’s local variables get pushed onto the stack. When the function is over, those values get popped off the stack.

### Ownership Rules

- These are three golden rules of ownership:

  1. Each value in Rust has a variable that’s called its _owner_.
  2. There can only be one owner at a time.
  3. When the owner goes out of scope, the value will be dropped.

### What is `moved`?

- This is not a move, it's a copy:

```rust
// y = x is a copy: Integers are simple values with a known, fixed size, pushed to stack, hence copied
let x = 5;
let y = x;
```

- This is a move:

```rust
// s1 = s2 is a move: Strings are stored on heap, hence only the data of string stored on stack is copied, hence moved
let s1 = String::from("hello");
let s2 = s1;
```

- Why `String` is only moved and not copied?

  - When we assign s1 to s2, the String data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack.
  - We do not copy the data on the heap that the pointer refers to.
  - Hence it is moved not copied.

  ![Move of String](https://doc.rust-lang.org/book/img/trpl04-02.svg)

- Why Rust preferes moving instead of copying the heap data?

  - If Rust preformed copy instead of move, the operation `s2 = s1` could be very expensive in terms of runtime performance if the data on the heap were large.
  - This is what it would look like if Rust would have copied instead of moved.

  ![If String was Copied](https://doc.rust-lang.org/book/img/trpl04-03.svg)

- How does Rust clean memory after we perform `s2 = s1` and both `s1` and `s2` go out of scope?

  - When a variable saved on heap goes out of scope, Rust calls a `drop` function to clean it from the memory.
  - But we performed `let s2 = s1;`, so Rust will try to clean both `s1` and `s2`, cleaning the same memory.
  - This problem is called _double free_ error.
  - To solve this problem, when we perform `let s2 = s1;`, Rust actually moves the value to `s2` by invalidating `s1`.
  - Now, Rust has to only clean the `s2` variable. Hence, the problem of double free is solved.
  - So, what may look like a shallow copy (refer shallow and deep copy in other languages), it is actually a move operation.
    ![Invalidation of s1](https://doc.rust-lang.org/book/img/trpl04-04.svg)

- In addition, there’s a design choice that’s implied by this: Rust will never automatically create “deep” copies of your data.
- Therefore, any automatic copying can be assumed to be inexpensive in terms of runtime performance.

### Clone

- This function is used when we want to clone the heap data.
- If we perform `s2 = s1`, on a heap data for example String, then only the stack data will be copied and not the heap data, hence moved.
- In case we want to copy the heap data too (also referred to deep copy), we use _clone_ function.
- Here's an example:

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```

### The `Copy` and `Drop` trait

- `Copy` trait can be placed on types that are stored on the stack like integers are.

- If a type implements the Copy trait, a variable is still valid after assignment to another variable.
- Rust won’t let us annotate a type with Copy if the type, or any of its parts, has implemented the Drop trait.
- If the type needs something special to happen when the value goes out of scope and we add the Copy annotation to that type, we’ll get a compile-time error.
- Types that implement copy:
  1. All the integer types, such as u32.
  2. The Boolean type, bool, with values true and false.
  3. All the floating point types, such as f64.
  4. The character type, char.
  5. Tuples, if they only contain types that also implement Copy. For example, (i32, i32) implements Copy, but (i32, String) does not.

### Ownership and Functions

- Passing a variable to a function will move or copy, just as assignment does.

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

- This is how the ownership works for the return values:

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return value into s1
    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into takes_and_gives_back, which also moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String { // gives_ownership will move its return value into the function that calls it
    let some_string = String::from("yours"); // some_string comes into scope

    some_string // some_string is returned and moves out to the calling function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into scope
    a_string  // a_string is returned and moves out to the calling function
}
```

- When a variable that includes data on the heap goes out of scope, the value will be cleaned up by drop unless ownership of the data has been moved to another variable. See how `takes_and_gives_back` function returns the variable before going out of scope.
- Basically if we send a variable, we must return it back from the function to use it again.
- So, there are two things we can do, either _return multiple values using tuples_ or use _references_.

```rust
// This is an example of how a fn returns multiple values using tuples
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

## Borrowing

- We do borrowing whenever we don't want to transfer the complete ownership of a varaible.
- Consider the above example in previous section, inside `calculate_length` function we returned String `s`.
- We returned it because when we passed `s1` into the function we transferred it's ownership to the variable `s`.
- Since, the scope of variable `s` is limited, the passed value of `s1` will die outside the scope of `calculate_length`.
- So, we returned the variable `s` inside a tuple before the end of it's scope.
- Now, there is a workaround to calculate length without transferring the ownership.
- The process of doing so is called _Borrowing_ and it's done using _references_.

### References

- A reference is a pointer to the variable.
- It’s an address we can follow to access data stored at that address that is owned by some other variable.
- Unlike a pointer, a reference is guaranteed to point to a valid value of a particular type.
- We use `&`, called as ampersand, to represent a reference.

  ```rust
  fn main() {
      let s1 = String::from("hello");

      // Instead of transferring ownership, we only passed a reference to the string
      let len = calculate_length(&s1);

      println!("The length of '{}' is {}.", s1, len);
  }

  // We declare that this function will only accept a reference to a String, hence only borrows
  fn calculate_length(s: &String) -> usize {
      s.len()
  }
  ```

- Instead of Ownership transfer, borrowing looks like this, where `s` stores the reference.

  ![Borrowing](https://doc.rust-lang.org/book/img/trpl04-05.svg)

#### Dereference

- This is the opposite of reference. It is represented by `*`.
- It returns the value of a pointer.

#### Mutable Reference

- The references are also immutable by default.
- To make a reference mutable, we need to make both the declared variable and the reference mutable using `mut` keyword.

  ```rust
  fn main() {
      let mut s = String::from("hello"); // Step 2 -> Change variable to mutable

      change(&mut s); // Step 3 -> Pass the string as a mutable reference
  }

  fn change(some_string: &mut String) { // Step 1 -> Declare in fn definition, that it demands a mutable reference
      some_string.push_str(", world");
  }
  ```

### Referencing for strings

#### Which is better `&String` or `&str`?

- Short Answer: `&str`.
- Reason: It allows us to use the same function on both `&String` values and `&str` values.

  ```rust
  fn first_word(s: &String) -> &str { // This sucks, only allows &String

  fn first_word(s: &str) -> &str { // Rustaceans prefer this, since it allows both `&String` and `&str`.
  ```

- Basically, `&str` works for all types of references:

  ```rust
  fn main() {
      let my_string = String::from("hello world");

      // `first_word` works on slices of `String`s, whether partial or whole
      let word = first_word(&my_string[0..6]);
      let word = first_word(&my_string[..]);
      // `first_word` also works on references to `String`s, which are equivalent
      // to whole slices of `String`s
      let word = first_word(&my_string);

      let my_string_literal = "hello world";

      // `first_word` works on slices of string literals, whether partial or whole
      let word = first_word(&my_string_literal[0..6]);
      let word = first_word(&my_string_literal[..]);

      // Because string literals *are* string slices already,
      // this works too, without the slice syntax!
      let word = first_word(my_string_literal);
  }
  ```

### Data Race

- The _data race_ condition happens when these three behaviors occur:

  1. Two or more pointers access the same data at the same time.
  2. At least one of the pointers is being used to write to the data.
  3. There’s no mechanism being used to synchronize access to the data.

- To prevent this condition, Rust adds limitations while using references.

#### Limitations of Referecnes

- We cannot create two mutable references to a variable.

  ```rust
      let mut s = String::from("hello");

      let r1 = &mut s;
      let r2 = &mut s;

      println!("{}, {}", r1, r2);
  ```

- We cannot create one immutable and one mutable reference to a variable.

  ```rust
      let mut s = String::from("hello");

      let r1 = &s; // no problem
      let r2 = &s; // no problem
      let r3 = &mut s; // BIG PROBLEM

      println!("{}, {}, and {}", r1, r2, r3);
  ```

#### Allowed Workarounds for References

- Multiple immutable references are allowed because no one who is just reading the data has the ability to affect anyone else’s reading of the data.

  ```rust
      let mut s = String::from("hello");

      let r1 = &s; // no problem
      let r2 = &s; // no problem

      println!("{}, {}", r1, r2);
  ```

- Rust treats last usage of a immutable reference, as it's end. Hence, the following code runs perfectly, read more about [Non-Lexical Lifetimes](https://blog.rust-lang.org/2018/12/06/Rust-1.31-and-rust-2018.html#non-lexical-lifetimes).

  ```rust
      let mut s = String::from("hello");

      let r1 = &s; // no problem
      let r2 = &s; // no problem
      println!("{} and {}", r1, r2);
      // variables r1 and r2 will not be used after this point

      let r3 = &mut s; // no problem
      println!("{}", r3);
  ```

- You may create a new scope to use two mutable references.

  ```rust
      let mut s = String::from("hello");

      {
          let r1 = &mut s;
      } // r1 goes out of scope here, so we can make a new reference with no problems.

      let r2 = &mut s;
  ```

### Dangling References

- Dangling Reference is a reference to a location in memory which is freed but the reference exists.
- In languages with pointers, it’s easy to erroneously create a dangling pointer.
- In Rust, by contrast, the compiler guarantees that references will never be dangling references: if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.

- Compiler throws error if we manually try to create a dangling reference:

  ```rust
  // FAIL: Rust won't allow you to create Dangling References
  fn main() {
      let reference_to_nothing = dangle();
  }

  fn dangle() -> &String {
      let s = String::from("hello");

      &s
  } // s goes out of scope here, but we try to reference to
  ```

- The Error that compiler throws is:

  ```zsh
  this function's return type contains a borrowed value, but there is no value
  for it to be borrowed from
  ```

- It also mentions, we can fix it using lifetimes.

### Golden Rules of Referencing

1. [No Data Racing](#data-race) - At any given time, you can have either one mutable reference or any number of immutable references.
2. [No Dangling References](#dangling-references) - References must always be valid.

### Useful operations

- Taking Input and performing type conversions

  ```rust
  // Always import when you want to take input from user
  use std::io;

  fn main() {
    let mut input = String::new();

    // read_line() returns io::Result, which is an enum
    // It acts as a match, either returns Ok() with value or prints error.
    io::stdin().read_line(&mut input)
               .expect("Failed to read line.");

    // Declaring a variable with same name again is called shadowing, mostly used for type conversions.
    // Trims whitespaces at start and end, `/n` (newline) and `/r/n` (windows carriage return and a newline)
    // parse() (returns Result), parses the string to a number, into the defined type
    let input: u32 = match guess.trim()
                                .parse()
                                .expect("Invalid Input");

    println!("Your input: {}", input);
  }
  ```

- Generating Random Numbers (need external crate `rand`)

  ```rust
  // The Rng trait defines the functions that we'll use to generate random numbers
  use rand::Rng;

  fn main() {
    // thread_rng() is a random number generator that is local to the current thread of execution and seeded by the operating system.
    // gen_range() is a function part of Rng and it generates a random number of range [inclusive, exclusive), 1..101 === 1..=100
    let random_number = rand::thread_rng().gen_range(1..101);
  }
  ```

- A hack to know type definitions, is to write a wrong definition, and read the error from logs, they'll tell you the correct type.

  ```rust
  // FAIL: It doesn't have u32 as type, we wrote wrong type,
  // to find the correct one by reading error logs.
  let f: u32 = File::open("hello.txt");
  ```

- To read the contents of a file into a string:

  ```rust
  use std::fs;
  use std::io;

  fn read_username_from_file() -> Result<String, io::Error> {
      fs::read_to_string("hello.txt")
  }
  ```

- To return the last character of first line in a text.

  ```rust
  fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines() // Converts string slice into iterator of lines
        .next()? // Returns First element of iterator or None
        .chars() // Converts string slice into iteratot of characters
        .last() // Returns last element
  }
  ```

- Read command line arguments and store them into a vector.

  ```rust
  use std::env;

  fn main() {
      let args: Vec<String> = env::args().collect();
      println!("{:?}", args);
  }
  ```

## Concepts

### Structs

- A _struct_, or structure, is a **custom data type** that lets you name and package together multiple related values that make up a meaningful group.
- It is used to just define the data attributes as we do in Object Oriented Programming Languages.
- There are three types of Structs:
  1. Structs with Named Fields
  2. Tuple Structs
  3. Unit Structs

#### Associated Functions and Methods

- Functions defined for structs using the `impl` keyowrd are called _associated functions_.
- The associated functions which accepts `self` as it's first argument are called _methods_.

#### Structs with Named Fields

- In structs, we name each piece of data, so it's clear what they mean. This name and data type pair are called _fields_.

- Struct definition:

  ```rust
  struct User {
      active: bool, // A Field
      username: String,
      email: String,
      sign_in_count: u64,
  }
  ```

- Creating a struct's instance:

  ```rust
  // If you specify mut, all the values will be mutable otherwise none
  let mut user1 = User {
      email: String::from("someone@example.com"),
      username: String::from("someusername123"),
      active: true,
      sign_in_count: 1,
  };
  ```

- Taking out and updating the values:

  ```rust
  user1.email = String::from("anotheremail@example.com");
  ```

- Defining functions for structs

  ```rust
  fn build_user(email: String, username: String) -> User {
      User {
          email, //We can write like this aslo-> email: email
          username,
          active: true,
          sign_in_count: 1,
      }
  }
  ```

- The struct update syntax (`..`), or spread operator in JS:

  ```rust
  // Initially
  let user2 = User {
      active: user1.active,
      username: user1.username,
      email: String::from("another@example.com"),
      sign_in_count: user1.sign_in_count,
  };

  // After using the struct update syntax (..)
  let user2 = User {
      email: String::from("another@example.com"),
      ..user1
  };
  ```

  Note: This update syntax, works same as assignment operator `=`, so stack values will get copied and heap values will be moved. Since, username is a String, it's value will be moved from `user1` to `user2`, hence `user1` can't be used again.

- To prevent this problem of ownership transfer, we can use `&str` instead of `String` but when we use references in structs, it won't actually compile but will ask for lifetimes.

  ```rust
  // FAIL: Lifetime specifier not provided.
  struct User {
      username: &str,
      email: &str,
      sign_in_count: u64,
      active: bool,
  }

  fn main() {
      let user1 = User {
          email: "someone@example.com",
          username: "someusername123",
          active: true,
          sign_in_count: 1,
      };
  }
  ```

- In this situation the compiler situation looks something like this:

  ```zsh
   --> src/main.rs:2:15
    |
  2 |     username: &str,
    |               ^ expected named lifetime parameter
    |
  help: consider introducing a named lifetime parameter
    |
  1 | struct User<'a> {
  2 |     username: &'a str,
    |
  ```

#### Tuple Structs

- Using Tuple Structs without Named Fields to Create Different Types:

  ```rust
  struct Color(i32, i32, i32);
  struct Point(i32, i32, i32);

  let black = Color(0, 0, 0);
  let origin = Point(0, 0, 0);
  ```

- To access their types, we use the `.` operator followed by the number of this argumnet.

  ```rust
  let color = Color(10, 25, 16);
  let red = color.0;
  let green = color.1;
  let blue = color.2;
  ```

#### Unit Structs

- They are structs without Any Fields (they act like `()`).
- They are Useful when we want to implement a trait on some type but don’t have any data that you want to store in the type itself.

  ```rust
  struct AlwaysEqual;

  let subject = AlwaysEqual;
  ```

#### Why do we use Structs?

1. It is a more sensible design choice to pass as minimum arguments as possible inside a function. For Example, if we need to calculate the area of rectangle, instead of passing `height` and `width`, it would be cleaner to pass the whole rectangle.
2. Now, this can be done with the tuples too. For Example, `let rect1 = (50, 30);` but the problem with this syntax is that any developer can confuse which one is width or height.
3. To make this process clearer and cleaner, we use `struct`, so that we can combine the data and still keep the meaning of each attribute intact.

   ```rust
   struct Rectangle {
       width: u32,
       height: u32,
   }

   fn main() {
       let rect1 = Rectangle {
           width: 50,
           height: 30
       };

       println!("The area of the rectangle is {} square pixels", area(&rect1));
   }


   // Passing Rectangle as a reference is important so that main fn
   // can retain it's ownership after this function is called.
   fn area(rectangle: &Rectangle) -> u32 {
       rectangle.width * rectangle.height
   }
   ```

#### Printing Variables

- Ways to Print the variables:

  - `{}` - Used to print variables with Display trait, for simple data types like int, string etc. we don't need to derive this attribute.
  - `{:?}` - Used to print complex variables with Debug trait, preferred for complex data type like struct, and we need to derive the `Debug` attribute.
  - `{:#?}` - Works similarly like `{:?}`, except it's preferred for structs with large number of fields.
  - `dbg!()` - It is a macro used with Debug trait to print the variables, file and line number. It prints to `stderr` instead of `stdout` (which `println!()` uses). It takes ownership, so prefer sending references to it.

- Here's an example of using the `dbg!()` macro:

  ```rust
  #[derive(Debug)]
  struct Rectangle {
      width: u32,
      height: u32,
  }

  fn main() {
      let scale = 2;
      let rect1 = Rectangle {
          width: dbg!(30 * scale), // It'll resolve the expression `30 * scale`, as if dbg!() call was never there, it happens due to ownership transfer
          height: 50,
      };

      dbg!(&rect1); // To maintian the scope of rect1 in main() we sent only the reference.
  }
  ```

- The output looks like this:

  ```zsh
     Compiling rectangles v0.1.0 (file:///projects/rectangles)
      Finished dev [unoptimized + debuginfo] target(s) in 0.61s
       Running `target/debug/rectangles`
  [src/main.rs:10] 30 * scale = 60
  [src/main.rs:14] &rect1 = Rectangle {
      width: 60,
      height: 50,
  }
  ```

- You can read more about [Derivable Traits](https://doc.rust-lang.org/book/appendix-03-derivable-traits.html) and [Attributes](https://doc.rust-lang.org/reference/attributes.html).

#### Structs with Method Syntax

- When functions are defined in the context of a struct, enum or trait they are called as _Methods_.
- The first parameter of a method is always `self`, which represents the instance.

  ```rust
  #[derive(Debug)]
  struct Rectangle {
      width: u32,
      height: u32,
  }

  // Everything inside the impl block is associated with Rectangle
  impl Rectangle {

      // &self is a short hand for self: `&self` (references are used to prevent mutation)
      // You can pass the following too:
      // self - Ownership of instance
      // &self - Reference to the instance {Currently Using}
      // &mut self - Mutable Reference to the instance
      fn area(&self) -> u32 {
          self.width * self.height
      }

      // It is possible to name methods same as fields of struct
      // Usually these methods are used as getters, to keep the fields private but provide read only accees using the methods
      fn width(&self) -> bool {
        self.width > 0
      }

      // This is how we pass anotherr instance of same struct to a method
      fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
      }
  }

  fn main() {
      let rect1 = Rectangle {
          width: 30,
          height: 50,
      };

      let rect2 = Rectangle {
        width: 15,
        height: 25,
      };

      println!(
          "The area of the rectangle is {} square pixels.",
          rect1.area()
      );

      // If we use rect1.width() - Rust unserstands it as method and
      // if we use rect1.width - Rust unserstands it as a field
      if rect1.width() {
        println!("The rectangle has a nonzero width; it is {}", rect1.width);
      };

      // This is how we can pass second instance while calling a method on first instance
      println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
  }
  ```

Note: When you call a method with `object.something()`, Rust automatically adds in `&`, `&mut`, or `*` so object matches the signature of the method. In other words, the following are the same:

```rust
p1.distance(&p2);
(&p1).distance(&p2);
```

- It is possible to use different `impl` blocks, it is a valid syntax.

  ```rust
  impl Rectangle {
      fn area(&self) -> u32 {
          self.width * self.height
      }
  }

  impl Rectangle {
      fn can_hold(&self, other: &Rectangle) -> bool {
          self.width > other.width && self.height > other.height
      }
  }
  ```

#### Associated Functions

- All the functions defined under `impl` are associated functions.
- Methods are associated functions which has `self` as an argument and we use `.` operator to access it.
- It is possible to define associated functions without passing `self` as the **first** argument, these functions are accessed through `::` operator.
- Here's an example:

  ```rust
  // Calling a method, also an associated function
  instance.method(some_argument);

  // Calling an associated function, without self as the first argument, hence not a method
  String::from("Hello, World!");
  ```

- These associated functions are commonly used as constructors. Also, for the previous example of Rectangle, we can use it as follows:

  ```rust
  impl Rectangle {
      // With this associated function we can create a new instance of Rectangle
      // by passing one value instead of two, hence creating a square.
      fn square(size: u32) -> Rectangle {
          Rectangle {
              width: size,
              height: size,
          }
      }
  }
  ```

- It can be called like this:

  ```rust
  let sq = Rectangle::square(3);
  ```

### Enums

- Enums is the short form of enumerations.
- It allows us to define a type with possible values, these possible values are called _variants_.
- We can enumerate all possible variants, which is where enumeration gets its name.

#### Where to use Enums?

- When their are certain possible values for a type and those possible values may not coincide together.
- For Example, we can make an `enum` for Day, with possible variants Monday-Sunday, now for a certain day any two possible values will never coincide.
- Another Example, IP Address, it's possible variants will be IPV4, IPV6, for a certain IP address, it can only be either of the two.

- Here's an example definition:

  ```rust
  enum IpAddrKind {
      V4,
      V6,
  }
  ```

- To create an instance of ane enum, we use `::` operator:

  ```rust
  let four = IpAddrKind::V4;
  let six = IpAddrKind::V6;
  ```

- To use it in a function:

  ```rust
  // In fn declaration
  fn route(ip_kind: IpAddrKind) {}

  // In fn call
  route(IpAddrKind::V4);
  route(IpAddrKind::V6);
  ```

- Using Enums with Structs:

  ```rust
      enum IpAddrKind {
          V4,
          V6,
      }

      struct IpAddr {
          kind: IpAddrKind,
          address: String,
      }

      let home = IpAddr {
          kind: IpAddrKind::V4,
          address: String::from("127.0.0.1"),
      };

      let loopback = IpAddr {
          kind: IpAddrKind::V6,
          address: String::from("::1"),
      };
  ```

- Enums with associated data types:

  ```rust
  // Now, we don't need an extra struct
  enum IpAddr {
      V4(String),
      V6(String),
  }

  // We get a default constructor function for each variant
  let home = IpAddr::V4(String::from("127.0.0.1"));

  let loopback = IpAddr::V6(String::from("::1"));
  ```

- Defining enum variants with different data types:

```rust
enum IpAddr {
    // Defining variants with two different data types
    // is only possible through enums and not through enums with struct
    V4(u8, u8, u8, u8),
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);

let loopback = IpAddr::V6(String::from("::1"));
```

- This is how [standard library defines IP addresses](https://doc.rust-lang.org/std/net/enum.IpAddr.html):

  ```rust
  struct Ipv4Addr {
      // --snip--
  }

  struct Ipv6Addr {
      // --snip--
  }

  // It is posible to put any data type inside
  // the enum variant, int, String, struct,
  // or even enum
  enum IpAddr {
      V4(Ipv4Addr),
      V6(Ipv6Addr),
  }

  ```

- Enum with complicated data types:

  ```rust
  // Cleaner Approach
  enum Message {
      Quit, // No data associated with it at all!
      Move { x: i32, y: i32 }, // Has named fields like struct
      Write(String),
      ChangeColor(i32, i32, i32),
  }

  // Uglier approach using struct
  struct QuitMessage; // unit struct
  struct MoveMessage {
      x: i32,
      y: i32,
  }
  struct WriteMessage(String); // tuple struct
  struct ChangeColorMessage(i32, i32, i32); // tuple struct
  ```

- It is possible to define associated functions on enums using `impl`:

  ```rust
  enum Message {
      Quit,
      Move { x: i32, y: i32 },
      Write(String),
      ChangeColor(i32, i32, i32),
  }

  impl Message {
      fn call(&self) {
          // method body would be defined here
      }
  }

  let _quit_message = Message::Quit; // We won't use parantheses because it is of Unit Type
  let _write_message = Message::Write(String::from("Hello")); // Constructor function, that accepts String and will stroe it on heap
  let _change_color_message = Message::ChangeColor(12, 12, 12); // Constructor function, that accepts three i32 values
  let _move_message = Message::Move {x: 5, y: 6}; // Works similar to creating new instance of struct with named fields

  _quit_message.call();
  ```

#### The `Option` Enum

- The Option type is used in many places because it encodes the very common scenario in which a value could be something or it could be nothing.
- Expressing this concept in terms of the type system means the compiler can check whether you’ve handled all the cases you should be handling.
- This functionality can prevent bugs that are extremely common in other programming languages.
- Rust doesn't have `Null`, so it uses `Option` enum with variants `Some` and `None`.
- This makes Rust extremely cool, you may read more about ["Null References: The Billion Dollar Mistake"](https://doc.rust-lang.org/book/ch06-01-defining-an-enum.html#the-option-enum-and-its-advantages-over-null-values).
- The problem with null values is that if you try to use a null value as a not-null value, you’ll get an error of some kind.
- Rust's `Option` enum will always ask you to offer solution for both `Some` and `None`.

  ```rust
  // It is generic over any data type T
  enum Option<T> {
      None,
      Some(T),
  }

  // Rust automatically inferred to be of type Option<i32> because we passed a number and i32 is it's default type
  let some_number = Some(5);
  // Similarly, Rust inferred Option<&str>, since we passed string literal
  let some_string = Some("a string");

  // Here, since None can belong to any data type, we explicitly define i32
  let absent_number: Option<i32> = None;
  ```

#### Why is having `Option<T>` any better than having `null`?

- In short, because `Option<T>` and `T` (where `T` can be any type) are different types, the compiler won’t let us use an `Option<T>` value as if it were definitely a valid value.
- For example, this code won’t compile because it’s trying to add an `i8` to an `Option<i8>`:

  ```rust
  let x: i8 = 5;
  let y: Option<i8> = Some(5);

  let sum = x + y;
  ```

- When we have a value of a type like `i8` in Rust, the compiler will ensure that we always have a valid value.
- We can proceed confidently without having to check for null before using that value.
- when we have an `Option<i8>`, we'll have to worry about possibly not having a value, and the compiler will make sure we handle that case before using the value.
- In other words, you have to convert an `Option<T>` to a `T` before you can perform `T` operations with it.
- Generally, this helps catch one of the most common issues with null: assuming that something isn’t null when it actually is.

- In languages like C, this will work and print something, even though we know it doesn't contain any value.

  ```c
  #include <stdio.h>

  int main() {
      int x;
      printf("Value of x: %i", x);

      return 0;
  }
  ```

- In Rust, it'll not compile, since it identifies an absence of value.

  ```rust
  fn main() {
      let number: i32;
      println!("Value of x: {}", number);
  }
  ```

- Everywhere that a value has a type that isn’t an `Option<T>`, you can safely assume that the value isn’t `null`.
- This was a deliberate design decision for Rust to limit null’s pervasiveness and increase the safety of Rust code.

#### The `match` conftrol flow operator

- It allows you to compare a value against a series of patterns and then execute code based on which pattern matches.
- It is possible to express very different kind of patterns. Also, Rust has a cumpolsary check, where it handles that all possible cases are handled.
- Think of a `match` expression as being like a coin-sorting machine: coins slide down a track with variously sized holes along it, and each coin falls through the first hole it encounters that it fits into.
- At the first pattern the value “fits”, the value falls into the associated code block to be used during execution.
- The expression with `if` statement only returns a boolean value but `match` expression can return any type.
- Here's an Example Below:

  ```rust
  enum Coin {
      Penny,
      Nickel,
      Dime,
      Quarter,
  }

  fn value_in_cents(coin: Coin) -> u8 {
      match coin {
          Coin::Penny => {
              println!("Lucky penny!");
              1
          },
          Coin::Nickel => 5,
          Coin::Dime => 10,
          Coin::Quarter => 25,
      }
  }
  ```

- Each new pattern under `match` is an arm. An arm has two parts: a pattern and some code.
- The code associated with each arm is an expression, and the resulting value of the expression in the matching arm is the value that gets returned for the entire match expression.

#### An `enum` inside another `enum`

- This is how we'll be using `match` for such cases:

```rust
#[derive(Debug)] // so we can inspect the state in a minute
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}

fn main() {
  let _value = value_in_cents(Coin::Quarter(UsState::Alaska));
}
```

#### Matching with `Option<T>`

- The value inside Option of type `T` can be passed through like a functional argument using the `match` expression.

  ```rust
      fn plus_one(x: Option<i32>) -> Option<i32> {
          match x {
              None => None,
              Some(i) => Some(i + 1),
          }
      }

      let five = Some(5);
      let six = plus_one(five);
      let none = plus_one(None);
  ```

- The `match` expression always covers all the possible values, that's why we call them _exhaustive_: we must exhaust every last possibility in order for the code to be valid.

  ```rust
  // FAIL: All the cases not covered in match expression, the None case is remaining
  fn plus_one(x: Option<i32>) -> Option<i32> {
      match x {
          Some(i) => Some(i + 1),
      }
  }
  ```

- Especially in the case of `Option<T>`, when Rust prevents us from forgetting to explicitly handle the `None` case, it protects us from assuming that we have a value when we might have `null`, thus _making the billion-dollar mistake discussed earlier impossible_.

#### Catch remaining patterns using `_` placeholder

- It is possible to cover the remaining cases inside the `match` expression, it is similar to `default` case of `switch` statement in other languages.

  ```rust
      let dice_roll = 9;
      match dice_roll {
          3 => add_fancy_hat(),
          7 => remove_fancy_hat(),
          other => move_player(other), // This will match all the cases that aren't specifically listed
      }

      fn add_fancy_hat() {}
      fn remove_fancy_hat() {}
      fn move_player(num_spaces: u8) {}
  ```

- Sometimes we use placeholder `_`, to specify Rust, that this value is useless.

  ```rust
      let dice_roll = 9;
      match dice_roll {
          3 => add_fancy_hat(),
          7 => remove_fancy_hat(),
          _ => reroll(), // These values aren't that important
      }

      fn add_fancy_hat() {}
      fn remove_fancy_hat() {}
      fn reroll() {}
  ```

- If we want to tell Rust to literally do nothing, then we can use unit tuple `()` instead of fn call.

  ```rust
      let dice_roll = 9;
      match dice_roll {
          3 => add_fancy_hat(),
          7 => remove_fancy_hat(),
          _ => (), // Telling Rust to "do nothing"
      }

      fn add_fancy_hat() {}
      fn remove_fancy_hat() {}
  ```

#### The `if let` syntax

- It is used in case you want to consider only particular case of an enum.
- For example, if you want to consider only the `Some` variant of an enum `Option<>`, you may prefer to use the `if let` syntax instead of `match`:

  ```rust
  // The older approach using the match syntax
  let config_max = Some(3u8);
  match config_max {
      Some(max) => println!("The maximum is configured to be {}", max),
      _ => (), // This line seems redundant
  }

  // More concise approach with if let
  let config_max = Some(3u8);
  if let Some(max) = config_max {
      println!("The maximum is configured to be {}", max);
  }
  ```

- The `if let` accepts a pattern (consider `Some(max)`) and an expression (consider `config_max`) seperated by and `=` sign.
- Before using `if let` please make sure whether gaining conciseness is an appropriate trade-off for losing exhaustive checking.
- This approach is not exhaustive in sense that it only considers one pattern and ignores other unlike the `match` syntax.

- It is possible to use `else` with `if let`:

```rust
// In this problem we are counting the coins that aren't quarter
let mut count = 0;
match coin {
    Coin::Quarter(state) => println!("State quarter from {:?}!", state),
    _ => count += 1,
}

// Another possible approach with if let and else
let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("State quarter from {:?}!", state);
} else {
    count += 1;
}
```

### Project Structuring

- The Rust's _module system_ includes:
  - **Packages:** A Cargo feature that lets you build, test, and share crates
  - **Crates:** A tree of modules that produces a library or executable
  - **Modules** and **use:** Let you control the organization, scope, and privacy of paths
  - **Paths:** A way of naming an item, such as a struct, function, or module
- Once you’ve implemented an operation, other code can call that code via the code’s public interface without knowing how the implementation works.
- The way you write code defines which parts are public for other code to use and which parts are private.
  - `private` - No exteranl code can call this code directly
  - `public` - External code can call this code directly
- The way privacy works in Rust is that all items (functions, methods, structs, enums, modules, and constants) are private by default.

#### Package

- When we run the command `cargo new` it creates the package.
- A package contains a _Cargo.toml_ file that describes how to build those crates.
- A package can contain **at most one** _library_ crate. It can contain **as many** _binary_ crates as you’d like, but it must contain at least one crate (either library or binary).
- As a package grows, you can extract parts into separate crates that become external dependencies.

#### Crates

- A crate is a binary or library.
- The _crate root_ is a source file that the Rust compiler starts from and makes up the root module of your crate.
- Cargo follows a convention that _src/main.rs_ is the crate root of a **binary** crate with the same name as the package.
- Similarly, _src/lib.rs_ is the crate root of a **library** crate with the same name as the package.
- Cargo passes the crate root files to rustc to build the library or binary.
- A crate’s functionality is namespaced in its own scope, it means we can import another crate let's say `rand` which has a trait named `Rng`, and still create a new struct named `Rng` in our project's crate. The `rustc` will never confuse between the two and we can access the `rand`'s components as `rand::Rng`.

#### Modules

- Modules are used to structure code inside a crate.
- It is also used to provide privacy to your code.
  - `private` - Exteranl code outside that module can not call this code directly
  - `public` - External code outside that module can call this code directly
- By using modules, we can group related definitions together and name why they’re related.
- The benefit it'll provide you is that other programmers reading your code can easily find the code they are searching for because then they'll navigate through groups rather than each function definition. Also, they'll add new code in the right module.
- The contents of the files _src/main.rs_ and _src/lib.rs_ (these files are also referred as crate roots) form a module named `crate` at the root of the crate’s module structure, known as the _module tree_.

  ```zsh
  crate                                 // An implicit module, definitely not named by you
   └── front_of_house                   // Main Module inside lib.rs
       ├── hosting                      // Submodule
       │   ├── add_to_waitlist
       │   └── seat_at_table
       └── serving                      // Submodule
           ├── take_order
           ├── serve_order
           └── take_payment
  ```

#### Paths

- We use a path in the same way we use a path when navigating a filesystem.
- A path can take two forms:

  - An _absolute path_ starts from a crate root by using a crate name or a literal `crate`.
  - A _relative path_ starts from the current module and uses `self`, `super`, or an identifier in the current module.

- You ma consider paths in rust quite similar to the paths used to access the filesystem
  - `crate` - Root (`/`)
  - `::` - Used to distinct others (`/`)
  - `super` - Used to go back one step (`../`)
- Here's an exmaple:

  ```rust
  // eat_at_restaurant is a sibling to front_of_house (since they are in same file),
  // thus front_of_house doesn't need pub keyword to make it accessible.
  mod front_of_house {
      // Add pub to allow the functions that can access front_of_house to access hosting too.
      pub mod hosting {
          // Add pub to allow the functions that can access hosting to access add_to_waitlist too.
          pub fn add_to_waitlist() {}
      }
  }

  pub fn eat_at_restaurant() {
      // Absolute path
      // Filesystem Equivalent to /front_of_house/hosting/add_to_waitlist
      crate::front_of_house::hosting::add_to_waitlist();

      // Relative path
      // Filesystem Equivalent to front_of_house/hosting/add_to_waitlist
      front_of_house::hosting::add_to_waitlist();
  }
  ```

- Our preference is to specify absolute paths because it’s more likely to move code definitions and item calls independently of each other.
- Items in a parent module can’t use the private items inside child modules, but items in child modules can use the items in their ancestor modules.
- But you can expose inner parts of child modules’ code to outer ancestor modules by using the pub keyword to make an item public.
- If you want to make an item like a function or struct private, you put it in a module.
- Another example to show usecase for `super`:

  ```rust
  fn serve_order() {}

  mod back_of_house {
      fn fix_incorrect_order() {
          cook_order();
          super::serve_order();
      }

      fn cook_order() {}
  }

  ```

- If we make a `struct` public, it **doesn't** mean all it's fields are public too. We use `.` to access fields.

  ```rust
  mod back_of_house {
      pub struct Breakfast {
          pub toast: String, // Accessible
          seasonal_fruit: String, // Not Accessible
      }

    ...
  }
  ```

- On the other hand, if we make an `enum` public all it's variants becomes public too. We use `::` to access variants.

  ```rust
  mod back_of_house {
      pub enum Appetizer {
          Soup, // Accessible
          Salad, // Accessible
      }

      ...
  }
  ```

#### The `use` keyword

- It is similar to the `import` keyword in python.

  ```rust
  mod front_of_house {
      pub mod hosting {
          pub fn add_to_waitlist() {}
      }
  }

  // The line below will make hosting as a valid name in the scope
  use crate::front_of_house::hosting;

  pub fn eat_at_restaurant() {
      hosting::add_to_waitlist();
      hosting::add_to_waitlist();
      hosting::add_to_waitlist();
  }
  ```

- We can use the following ways to achieve the same thing:
  - `use self::front_of_house::hosting;`
  - `use crate::front_of_house::hosting::add_to_waitlist;`
- Though, the one mentioned inside the code block is the idiiomatic way to do it in Rust.
- On the other hand, when bringing in structs, enums, and other items with use, it’s idiomatic to specify the full path.

  ```rust
  use std::collections::HashMap;

  fn main() {
      let mut map = HashMap::new();
      map.insert(1, 2);
  }
  ```

- In case if we have two items with same name (in our case `Result`) but from different crates (in our case `fmt` and `io`), then we'll not use the full path, as it'll confuse Rust.

  ```rust
  use std::fmt;
  use std::io;

  // This way Rust will be able to distinguish which Result we want
  fn function1() -> fmt::Result {
      // --snip--
  }

  fn function2() -> io::Result<()> {
      // --snip--
  }
  ```

- Alternatively, we can use the `as` keyword to deal with two same names.

  ```rust
  use std::fmt::Result;
  use std::io::Result as IoResult;
  ```

- We can re-export the code using `pub use`.

  ```rust
  mod front_of_house {
      pub mod hosting {
          pub fn add_to_waitlist() {}
      }
  }

  // The use keyword will create a local variable named hosting in this scope
  // and pub keyword will re-export it for the external code to use it.
  pub use crate::front_of_house::hosting;

  pub fn eat_at_restaurant() {
      hosting::add_to_waitlist();
      hosting::add_to_waitlist();
      hosting::add_to_waitlist();
  }
  ```

- Using external packages:

  - First, we'll add the name and version of the package in `cargo.toml`, so that it can be automatically downloaded through crates.io.

    ```toml
    rand = "0.8.3"
    ```

  - Then, we'll use the `use` keyword to bring it into the scope.

    ```rust
    use rand::Rng;

    fn main() {
        let secret_number = rand::thread_rng().gen_range(1..101);
    }
    ```

  - The packages like `std` is also external but is a part of Rust language and it is not needed to download it from crates.io.

- Nesting the paths:

  ```rust
  // Dirty Approach
  use std::cmp::Ordering;
  use std::io;

  // Cleaneer Aproach (Nested)
  use std::{cmp::Ordering, io};

  //Another Dierty Approach
  use std::io;
  use std::io::Write;

  // Cleaner Approach (Nesting using self)
  use std::io::{self, Write};
  ```

- The glob operator (`*`), is used to bring all public definitions into the scope.

  ```rust
  // It is a bit riskier, as it is hard to identify what all definitions have been brought into scope
  use std::collections::*;
  ```

### Modularizing in different files

- Let's say we decided to put some code in the file `src/front_of_house.rs`, and we want to use this code inside the file `src/lib.rs`. It can be done like this:

  ```zsh
  src
   ├── front_of_house.rs
   └── lib.rs
  ```

  ```rust
  // Filename: src/lib.rs
  // This will bring the contents of module (thst is stored in file `src/front_of_house.rs`)
  // into the current file
  mod front_of_house;

  // This will allow us to use as well as export it
  // so that external can use it too.
  pub use crate::front_of_house::hosting;

  pub fn eat_at_restaurant() {
      hosting::add_to_waitlist();
      hosting::add_to_waitlist();
      hosting::add_to_waitlist();
  }
  ```

- Now, we can make a new directory as well and can store the files as folows:

  ```zsh
  src
   ├── front_of_house
   │   └── hosting.rs
   ├── front_of_house.rs
   └── lib.rs
  ```

  ```rust
  // Filename: src/front_of_house.rs
  pub mod hosting;
  ```

  ```rust
  // Filename: src/front_of_house/hosting.rs
  pub fn add_to_waitlist() {}
  ```

### Refactoring Guides

This pattern is about separating concerns: `main.rs` handles running the program, and `lib.rs` handles all the logic of the task at hand. Because you can’t test the main function directly, this structure lets you test all of your program’s logic by moving it into functions in `lib.rs`. The only code that remains in main.rs will be small enough to verify its correctness by reading it.

### Common Collections

- The various data structures in Rust's standard library are called Collections. Refer [here](https://doc.rust-lang.org/std/collections/index.html).
- They can contain multiple values and collections point data stored on heap.
- Most Common Collections:
  - Vectors
  - String
  - Hash Map

#### Vector

- It is represesnted as `Vec<T>`.
- You can store variable number of values, unlike Array. Though, the data type of stored values should be same.
- Vectors store values next to each other in memory.
- Creating a new vector:

  ```rust
    // We'll add type annotation because the vector has 0 elements,
    // hence, there is no way for Rust to recognize type implicitly
    let v: Vec<i32> = Vec::new();
  ```

- Creating vectors using a macro:

  ```rust
    let v = vec![1, 2, 3];
  ```

- Pushing new values (make sure Vector is mutable):

  ```rust
    v.push(5);
  ```

- Popping new values:

  ```rust
  let mut vec = vec![1, 2, 3];
  assert_eq!(vec.pop(), Some(3));
  ```

- Even though vectors store values on heap and it is possible to introduce references to the elements of the vector. Still, the vectors automatically cleans up memory as it goes out of scope:

  ```rust
    {
        let v = vec![1, 2, 3, 4];

        // do stuff with v
    } // <- v goes out of scope and is freed here
  ```

- Accessing elements inside a vector:

  - Method 1:

    ```rust
      let third: &i32 = &v[2]; // Might panic due to out of index
    ```

  - Method 2:

    ```rust
    match v.get(2) { // Gives Option<&T>
        Some(third) => println!("The third element is {}", third),
        None => println!("There is no third element."),
    }
    ```

- You can't do that:

  ```rust
      let mut v = vec![1, 2, 3, 4, 5];

      let first = &v[0]; // A reference to immutable vector [Immutable Borrow]

      v.push(6); // Writing to a mutable vector [Mutable Error]

      println!("The first element is: {}", first); // Accessing the reference after writing [Immutable Borrow Used]

      // Recall: You can’t have mutable and immutable references in the same scope.
  ```

- Why should a reference to the first element care about what changes at the end of the vector? This error is due to the way vectors work: adding a new element onto the end of the vector might require allocating new memory and copying the old elements to the new space, if there isn’t enough room to put all the elements next to each other where the vector currently is.

- Itearting over the Vector:

  ```rust
    let v = vec![100, 32, 57];
    for i in &v {
        println!("{}", i);
    }
  ```

- Iterating and mutating the vector:

  ```rust
      let mut v = vec![100, 32, 57];
      for i in &mut v {
          *i += 50;
      }
  ```

- Storing values of different types using enums:

  ```rust
      // Using enums in vectors add stability as
      // when we'll use match all possible cases
      // will be covered.
      enum SpreadsheetCell {
          Int(i32),
          Float(f64),
          Text(String),
      }

      let row = vec![
          SpreadsheetCell::Int(3),
          SpreadsheetCell::Text(String::from("blue")),
          SpreadsheetCell::Float(10.12),
      ];
  ```

#### Strings and UTF-8 encoding

- Characters are represented by single inverted commas, and has `4 bytes` of storage. For Example, `'😀'`.
- String is not a collection of characters but collections of bytes.
- Rust has only one string type in the core language, which is the string slice `str` that is usually seen in its borrowed form `&str`.
- String Slices are the references to some UTF-8 data stored somewhere else.
- String Literals are string slices when stored in program's binary.
- The `String` type, which is provided by Rust’s standard library rather than coded into the core language, is a growable, mutable, owned, UTF-8 encoded string type.
- When Rustaceans, call "string in rust", they collectively mean:
  - `String`
  - `&str`
- Both `String` and `&str` are UTF-8 encoded.
- Creating the `String` type:

  ```rust
  let mut s = String::new();
  ```

- To create a `String` from some starting string:

  ```rust
  let s = "initial contents".to_string(); // This fn can be used on any type that implements Display trait
  let s = String::from("initial contents");
  ```

- It is possible to store any properly encoded data:

  ```rust
  let hello = String::from("नमस्ते");
  let hello = String::from("안녕하세요");
  let hello = String::from("Здравствуйте");
  ```

- Updating the String:

  ```rust
  let mut s = String::from("foo");
  s.push_str("bar"); // It takes string slice, hence doesn't takes ownership
  s.push('!'); // This fn only takes character as argument.
  // s will become "foobar!"
  ```

- Concatenating two strings with the `+` operator:

  ```rust
  // '+' is a replacement of - fn add(self, s: &str) -> String {
  let s1 = String::from("Hello, ");
  let s2 = String::from("world!");
  let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used
  ```

Note: In Rust, if we provide `&str`, as a function's argument, it can accept both `&String` and `&str`. Rust uses a deref coercion, which here turns `&s2` into `&s2[..]`.

- Combining multiple strings or formatting them:

  ```rust
  let s1 = String::from("tic");
  let s2 = String::from("tac");
  let s3 = String::from("toe");

  // Method 1
  let s = s1 + "-" + &s2 + "-" + &s3;

  // Method 2
  let s = format!("{}-{}-{}", s1, s2, s3); // It works like println!() but returns String
  ```

- Indexing into Strings is not possible and results in error:

  ```rust
  // FAIL: Strings can be indexed in Rust
  let s1 = String::from("hello");
  let h = s1[0]; // Won't work
  ```

- How values are stored in string.

  - String is just a wrapper over `Vec<u8>`, this means `1 byte` of space for each element in the vector. Hence, if we want to save special charcters, then it may take more than one element to store the values.
  - Let's consider following examples:

    ```rust
    let hello = String::from("Hola"); // Each character will take 1 byte of storage
    let hello = String::from("Здравствуйте"); // Each character will take 2 bytes of storage
    ```

  - Let's understand using the Hindi word `“नमस्ते”`:

    - As Bytes (the way `String` does using `u8` which ranges from `0` to `255`):

      ```rust
      [224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164,
      224, 165, 135]
      ```

    - As Unicode Scalar Values (the way `char` does):

      ```rust
      ['न', 'म', 'स', '्', 'त', 'े']
      ```

    - As Grapheme Clusters (the way a Hindi speaker might do):

      ```rust
      ["न", "म", "स्", "ते"]
      ```

- Slicing Strings:

  - You need to provide the range of `bytes` to be sliced out of String. Again, not characters but bytes.

    ```rust
    let hello = "Здравствуйте"; // Each character here is composed of 2 bytes

    let s = &hello[0..4]; // It'll save first 4 bytes, `Зд`

    let will_panic = &hello[0..1]; // It'll panic, as if invalid index was accessed in the vector.
    ```

- Iterating over strings:

  - You can iterate over the unicode scalar values or what `chars` might store:

    ```rust
    for c in "नमस्ते".chars() {
        println!("{}", c);
    }

    // This is what it'll print
    न
    म
    स
    ्
    त
    े
    ```

  - You can iterate over bytes also, the way `String` is stored in `Vec<u8>` format:

    ```rust
    for b in "नमस्ते".bytes() {
        println!("{}", b);
    }

    // The output will be like
    224
    164
    // --snip--
    165
    135
    ```

#### HashMaps

- The type `HashMap<K, V>` stores a mapping of keys of type `K` to values of type `V`.
- It does this via a hashing function, which determines how it places these keys and values into memory.
- Creating a new HashMap:

  ```rust
      use std::collections::HashMap;

      let mut scores = HashMap::new();

      scores.insert(String::from("Blue"), 10);
      scores.insert(String::from("Yellow"), 50);
  ```

- Just like Vectors, HashMaps also save their values on Heap.
- All Keys must have same type, and all values must have same type.
- Creating HashMap through iterators:

  ```rust
      use std::collections::HashMap;

      let teams = vec![String::from("Blue"), String::from("Yellow")];
      let initial_scores = vec![10, 50];

      let mut scores: HashMap<_, _> =
          teams.into_iter()
               .zip(initial_scores.into_iter()) // creates a tuple, example ("Blue", 10)
               .collect(); // Converts tuple into HashMap
  ```

- HashMap and ownership: Types that implement `Copy` trait will be copied else moved. For Example, `i32` will be copied but `String` will be moved.

  ```rust
      use std::collections::HashMap;

      let field_name = String::from("Favorite color");
      let field_value = String::from("Blue");

      let mut map = HashMap::new();
      map.insert(field_name, field_value);
      // You can't use field_name or field_value now, as they've been moved
  ```

- Accessing value in HashMap, the `get` method returns `Option<&V>`:

  ```rust
      use std::collections::HashMap;

      let mut scores = HashMap::new();

      scores.insert(String::from("Blue"), 10);
      scores.insert(String::from("Yellow"), 50);

      let team_name = String::from("Blue");
      let score = scores.get(&team_name); // This is how we access value for a certain Key

      // The score variable will contain - Some(&10)
  ```

- Iterating over a HashMap:

  ```rust
      use std::collections::HashMap;

      let mut scores = HashMap::new();

      scores.insert(String::from("Blue"), 10);
      scores.insert(String::from("Yellow"), 50);

      for (key, value) in &scores {
          println!("{}: {}", key, value);
      }
  ```

- Updating a HashMap:

  - Overwriting the value:

    ```rust
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Blue"), 25);

    // Output - scores = {"Blue": 25}
    ```

  - Only inserting the value if the Key has no value:

    ```rust
    scores.insert(String::from("Blue"), 10);

    // We'll need to use entry to use or_insert
    scores.entry(String::from("Yellow")).or_insert(50); // This will add 50
    scores.entry(String::from("Blue")).or_insert(50); // This won't replace 10 with 50

    // Output - scores = {"Yellow": 50, "Blue": 10}
    ```

  - Updating a value based on the Old Value:

    ```rust
    use std::collections::HashMap;

    let text = "hello world wonderful world";

    let mut map = HashMap::new();

    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0); // or_insert returns mutable reference to the Value, &mut V
        *count += 1;
    }

    println!("{:?}", map);

    // Output - map = {"world": 2, "hello": 1, "wonderful": 1}
    ```

- The hashing function that Rust uses is `SipHash`. You can replace the hashing function. Please [refer here](https://doc.rust-lang.org/book/ch08-03-hash-maps.html#hashing-functions) for more.

### Error Handling

#### Types of Errors

| Recoverable                                                                | Unrecoverable                                                                |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Errors like file not found error.                                          | Errors like trying to access a location beyond the end of an array.          |
| It’s reasonable to report the problem to the user and retry the operation. | They are always symptoms of bugs.                                            |
| `Result<T, E>` is used for Recoverable Errors.                             | The `panic!` macro is used to stop the execution for an unrecoverable error. |

#### Unrecoverable Errors with `panic!`

Fun Fact: In C, attempting to read beyond the end of a data structure is undefined behavior. You might get whatever is at the location in memory that would correspond to that element in the data structure, even though the memory doesn’t belong to that structure. This is called a buffer overread and can lead to security vulnerabilities if an attacker is able to manipulate the index in such a way as to read data they shouldn’t be allowed to that is stored after the data structure. In Rust, you'll encounter a `panic!()` in such cases.

- When the `panic!` macro executes, Rust does the following:

  - Print a failure message
  - Unwind and clean up the stack
  - Quit

- Panic is usually used, when a bug appears and the programmer doesn't know how to handle it.
- If you don't want your program to "slowly unwind and clean up the stack" instead "abort the program and let OS handle the cleaning". You may do that by adding following lines to the `Cargo.toml` file. Refer [here](https://doc.rust-lang.org/book/ch09-01-unrecoverable-errors-with-panic.html#unwinding-the-stack-or-aborting-in-response-to-a-panic) for more.

  ```rust
  [profile.release]
  panic = 'abort'
  ```

- To receive a backtrace in case of panic, you might need to run the following command:

  ```zsh
  RUST_BACKTRACE=1 cargo run
  ```

- The best way to read backtraces is to ready from top to bottom, once you see the first instance mentioning a file that you've written, you should probably try to solve from there.

- Debug symbols (they are required to receive backtraces) are enabled by default when using `cargo build` or `cargo run` without the `--release` flag.

#### Recoverable Errors with `Result`

- Result is an enum, that considers two possible outcomes: success (`Ok(T)`) or failure (`Err(E)`).

  ```rust
  enum Result<T, E> {
      Ok(T),
      Err(E),
  }
  ```

- Handling recoverable errors using the `match` expression.

  ```rust
  use std::fs::File;

  fn main() {
      let f = File::open("hello.txt");

      let f = match f {
          Ok(file) => file, // Handling Success
          Err(error) => panic!("Problem opening the file: {:?}", error), // Handling Failure
      };
  }
  ```

- Matching on different errors:

  ```rust
  use std::fs::File;
  use std::io::ErrorKind;

  fn main() {
      let f = File::open("hello.txt");

      // Match on File, whether it gets opened or not
      let f = match f {
          Ok(file) => file,
          // If file not found, then create a new file and transfer file handle,
          // this error is part of io::ErrorKind, which was found using error.kind()
          Err(error) => match error.kind() {
              // In case we receive ErrorKind::NotFound, we'll apply
              // match again to check whether creation of file, fails or succeeds
              ErrorKind::NotFound => match File::create("hello.txt") {
                  Ok(fc) => fc,
                  Err(e) => panic!("Problem creating the file: {:?}", e),
              },
              other_error => {
                  panic!("Problem opening the file: {:?}", other_error)
              }
          },
      };
  }
  ```

- In case you don't like using a lot of match statements (refer above example), you may use `unwrap_or_else`:

  ```rust
  use std::fs::File;
  use std::io::ErrorKind;

  fn main() {
     let f = File::open("hello.txt").unwrap_or_else(|error| {
       if error.kind() == ErrorKind::NotFound {
         File::create("hello.txt").unwrap_or_else( |error| {
           panic!("Problem creating the file: {:?}", error);
         }
         )
       } else {
         panic!("Problem opening the file: {:?}", error);
       }
     })
  }
  ```

- In case you want a shortcut, you may only use `unwrap()`. It either returns what's inside `Ok(T)`, or panics in case of `Err(E)`:

  ```rust
  use std::fs::File;

  fn main() {
      let f = File::open("hello.txt").unwrap();
  }
  ```

- For those cases, when you want to send a panic message but only want to unwrap in one line, you may use `expect`:

  ```rust
  use std::fs::File;

  fn main() {
      let f = File::open("hello.txt").expect("Failed to open hello.txt"); // Same as unwrap but contains panic message
  }
  ```

- Propogating errors using the `Result`:

  ```rust
  use std::fs::File;
  use std::io::{self, Read};

  fn read_username_from_file() -> Result<String, io::Error> {
      let f = File::open("hello.txt");

      let mut f = match f {
          Ok(file) => file,
          Err(e) => return Err(e), // This is a std::io error type
      };

      let mut s = String::new();

      match f.read_to_string(&mut s) {
          Ok(_) => Ok(s),
          Err(e) => Err(e), // This is also a std::io error type
      }
  }
  ```

- The shortcut of above code can be done using `?`. `unwrap` panics in case of Err(E), but this operator returns the error, same as the code above.

  ```rust
  // ? operator changes the error type to the mentioned
  // Error type in the fn declaration using the from implementation
  use std::fs::File;
  use std::io;
  use std::io::Read;

  fn read_username_from_file() -> Result<String, io::Error> {
      let mut f = File::open("hello.txt")?;
      let mut s = String::new();
      f.read_to_string(&mut s)?;
      Ok(s)
  }
  ```

- It is possible to use the `?` operator multiple times in a single line:

  ```rust
  use std::fs::File;
  use std::io;
  use std::io::Read;

  fn read_username_from_file() -> Result<String, io::Error> {
      let mut s = String::new();

      File::open("hello.txt")?.read_to_string(&mut s)?;

      Ok(s)
  }
  ```

- There's a Rust's official implementation of the functionality mentioned in the above code:

  ```rust
  use std::fs;
  use std::io;

  fn read_username_from_file() -> Result<String, io::Error> {
      fs::read_to_string("hello.txt")
  }
  ```

- The `?` operator can only be used in the functions that has a return type of `Result<Ok(T), Err(E)>`, `Option<Some(T), None>`, or another type that implements FromResidual:

  ```rust
  // FAIL: main() doensn't returns a Result<>
  // but the ? operator requires that
  use std::fs::File;

  fn main() {
      let f = File::open("hello.txt")?;
  }
  ```

  ```rust
  // It works with the Option
  fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
  }
  ```

- There's a way to use `?` inside `main()`. The `main()` either returs `0` on success or other integer on failure. Also, it's possible to return `<Result(), E>`:

  ```rust
  use std::error::Error;
  use std::fs::File;

  fn main() -> Result<(), Box<dyn Error>> {
      let f = File::open("hello.txt")?;

      Ok(())
  }
  ```

- Differences between `unwrap`, `unwrap_or`, and `?` operator

| Property                                        | `unwrap`           | `expect`                      | `unwrap_or`                           | `?` operator       |
| ----------------------------------------------- | ------------------ | ----------------------------- | ------------------------------------- | ------------------ |
| Error Handling                                  | Panics             | Panics with the given message | Executes code inside it's parantheses | Returns error      |
| Can be used on `Result`                         | :heavy_check_mark: | :heavy_check_mark:            | :heavy_check_mark:                    | :heavy_check_mark: |
| Can be used on `Option`                         | :heavy_check_mark: | :heavy_check_mark:            | :heavy_check_mark:                    | :heavy_check_mark: |
| Function return type to be same as wrapped item | :x:                | :x:                           | :x:                                   | :heavy_check_mark: |

Note: You can only use the `?` operator on a `Result` in a function that returns `Result`, and you can use the `?` operator on an `Option` in a function that returns `Option`.

- To `panic!` or Not to `panic!`

  - When to use `Result`

    - When `panic!` is called, there is no way to recover the program, so if there is a slightest possiblity to recover the program, it's recommended to use that instead of `panic!`.
    - Always try to prevent converting a recoverable error into an unrecoverable one. Hence, always prefer `Result` over `panic!`.
    - The `unwrap` and `expect` methods are very handy when prototyping, and if you want to make your program more robust, you may add better error handling.

  - When to use `panic!`

    - In case you want your test to fail in certain cases, even if a certain fn is not exactly what the test is for, it's better to `panic!` in those situations.
    - It’s advisable to have your code panic when it’s possible that your code could end up in a `bad state`. The bad state is something that is unexpected, as opposed to something that will likely happen occasionally, like a user entering data in the wrong format. You don't want to carry this bad state throughout the program and instead would prefer it to end through `panic!`.
    - If someone calls your code and passes in values that don’t make sense, the best choice might be to call `panic!` and alert the person using your library to the bug in their code so they can fix it during development.
    - Similarly, `panic!` is often appropriate if you’re calling external code that is out of your control and it returns an invalid state that you have no way of fixing.
    - When your code performs operations on values, your code should verify the values are valid first and panic if the values aren’t valid. This is mostly for safety reasons: attempting to operate on invalid data can expose your code to vulnerabilities.
    - However, having lots of error checks in all of your functions would be verbose and annoying. Fortunately, you can use Rust’s type system (and thus the type checking the compiler does) to do many of the checks for you. If your function has a particular type as a parameter, you can proceed with your code’s logic knowing that the compiler has already ensured you have a valid value. For example, if you have a type rather than an Option, your program expects to have something rather than nothing.
    - Another example is using an unsigned integer type such as u32, which ensures the parameter is never negative.

  - When to call `unwrap()`

    - In case you exactly know that the code won't `panic!`, then it's better to use `unwrap()`, and stop caring about the other possibilities. Here's an Example:

      ```rust
      use std::net::IpAddr;

      // Compile isn't smart enough to see this string is a valid IP address
      // but we are
      let home: IpAddr = "127.0.0.1".parse().unwrap();
      ```

### Generics

- Generics are used to prevent the duplication of **concepts** and are generalized over a type.
- Some examples of generics are `Result<T,E>`, `Option<T>`, `Vec<T>`, and `HashMap<K,V>`.
- Possible Use Cases:
  - You can define an enum or struct which can accomodate different data types.
  - You can define a function which can provide same functionality for different types. For Example, finding the largest element inside a vector of numbers or chars.
- In Rust, declaring generics aren't any slower than using concrete types, because it uses a process called _Monomorphization_ to achieve that. Monomorphization is the process of turning generic code into specific code by filling in the concrete types that are used when compiled.

#### Generics on structs

- To create a generic struct:

  ```rust
  // We used type T to make the struct generic
  // so that it can accomodate any type
  struct Point<T> {
      x: T,
      y: T,
  }

  fn main() {
      let integer = Point { x: 5, y: 10 };
      let float = Point { x: 1.0, y: 4.0 };

      // FAIL: First is i32 and the other is f32, hence different types.
      let wont_work = Point { x: 5, y: 4.0 };
  }
  ```

- To make the `wont_work` to work fine, we'll need to change the code as follows:

  ```rust
  struct Point<T, U> {
      x: T,
      y: U,
  }

  fn main() {
      let integer = Point { x: 5, y: 10 };
      let float = Point { x: 1.0, y: 4.0 };

      let will_work = Point { x: 5, y: 4.0 };
  }
  ```

#### Generics on Enums

- The `Option<T>` enum:

  ```rust
  enum Option<T> {
    Some(T),
    None
  }
  ```

- The `Result<T, E>` enum uses two types:

  ```rust
  enum Result<T, E> {
      Ok(T),
      Err(E),
  }
  ```

#### Generics on Functions

- To use generics on `impl` blocks:

  ```rust
  struct Point<T> {
      x: T,
      y: T,
  }

  // By using T after impl means that
  impl<T> Point<T> {
      fn x(&self) -> &T {
          &self.x
      }
  }

  // impl for just one concrete type
  impl Point<f32> {
      fn distance_from_origin(&self) -> f32 {
          (self.x.powi(2) + self.y.powi(2)).sqrt()
      }
  }

  fn main() {
      let p = Point { x: 5, y: 10 };

      println!("p.x = {}", p.x());
  }
  ```

- To use on `impl` on different types:

  ```rust
  struct Point<X1, Y1> {
      x: X1,
      y: Y1,
  }

  impl<X1, Y1> Point<X1, Y1> {
      fn mixup<X2, Y2>(self, other: Point<X2, Y2>) -> Point<X1, Y2> {
          Point {
              x: self.x,
              y: other.y,
          }
      }
  }

  fn main() {
      let p1 = Point { x: 5, y: 10.4 };
      let p2 = Point { x: "Hello", y: 'c' };

      let p3 = p1.mixup(p2);

      println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
  }
  ```

### Traits

- A _trait_ tells the Rust compiler about functionality a particular type has and can share with other types.
- They are similar to the interfaces in other languages. They are used to define the method signature.
- You may define the code implementations inside the `impl` block of the types that implement that trait.
- It is also possible to define a default implementation and then override it in `impl` block.
- Use cases:

  - For example, you're creating a library that wants to summarize an article or a tweet. We want to implement this shared functionality.
  - We can define a trait to define the interface of this functionality.

    ```rust
    pub trait Summary {
        fn summarize(&self) -> String;
    }
    ```

  - Now each type implementing this trait must provide its own custom behavior for the body of the method.

#### Implementing a trait

- Implementing trait on different types

  ```rust
  pub struct NewsArticle {
      pub headline: String,
      pub location: String,
      pub author: String,
      pub content: String,
  }

  impl Summary for NewsArticle {
      fn summarize(&self) -> String {
          format!("{}, by {} ({})", self.headline, self.author, self.location)
      }
  }

  pub struct Tweet {
      pub username: String,
      pub content: String,
      pub reply: bool,
      pub retweet: bool,
  }

  impl Summary for Tweet {
      fn summarize(&self) -> String {
          format!("{}: {}", self.username, self.content)
      }
  }
  ```

- Using types that implements trait:

  ```rust
  // You'll require to pull both trait along with the desired type
  use aggregator::{Summary, Tweet};

  fn main() {
      let tweet = Tweet {
          username: String::from("horse_ebooks"),
          content: String::from(
              "of course, as you probably already know, people",
          ),
          reply: false,
          retweet: false,
      };

      println!("1 new tweet: {}", tweet.summarize());
  }
  ```

#### Restrictions

- One restriction to note with trait implementations is that we can implement a trait on a type only if at least one of the trait or the type is local to our crate.
  - For example, we can implement standard library traits like `Display` on a custom type like `Tweet` as part of our `aggregator` crate functionality, because the type Tweet is local to our aggregator crate.
  - But we can’t implement external traits on external types. For example, we can’t implement the `Display` trait on `Vec<T>` within our `aggregator` crate, because `Display` and `Vec<T>` are defined in the standard library and aren’t local to our `aggregator` crate.
- This restriction is part of a property of programs called _coherence_, and more specifically the _orphan rule_, so named because the parent type is not present. This rule ensures that other people’s code can’t break your code and vice versa.

#### Default Implementations

- We can define a default implementation by adding code inside the method signatures of traits.

  ```rust
  pub trait Summary {
      fn summarize(&self) -> String {
          String::from("(Read more...)")
      }
  }
  ```

- To use default implementation on a type, we can do that by using empty braces `{}`.

  ```rust
  impl Summary for NewsArticle {}
  ```

- It is possible to keep a trait with a mix of method signatures and method signatures with default implementations.

  ```rust
  pub trait Summary {
      fn summarize_author(&self) -> String;

      fn summarize(&self) -> String {
          format!("(Read more from {}...)", self.summarize_author())
      }
  }
  ```

- Now we only need to require to define `summarize_author` method inside the `impl` block of the type that's implementing the trait.

  ```rust
  impl Summary for Tweet {
      fn summarize_author(&self) -> String {
          format!("@{}", self.username)
      }

      // We do not require to define the summarize() method
      // as we can use the trait's default implementation
  }
  ```

Note: Calling the default implementation from an overriding implementation won't work.

#### Traits as Paramteres

- You can define the `type` of parameters of a function as a trait.
- Then, you can pass any type that implements the specified trait.
- Here's the syntax for that:

  ```rust
  pub fn notify(item: &impl Summary) {
      println!("Breaking news! {}", item.summarize());
  }
  ```

- Code that calls the function with any other type, such as a `String` or an `i32`, won’t compile because those types don’t implement Summary.

- The above syntax is the simpler version of this original syntax, known as "_trait bound syntax_":

  ```rust
  pub fn notify<T: Summary>(item: &T) {
      println!("Breaking news! {}", item.summarize());
  }
  ```

- It is possible to use this syntax like this:

  ```rust
  pub fn notify<T: Summary>(item1: &T, item2: &T) {
  ```

- It is possible to define multiple trait bounds for a single parameter:

  ```rust
  // In both these cases, item must be a type that
  // implements both traits Summary and Display

  // Method 1 ->
  pub fn notify(item: &(impl Summary + Display)) {

  // Method 2 ->
  pub fn notify<T: Summary + Display>(item: &T) {
  ```

- You can use `where` clause to declutter the signature. For Example:

  ```rust
  fn some_function<T, U>(t: &T, u: &U) -> i32
      where T: Display + Clone,
            U: Clone + Debug
  {
  ```

- Similar to function parameters, it is possible to return types that implements traits:

  ```rust
  // Signature says that it'll return any type that implements the trait Summary
  fn returns_summarizable() -> impl Summary {
      // Tweet is some type that implements Summary
      Tweet {
          username: String::from("horse_ebooks"),
          content: String::from(
              "of course, as you probably already know, people",
          ),
          reply: false,
          retweet: false,
      }
  }
  ```

- However, you can only use `impl Trait` if you’re returning a single type. For example:

  ```rust
  // FAIL: Either return NewsArticle or Tweet (only one type that implements Summary)
  fn returns_summarizable(switch: bool) -> impl Summary {
      if switch {
          NewsArticle {
            ...
          }
      } else {
          Tweet {
            ...
          }
      }
  }
  ```

- Finding the largest character of an array of integer or an array of characters using generics and traits:

  ```rust
  // Generic is used as we defined T in the signature, allowing any type to pass
  // Trait bound is specified as PartialOrd is added to the signature, allowing any type that allows comparison, and copy (both i32 and char do)
  fn largest<T: PartialOrd + Copy>(list: &[T]) -> T {
      let mut largest = list[0];

      for &item in list {
          if item > largest {
              largest = item;
          }
      }

      largest
  }

  fn main() {
      let number_list = vec![34, 50, 25, 100, 65];

      let result = largest(&number_list);
      println!("The largest number is {}", result);

      let char_list = vec!['y', 'm', 'a', 'q'];

      let result = largest(&char_list);
      println!("The largest char is {}", result);
  }
  ```

- Using Trait Bounds to Conditionally Implement Methods:

  ```rust
  use std::fmt::Display;

  struct Pair<T> {
      x: T,
      y: T,
  }

  impl<T> Pair<T> {
      fn new(x: T, y: T) -> Self {
          Self { x, y }
      }
  }

  // cmp_display will only run on types bounded by traits Display and PartialOrd, hence works conditionally
  impl<T: Display + PartialOrd> Pair<T> {
      fn cmp_display(&self) {
          if self.x >= self.y {
              println!("The largest member is x = {}", self.x);
          } else {
              println!("The largest member is y = {}", self.y);
          }
      }
  }
  ```

### Lifetimes

Fun Fact: The developers who are programming Rust are constantly programming the patterns into the compiler’s code so the borrow checker could infer the lifetimes in some situations and wouldn’t need explicit annotations. These patterns programmed into Rust’s analysis of references are called the _lifetime elision rules_. Thus, making lifetimes easier to use day by day.

- Lifetime is a way to specify how long the multiple references will live. So, it doesn't make sense to add lifetime to just one reference, they must be multiple.
- Ways to add lifetime specifiers:

  ```rust
  &i32        // a reference
  &'a i32     // a reference with an explicit lifetime
  &'a mut i32 // a mutable reference with an explicit lifetime
  ```

Note: We'll may or may not use lifetimes only when we're dealing with references.

- For example, let’s say we have a function with the parameter `first` that is a reference to an `i32` with lifetime `'a`. The function also has another parameter named `second` that is another reference to an `i32` that also has the lifetime `'a`. The lifetime annotations indicate that the references `first` and `second` must both live as long as that generic lifetime.

- _Every reference_ in Rust has a _lifetime_.
- Here's an exmple of dangling reference:

  ```rust
  // FAIL: Rust prevents dangling references
      {
          let r;                // ---------+-- 'a
                                //          |
          {                     //          |
              let x = 5;        // -+-- 'b  |
              r = &x;           //  |       |
          }                     // -+       | <- x dies but r stores reference of x, hence r stores a dangling referece
                                //          |
          println!("r: {}", r); //          |
      }                         // ---------+
  ```

- Rust won't compile the above code, as it uses a `borrow checker`, to verify whether a reference or borrow is valid or not.
- We may fix it by fixing the lives of variables by declaring them at different places:

  ```rust
      {
          let x = 5;            // ----------+-- 'b
                                //           |
          let r = &x;           // --+-- 'a  |
                                //   |       |
          println!("r: {}", r); //   |       |
                                // --+       |
      }                         // ----------+
  ```

- This code will not compile, it'll require lifetime specifiers:

  ```rust
  // FAIL: Rust can’t tell whether the reference being returned refers to x or y.
  fn longest(x: &str, y: &str) -> &str {
      if x.len() > y.len() {
          x
      } else {
          y
      }
  }
  ```

  ```rust
  // Compiler will ask us to rewrite the signature like this
  fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
  // Lifetimes on function or method parameters are called input lifetimes, and lifetimes on return values are called output lifetimes.
  ```

- The reason why Rust asks us to specify the lifetimes are due to these reasons:

  - We don’t know whether the if case or the else case will execute.
  - We also don’t know the concrete lifetimes of the references that will be passed in.

- When we add the lifetime specifiers as specified by the compiler, it means, the generic lifetime `'a` will get the concrete lifetime that is equal to the smaller of the lifetimes of `x` and `y` (the variables passed in).

Note: Remember, when we specify the lifetime parameters in this function signature, we’re not changing the lifetimes of any values passed in or returned. Rather, we’re specifying that the borrow checker should reject any values that don’t adhere to these constraints. Note that the longest function doesn’t need to know exactly how long `x` and `y` will live, only that some scope can be substituted for `'a` that will satisfy this signature.

- How borrow checker will allow:

```rust
// Works: result is valid until the inner scope ends, string2 and string1 are valid too, hence borrow checker allows
fn main() {
    let string1 = String::from("long string is long");

    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {}", result);
    }
}

// FAILS: The way we've specified lifetimes, result should have a shorter lifetime, equivalent to that of string2. Since, the code doesn't follows the rule, it fails.
fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }
    println!("The longest string is {}", result);
}
```

- In the second case, this is the error that the compiler will throw:

```zsh
  |
6 |         result = longest(string1.as_str(), string2.as_str());
  |                                            ^^^^^^^^^^^^^^^^ borrowed value does not live long enough
7 |     }
  |     - `string2` dropped here while still borrowed
8 |     println!("The longest string is {}", result);
  |                                          ------ borrow later used here
```

- The below code will not compile because even though we’ve specified a lifetime parameter `'a` for the return type, this implementation will fail to compile because the return value lifetime is not related to the lifetime of the parameters at all.

  ```rust
  fn longest<'a>(x: &str, y: &str) -> &'a str {
      let result = String::from("really long string");
      result.as_str()
  }
  ```

- The compiler will throw this error, since Rust will prevent us from creating dangling reference.

  ```zsh
    --> src/main.rs:11:5
     |
  11 |     result.as_str()
     |     ^^^^^^^^^^^^^^^ returns a reference to data owned by the current function
  ```

- In this case, the best fix would be to return an owned data type rather than a reference so the calling function is then responsible for cleaning up the value.

- Rust is improving day by day to not require programmers to use lifetimes in some places. For Example:

  ```rust
  // Even though we're dealing with references in functions,
  // compiler won't ask us to specify lifetimes, it's because
  // rust devs improved the rust compiler so that the borrow
  // checker need not to not ask for lifetimes in this case
  fn first_word(s: &str) -> &str {
      let bytes = s.as_bytes();

      for (i, &item) in bytes.iter().enumerate() {
          if item == b' ' {
              return &s[0..i];
          }
      }

      &s[..]
  }

  // In earlier version (pre-1.0), the signature would've looked like this
  fn first_word<'a>(s: &'a str) -> &'a str {
  ```

#### Rules of lifetimes

- There are 3 rules that compiler follows to verify whether lifetimes are valid or not.

  - **First Rule:** _Each parameter that is a reference gets its own lifetime parameter._
    - A function with one parameter gets one lifetime parameter: `fn foo<'a>(x: &'a i32)`;
    - A function with two parameters gets two separate lifetime parameters: `fn foo<'a, 'b>(x: &'a i32, y: &'b i32);` and so on.
  - **Second Rule:** _If there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters._
    - For Example, `fn foo<'a>(x: &'a i32) -> &'a i32`.
    - There was only one parameter, hence one lifetime for inputs, so the same lifetime was assigned to the output.
  - **Third Rule:** _If there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters._
    - This third rule makes methods much nicer to read and write because fewer symbols are necessary.
    - Please note that this rule only applies to methods (functions that uses `self`), and not to simple functions.

- You can read in detail about How compiler automatically applies lifetimes and the about the rules of lifetimes in [_Lifetime Elision_](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-elision).

#### Lifetimes in Structs and Methods

- Lifetimes in struct. It’s possible for structs to hold references, but in that case we would need to add a lifetime annotation on every reference in the struct’s definition.

  ```rust
  struct ImportantExcerpt<'a> {
      // Since, string slice is a referece, we added lifetime, such that field part and struct lives together
      part: &'a str,
  }

  fn main() {
      let novel = String::from("Call me Ishmael. Some years ago...");
      let first_sentence = novel.split('.').next().expect("Could not find a '.'");
      let i = ImportantExcerpt {
          part: first_sentence,
      };
  }
  ```

- Lifetimes in `impl` blocks:

  ```rust
  // The lifetime parameter declaration after impl and its use after the type name are required,
  // but we’re not required to annotate the lifetime of the reference to self because of the first elision rule.
  impl<'a> ImportantExcerpt<'a> {
      // No need to apply in the method below due to the first elision rule
      fn level(&self) -> i32 {
          3
      }
      // No need to apply in the method below due to the third elision rule
      fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
  }
  ```

#### The static lifetime

- The `'static` is a lifetime which means that this reference can live for the entire duration of the program.
- All string literals have the `'static` lifetime.
- You may use it as shown in the code below:

  ```rust
  let s: &'static str = "I have a static lifetime.";
  ```

Note: You might see suggestions to use the `'static` lifetime in error messages. But before specifying `'static` as the lifetime for a reference, think about whether the reference you have actually lives the entire lifetime of your program or not. You might consider whether you want it to live that long, even if it could. Most of the time, the problem results from attempting to create a dangling reference or a mismatch of the available lifetimes. In such cases, the solution is fixing those problems, not specifying the `'static` lifetime.

### Generic Type Parameters, Trait Bounds, and Lifetimes Together

- You may consider the code below, it prints the type (they type `T` can be filled with any type that implements `Display` trait), also it returns the longest string slice.

```rust
// Generic Type: T
// Trait Bounds: Display
// Lifetime: 'a
use std::fmt::Display;

// Because lifetimes are a type of generic, the declarations of
// the lifetime parameter 'a and the generic type parameter T go
// in the same list inside the angle brackets after the function name.
fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

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

### Iterators

#### Difference between `iter`, `into_iter`, and `iter_mut`

- They all returns iterator, except the way the returns differ.

- Here are the differences:

  - `into_iter`: It yields any of `T`, `&T` or `&mut T`, depending on the context.
  - `iter`: It yields `&T`.
  - `iter_mut`: It yields `&mut T`.

- For more details refer to this [stackoverflow answer](https://stackoverflow.com/questions/34733811/what-is-the-difference-between-iter-and-into-iter).

#### Iterator, Enumerator, Map, Filter

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
- The tests fail when something in the test function panics.

- Here is the table for the logging statistics:

  | Statistic    | Meaning                                                                      |
  | ------------ | ---------------------------------------------------------------------------- |
  | Passed       | Passing Tests                                                                |
  | Failed       | Failing Tests                                                                |
  | Ignored      | Tests that were ignored due to `#[ignore]` attribute.                        |
  | Measured     | This is for benchmark tests that measure performance. (only in nightly Rust) |
  | Filtered Out | While running specific tests, the left out tests are called `filtered`.      |

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

#### Unit Tests

- This annotation `#[cfg(test)]` tells Rust to only run this module on `cargo test`, not when you run `cargo build`.
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

### Test Driven Development

This software development technique follows these steps:

1. Write a test that fails and run it to make sure it fails for the reason you expect.
2. Write or modify just enough code to make the new test pass.
3. Refactor the code you just added or changed and make sure the tests continue to pass.
4. Repeat from step 1!

## Editions in Rust

- When you use, `cargo new`, Rust adds a bit of metadata to your `cargo.toml` about edition under `[package]`.

  ```toml
  edition = "2021"
  ```

- Here are the details about editions:

  | Edition | Description                                                     |
  | ------- | --------------------------------------------------------------- |
  | 2015    | If no version is specified, your project is using this version. |
  | 2018    | The Rust Book is written using this version.                    |
  | 2021    | This is the latest release at the moment.                       |

- Rust has a 6-week release cycle.
- Rust releases small changes very often rather than big changes less often.
- Every 2-3 years, Rust team releases a new edition.
- Rust supports backward compatibility, it means even if you update your Rust software your old code will still compile.
- Here are the following cases you may consider:

  | Edition | Dependency |   Will Compile?    |
  | :-----: | :--------: | :----------------: |
  |  2015   |    2018    | :white_check_mark: |
  |  2018   |    2015    | :white_check_mark: |

- For more details, the [_Edition Guide_](https://doc.rust-lang.org/stable/edition-guide/) is a complete book about editions that enumerates the differences between editions and explains how to automatically upgrade your code to a new edition via cargo fix.

## Appendix

### Installation

For Linux and macOS:

- Download the rustup and install it using:

  ```zsh
  curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
  ```

- You may need to install C compiler, because it'll give you a linker and also because some common Rust packages depend on C code:

  - For macOS:

    ```zsh
    xcode-select --install
    ```

  - For Linux:

    Linux users should generally install GCC or Clang, according to their distribution’s documentation. For example, if you use Ubuntu, you can install the `build-essential` package.

For Windows:

- People should follow [these instructions](https://www.rust-lang.org/tools/install) to install Rust. Also, install [Build Tools for Visual Studio 2019](https://visualstudio.microsoft.com/visual-cpp-build-tools/).

### Rust Basic Commands

- To check version or to verify that Rust is installed properly:

  ```zsh
  rustc --version
  ```

- Update Rust:

  ```zsh
  rustup update
  ```

- Uninstall Rust and rustup:

  ```zsh
  rustup self uninstall
  ```

- Open Rust Docs locally on browser:

  ```zsh
  rustup doc
  ```

- List the rustup toolchain

  ```zsh
  rustup toolchain list
  ```

- Install rustup toolchain

  ```zsh
  rustup toolchain install nightly-x86_64-unknown-linux-gnu
  ```

### Conventions in Rust

- Rust code uses _snake case_ as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words.
- Rust style is to indent with four spaces, not a tab.
- Naming convention for constants is to use all uppercase with underscores between words.

  ```rust
  const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
  ```

- The names of a _type_ in rust uses CamelCase. For example, consider `T` in `Result<T>`.

### Cargo

- The package manager for rust. It does the following things:

  - Manages Rust Projects
  - Download packages or dependencies
  - Build both your code and it's dependencies

- Check Cargo Installation or Version:

  ```bash
  cargo --version
  ```

- Create New Boilerplate Project:

  ```bash
  cargo new {project-name}
  ```

- Create Boilerplate Binary Project (only `main.rs`):

  ```bash
  cargo new {project-name} --bin
  ```

- Create Boilerplate Library Project (for writing tests, contains `lib.rs`):

  ```bash
  cargo new {project-name} --lib
  ```

- Help for Cargo new:

  ```bash
  cargo new --help
  ```

- Build a Project (Installing Dependencies and Compiling):

  ```bash
  cargo build
  ```

- Run a Project (build + run):

  ```bash
  cargo run
  ```

- Running through executable binary:

  ```bash
  ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
  ```

- Compile but don't generate executable (it's just faster than `cargo build`):

  ```bash
  cargo check
  ```

- Build for releases (it's optimized and binaries lives in `target/release`):

  ```bash
  cargo build --release
  ```

- To generate docs of all dependencies of your project and run them in browser:

  ```zsh
  cargo doc --open
  ```

- We can install `cargo-expand` to use cargo libraries system wide.

  ```zsh
  cargo install cargo-expand
  ```

- The cargo expand command

  ```zsh
  cargo expand
  ```

For more information about Cargo, check out [its documentation](https://doc.rust-lang.org/cargo/).

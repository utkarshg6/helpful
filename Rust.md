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

## Basic Concepts

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

#### Re-exporting

- You may use re-exporting, for making it easier to use your crate for other developers. It allows to use the structures directly intead of following the heirarchy in which the crate is designed.

  - Without re-exporting:

    - How structure looks:

      ```rust
      pub mod kinds {
          pub enum PrimaryColor {
            ...
          }

          pub enum SecondaryColor {
            ...
          }
      }

      pub mod utils {
          use crate::kinds::*;

          pub fn mix(c1: PrimaryColor, c2: PrimaryColor) -> SecondaryColor {
              ...
          }
      }
      ```

    - How others will be using it:

      ```rust
      use art::kinds::PrimaryColor;
      use art::utils::mix;

      fn main() {
          let red = PrimaryColor::Red;
          let yellow = PrimaryColor::Yellow;
          mix(red, yellow);
      }
      ```

  - With re-exporting:

    - How structure looks:

      ```rust
      // Here we're re-exporting it for direct use
      pub use self::kinds::PrimaryColor;
      pub use self::kinds::SecondaryColor;
      pub use self::utils::mix;

      pub mod kinds {
          ...
      }

      pub mod utils {
          ...
      }
      ```

    - How others will be using it:

      ```rust
      // Isn't it easier to import now?
      use art::mix;
      use art::PrimaryColor;

      fn main() {
        let red = PrimaryColor::Red;
        let yellow = PrimaryColor::Yellow;
        mix(red, yellow);
      }
      ```

#### Workspaces

- A `workspace` is a set of packages that share the same `Cargo.lock` and output directory.
- You can build your workspace that looks like this:

  ```zsh
  add
  ├── Cargo.lock
  ├── Cargo.toml
  ├── add_one
  │   ├── Cargo.toml
  │   └── src
  │       └── lib.rs
  ├── adder
  │   ├── Cargo.toml
  │   └── src
  │       └── main.rs
  └── target // Notice only one target directory
  ```

- The `cargo.toml` of `add` (the outer one) of the workspace will look like this:

  ```toml
  <!-- Filename: add/Cargo.toml -->
  [workspace]

  members = [
      "adder",
      "add_one",
  ]
  ```

- The workspace has one target directory at the top level, the `adder` package doesn’t have its own `target` directory.
- Even if we were to run `cargo build` from inside the `adder` directory, the compiled artifacts would still end up in `add/target` rather than `add/adder/target`.

- The `cargo.toml` of `adder` will look like this:

  ```toml
  <!-- Filename: add/adder/Cargo.toml -->
  [dependencies]
  add_one = { path = "../add_one" }
  ```

- The `main.rs` in `adder` will look something like this:

  ```rust
  // Filename: add/adder/src/main.rs
  use add_one;

  fn main() {
      let num = 10;
      println!(
          "Hello, world! {} plus one is {}!",
          num,
          add_one::add_one(num)
      );
  }
  ```

- To build the whole workspace, you may run this command from the `add` directory (the outer).

  ```zsh
  $ cargo build
     Compiling add_one v0.1.0 (file:///projects/add/add_one)
     Compiling adder v0.1.0 (file:///projects/add/adder)
      Finished dev [unoptimized + debuginfo] target(s) in 0.68s
  ```

- To run a particular package you may run the following command:

  ```zsh
  $ cargo run -p adder
      Finished dev [unoptimized + debuginfo] target(s) in 0.0s
       Running `target/debug/adder`
  Hello, world! 10 plus one is 11!
  ```

- All the dependencies in different packages will use the same version of the dependency. It is because the `cargo.toml` of the workspace will make only one entry of the dependency. It also saves space and makes all the package compatible with each other, since they'll be using the same version of the dependency.

- To run all test:

  ```zsh
  cargo run test
  ```

- To run test in particular file:

  ```zsh
  cargo test -p add_one
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

## Medium Concepts

### Functional Language Features

Fun Fact: The implementations of closures and iterators are such that runtime performance is not affected. It means as if you've written to an optimized low level code, like in Assembly Language. This is part of Rust’s goal to strive to provide _zero-cost abstractions_.

- Programming in a functional style often includes using functions as values by passing them in arguments, returning them from other functions, assigning them to variables for later execution, and so forth.
- Specifically, functional programming includes:
  - _Closures_: A function-like construct you can store in a variable.
  - _Iterators_: A way of processing a series of elements.

#### Closures

- They are "Anonymous Functions that Can Capture Their Environment".
- An example of closure:

  ```rust
  let expensive_closure = |num| {
      println!("calculating slowly...");
      thread::sleep(Duration::from_secs(2));
      num
  };
  ```

- Why closures don't require type annotations, but functions (`fn`) do?

  - Type annotations are required on functions because they’re part of an explicit interface exposed to your users. Defining this interface rigidly is important for ensuring that everyone agrees on what types of values a function uses and returns.
  - But closures aren’t used in an exposed interface like this: they’re stored in variables and used without naming them and exposing them to users of our library.

- In case, we still want to explicitly define type annotations, we can do it by:

  ```rust
  let expensive_closure = |num: u32| -> u32 {
      println!("calculating slowly...");
      thread::sleep(Duration::from_secs(2));
      num
  };
  ```

- Comparisons for Functions and closures syntax:

  ```rust
  fn  add_one_v1   (x: u32) -> u32 { x + 1 }
  let add_one_v2 = |x: u32| -> u32 { x + 1 };
  let add_one_v3 = |x|             { x + 1 };
  let add_one_v4 = |x|               x + 1  ;
  ```

- Closures will always have only one concrete type:

  ```rust
  // FAIL: Closure inferred two different types of x, which is against the rules
  let example_closure = |x| x;

  let s = example_closure(String::from("hello"));
  let n = example_closure(5);
  ```

- Performing _memoization_ or _lazy evaluation_:

  - We can create a struct that will hold the closure and the resulting value of calling the closure.
  - The struct will execute the closure only if we need the resulting value, and it will cache the resulting value so the rest of our code doesn’t have to be responsible for saving and reusing the result.
  - All closures implement at least one of the traits: `Fn`, `FnMut`, or `FnOnce`.
  - Using this information, we can create a `Cacher`

    ```rust
    // Cacher will store a closure inside calculation
    // and the computed value inside value
    struct Cacher<T>
    where
        T: Fn(u32) -> u32,
    {
        calculation: T,
        value: Option<u32>,
    }
    ```

  - The use of the memoization is that, we can store the closure, that contains computation which takes very long time to finish. Then we can save it's computed value inside the struct, so that we can reuse to computation (thereby preventing expensive computation again and again), as well as update the computed value whenever necessary.
  - We'll need to write an implementation block to make the `Cacher` easier to use:

    ```rust
    impl<T> Cacher<T>
    where
        T: Fn(u32) -> u32,
    {
        fn new(calculation: T) -> Cacher<T> {
            Cacher {
                calculation,
                value: None
            }
        }

        fn value(&mut self, arg: u32) -> u32 {
            match self.value {
                Some(v) => v,
                None => {
                    let v = (self.calculation)(arg);
                    self.value = v;
                    v
                }
            }
        }
    }
    ```

  - The only limitation of this `Cacher` is that it assumes that, it'll only receive one value, that means even if we call the `value()` function multiple with different arguments, it'll still return the same value every time and that value will be the computed value when the closure was called for the first time. Here's the explanation:

    ```rust
    let mut c = Cacher::new(|a| a);

    let v1 = c.value(1); // v1 = 1
    let v2 = c.value(2); // v2 = 1
    let v3 = c.value(3); // v3 = 1
    ```

  - So, here is a better version of the above cacher that can memorize all the arguments and their computed value inside a HashMap, which is also generic. You may refer it's implementation over [here](https://gist.github.com/utkarshg6/c8a5cb39ef89b8f16fcce3098754c001).

- Capturing the Environmet with closures:

  - You can't don the following thing using simple functions:

    ```rust
    // FAIL: Functions can't capture their environment, hence x shouldn't live inside the function
    fn main() {
        let x = 4;

        fn equal_to_x(z: i32) -> bool {
            z == x
        }

        let y = 4;

        assert!(equal_to_x(y));
    }
    ```

  - But, you can easliy do this using closure:

    ```rust
    fn main() {
        let x = 4;

        let equal_to_x = |z| z == x;

        let y = 4;

        assert!(equal_to_x(y));
    }
    ```

  - Closures can capture values from their environment in three ways, which directly map to the three ways a function can take a parameter: taking ownership, borrowing mutably, and borrowing immutably. These are encoded in the three Fn traits as follows:

    - `FnOnce` consumes the variables it captures from its enclosing scope, known as the closure’s environment. To consume the captured variables, the closure must take ownership of these variables and move them into the closure when it is defined. The Once part of the name represents the fact that the closure can’t take ownership of the same variables more than once, so it can be called only once.
    - `FnMut` can change the environment because it mutably borrows values.
    - `Fn` borrows values from the environment immutably.

  - When you create a closure, Rust infers which trait to use based on how the closure uses the values from the environment. All closures implement `FnOnce` because they can all be called at least once. Closures that don’t move the captured variables also implement `FnMut`, and closures that don’t need mutable access to the captured variables also implement `Fn`. In Listing 13-12, the equal_to_x closure borrows x immutably (so equal_to_x has the Fn trait) because the body of the closure only needs to read the value in x.

  - If you want to force the closure to take ownership of the values it uses in the environment, you can use the `move` keyword before the parameter list. This technique is mostly useful when passing a closure to a new thread to move the data so it’s owned by the new thread. The `move` closures may still implement Fn or FnMut, even though they capture variables by move. This is because the traits implemented by a closure type are determined by what the closure does with captured values, not how it captures them. The move keyword only specifies the latter.

  - An example of `move`:

    ```rust
    // FAIL: We tried to print x even though it is moved inside closure
    fn main() {
        let x = vec![1, 2, 3];

        let equal_to_x = move |z| z == x;

        println!("can't use x here: {:?}", x);

        let y = vec![1, 2, 3];

        assert!(equal_to_x(y));
    }
    ```

  - Most of the time when specifying one of the `Fn` trait bounds, you can start with `Fn` and the compiler will tell you if you need `FnMut` or `FnOnce` based on what happens in the closure body.

#### Iterators

- Iterators are lazy in rust, meaning they have no effect until you call methods that consume the iterator to use it up.

  ```rust
  let v1 = vec![1, 2, 3];

  let v1_iter = v1.iter(); // It won't do anything useful until called upon

  for val in v1_iter { // Now, the iterator is called upon and used
      println!("Got: {}", val);
  }
  ```

- To just get the next element stored in iterator:

  ```rust
  let v1 = vec![1, 2, 3];

  // Calling the next() method changes the state of iterator,
  // hence we'll need to use mut in this case
  let mut v1_iter = v1.iter();

  assert_eq!(v1_iter.next(), Some(&1));
  assert_eq!(v1_iter.next(), Some(&2));
  assert_eq!(v1_iter.next(), Some(&3));
  assert_eq!(v1_iter.next(), None);
  ```

- Why is it required to use `mut` when using `next()`, but not when using `for` loop?

  - `next()` - Each call to `next` eats up an item from the iterator. Hence, we need to define it as `mut` to be able to do that.
  - `for` - The loop takes ownership of the iterator and made it mutable behind the scenes. Hence, we don't need to use `mut` there.

- Difference between `iter`, `into_iter`, and `iter_mut`

  - They all return iterator, except the way they return differs. Here are the differences:

    - `into_iter`: It yields any of `T`, `&T` or `&mut T`, depending on the context.
    - `iter`: It yields `&T`.
    - `iter_mut`: It yields `&mut T`.

  - For more details refer to this [stackoverflow answer](https://stackoverflow.com/questions/34733811/what-is-the-difference-between-iter-and-into-iter).

- `Consuming Adaptors`: Some methods inside `Iterator` trait uses `next()`. It means those functions will also eat away the iterator, just like how `next()` does. Here's an example:

  ```rust
  let v1 = vec![1, 2, 3];

  let v1_iter = v1.iter();

  let total: i32 = v1_iter.sum(); // sum() uses the next() and hence will eat away the iterator
  ```

- `Iterator Adaptors`: Some methods inside `Iterator` allows you to change iterators into different kinds of iterators. It is also possible to use `Iterator`, `Enumerator`, `Map`, and `Filter` together. Rust has these functions inside the Iterator Trait.

  ```rust
  let v1: Vec<u32> = vec![0, 1, 2, 3, 4, 5];
  let iterator = v1.iter()
                   .enumerate()
                   .filter(|(i, val)| i % 2 == 0)
                   .map(|(i, val)| val); // On it's own it won't do anything, because iterators are lazy

  // You can either print them one by one using for loop (remember it'll consume the iterator)
  for val in iterator {
      println!("{}", val);
  }

  // Or you can collect them inside a vector, make sure you explicitly specify the type (`Vec<_>`) too.
  let new_vector: Vec<_> = iterator.collect();
  println!("New Vector: {:?}")
  ```

- Creating your own iterator:

  - You'll need to implement `Iterator` trait on your type.
  - You'll only need to define one function, that is `next()`, it'll be sufficient.

    ```rust
    struct Counter {
        count: u32,
    }

    impl Counter {
        fn new() -> Self {
            Self {
                count: 0,
            }
        }
    }

    impl Iterator for Counter {
        type Item = u32;

        fn next(&mut self) -> Option<Self::Item> {
            if self.count < 5 {
                self.count += 1;
                Some(self.count)
            } else {
                None
            }
        }
    }

    fn main() {
        let counter = Counter::new();
        for val in counter {
            println!("{:?}", val);
        }

        // Since we have next() method we can use any default implementation of the Iterator trait
        let sum: u32 = Counter::new()
            .zip(Counter::new().skip(1)) // Skips first element only and generate pairs { (1,2) (2,3) (3,4) (4,5) } because (5,None) is ignored
            .map(|(a, b)| a * b) // [2, 6, 12, 20]
            .filter(|x| x % 3 == 0) // [6, 12]
            .sum(); // 18
    }
    ```

- Which is faster, `for` loop or `iterator adapters`?

  - Here are the benchmarks:

    ```zsh
    test bench_search_for  ... bench:  19,620,300 ns/iter (+/- 915,700)
    test bench_search_iter ... bench:  19,234,900 ns/iter (+/- 657,200)
    ```

  - Iterators, although a high-level abstraction, get compiled down to roughly the same code as if you’d written the lower-level code yourself. Iterators are one of Rust’s `zero-cost abstractions`, which means that using the abstraction imposes no additional runtime overhead.

### Smart Pointers

- Differences between Pointer and Smart Pointer:

| Pointer                                                                           | Smart Pointer                                                                                                                               |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| A pointer is a general concept for a variable that contains an address in memory. | Smart pointers, on the other hand, are data structures that not only act like a pointer but also have additional metadata and capabilities. |
| References are pointers that only borrow data.                                    | Smart pointers _own_ the data they point to.                                                                                                |
| The most common kind of pointer in Rust is a reference and is indicated by `&`.   | The commonly used smart pointers are `String` and `Vec<T>`.                                                                                 |

- Smart pointers are usually implemented using `structs`.
- The characteristic that distinguishes a smart pointer from an ordinary struct is that smart pointers implement the `Deref` and `Drop` traits.
  - `Deref`: Allows an instance of the smart pointer struct to behave like a reference so you can write code that works with either references or smart pointers.
  - `Drop`: Allows you to customize the code that is run when an instance of the smart pointer goes out of scope.

#### `Box<T>`

- For allocating values on Heap.
- Box allows you to store _data on heap_ and the _pointer to the heap data on stack_.
- You’ll use them most often in these situations:
  - When you have a type whose size can’t be known at compile time and you want to use a value of that type in a context that requires an exact size
  - When you have a large amount of data and you want to transfer ownership but ensure the data won’t be copied when you do so
  - When you want to own a value and you care only that it’s a type that implements a particular trait rather than being of a specific type
- Once Box goes out of scope, the deallocation happens for the box (stored on the stack) and the data it points to (stored on the heap).

- Using `Box<T>` for the recursive call:

  - Let's try to create an enum which will create it's variant recursively:

    ```rust
    // FAIL: While computing Size for Cons, Rust will detect an inifinite memory allocation
    enum List {
        Cons(i32, List),
        Nil,
    }

    use crate::List::{Cons, Nil};

    fn main() {
        let list = Cons(1, Cons(2, Cons(3, Nil)));
    }
    ```

  - When Rust will try to recognize the size, it'll recognize that it is an infinite loop:

    <img src="https://doc.rust-lang.org/book/img/trpl15-01.svg" alt="Infinite List Containing Infinite Cons Value" width="400"/>

  - Now, to make it easier for Rust to identify the size of enum at compile time, we can use `Box<T>` for the recursive call. Since `Box<T>` is a pointer, Rust will need not to find the size what's inside of it, instead just allocate the memory for it's pointer.

    ```rust
    enum List {
        Cons(i32, Box<List>),
        Nil,
    }

    use crate::List::{Cons, Nil};

    fn main() {
        let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
    }
    ```

  - Conceptually, we still have a list, created with lists “holding” other lists, but this implementation is now more like placing the items next to one another rather than inside one another.

    <img src="https://doc.rust-lang.org/book/img/trpl15-02.svg" alt="List that is not infinitely sized" width="400"/>

- Since `Box<T>` implements the `Deref` trait, so you can use the `*` operator to dereference it.

#### `Deref` Trait

- Note that the `*` operator is replaced with a call to the deref method and then a call to the `*` operator. It means that `*y`, translates into:

  ```rust
  *(y.deref())
  ```

- We're trying to re-create `Box<T>` and it's capabilities to dereference itself. One important thing to notice, here we're only mimicing the dereferencing because the data doesn't actually get stored on heap.

  ```rust
  use std::ops::Deref;

  struct MyBox<T>(T);

  impl<T> MyBox<T> {
    fn new(x: T) -> MyBox<T>  {
        MyBox(x)
    }
  }

  impl<T> Deref for MyBox<T> {
      type Target = T;

      fn deref(&self) -> &Self::Target {
          &self.0
      }
  }
  ```

#### Deref Coercion

- Deref coercion can convert `&String` to `&str`. It's possible because `String` implements the `Deref` trait such that it returns `&str`.
- They are meant for the arguments of functions and methods. The ease is that, you can pass `&String` into a function that accepts `&str`:

  ```rust
  fn hello(name: &str) {
      println!("Hello, {}!", name);
  }

  fn main() {
    let name = String::from("Bob");
    hello(&name);
  }
  ```

- Deref coercion was added to Rust so that programmers writing function and method calls don’t need to add as many explicit references and dereferences with `&` and `*`.

- How does Rust automatically converts `&String` to `&str`?

  - It happens because `Deref` trait is implemented.
  - Rust simplifies all the deref implementations.

- Here's an even complex example of deref coercion, using the `MyBox`, that we created earlier:

  - Instead of calling this:

    ```rust
    fn main() {
        let m = MyBox::new(String::from("Rust"));
        hello(&(*m)[..]);
    }
    ```

  - Here we are manually converting:

    ```rust
    (*m) => MyBox<String> -> String
    [..] => String -> str
    & => str -> &str
    ```

  - We can just call this:

    ```rust
    fn main() {
        let m = MyBox::new(String::from("Rust"));
        hello(&m);
    }
    ```

  - Rust simplifies the deref implementations by calling the `deref()` again and again. First It'll call `deref()` for `MyBox` then for `String`.

    ```rust
    &MyBox<String> -> &String -> &str
    ```

Note: When the Deref trait is defined for the types involved, Rust will analyze the types and use `Deref::deref` as many times as necessary to get a reference to match the parameter’s type. The number of times that `Deref::deref` needs to be inserted is resolved at compile time, so there is no runtime penalty for taking advantage of deref coercion!

- Rust does deref coercion when it finds types and trait implementations in three cases:

  - From `&T` to `&U` when `T: Deref<Target=U>`
  - From `&mut T` to `&mut U` when `T: DerefMut<Target=U>`
  - From `&mut T` to `&U` when `T: Deref<Target=U>`

Note: The first case states that if you have a `&T`, and `T` implements `Deref` to some type `U`, you can get a `&U` transparently. the second case is similar. The third case is a bit different as mutable reference changes into immutable reference, though vice versa is not true.

#### `Drop` Trait

- In Rust, you can specify that a particular bit of code be run whenever a value goes out of scope, and the compiler will insert this code automatically.
- As a result, you don’t need to be careful about placing cleanup code everywhere in a program that an instance of a particular type is finished with—you still won’t leak resources!

  ```rust
  struct CustomSmartPointer {
      data: String,
  }

  impl Drop for CustomSmartPointer {
      fn drop(&mut self) {
          println!("Dropping CustomSmartPointer with data `{}`!", self.data);
      }
  }

  fn main() {
      let c = CustomSmartPointer {
          data: String::from("my stuff"),
      };
      let d = CustomSmartPointer {
          data: String::from("other stuff"),
      };
      println!("CustomSmartPointers created.");
  }
  // Output -
  // CustomSmartPointers created.
  // Dropping CustomSmartPointer with data `other stuff`!
  // Dropping CustomSmartPointer with data `my stuff`!
  ```

- Notice that, variables are dropped in the reverse order, `d` was dropped before `c`.
- There might be some cases when you want to manually drop the Smart Pointer, instead of waiting for the scope to end. For example, you want to release the lock so that other code in the same scope can acquire the lock.

  - First thing is that, you can't call the `drop()` from the `Drop` trait.

    ```rust
    // FAIL: This is not allowed in Rust, compiler will throw "explicit destructor calls not allowed"
    fn main() {
        let c = CustomSmartPointer {
            data: String::from("some data"),
        };
        println!("CustomSmartPointer created.");
        c.drop();
        println!("CustomSmartPointer dropped before the end of main.");
    }
    ```

    ```zsh
      --> src/main.rs:16:7
       |
    16 |     c.drop();
       |     --^^^^--
       |     | |
       |     | explicit destructor calls not allowed
       |     help: consider using `drop` function: `drop(c)`
    ```

  - Compiler uses term `destructor`, which is the general programming term for a function that cleans up an instance. It is analogous to the word `constructor`.
  - The reason that compiler doesn't allows us to do that, is to prevent the _double free error_.
  - The alternative is to use `drop()` from `std::mem::drop`, good thing is it's already in the `prelude`, so you don't need to import it.

    ```rust
    fn main() {
        let c = CustomSmartPointer {
            data: String::from("some data"),
        };
        println!("CustomSmartPointer created.");
        drop(c); // Notice, we're passing it as an argument
        println!("CustomSmartPointer dropped before the end of main.");
    }

    // Ouput -
    // CustomSmartPointer created.
    // Dropping CustomSmartPointer with data `some data`!
    // CustomSmartPointer dropped before the end of main.
    ```

  - It solves the _double free error_ through the ownership rules, as we pass it as an argument.

#### `Rc<T>`

- Also known as _Reference Counted Smart Pointer_, it allows multiple ownership.
- It does that by keeping the count of references. When the count becomes 0, it means that there are no references linked to data, so it's safe to clean.
- Imagine `Rc<T>` as a TV in a family room. When one person enters to watch TV, they turn it on. Others can come into the room and watch the TV. When the last person leaves the room, they turn off the TV because it’s no longer being used. If someone turns off the TV while others are still watching it, there would be uproar from the remaining TV watchers!
- Use case:

  - We want to allocate data on heap and we want multiple parts of our code to read it.
  - The problem is that we don't know which part will stop reading it last, that's why we can't make someone as an owner.
  - For those cases, `Rc<T>` will help us, it'll save us from deciding someone as owner and will allow multiple parts to read it at the same time.

- With `Rc<T>` it is possible to create two lists that both share ownership of a third list.

  <img src="https://doc.rust-lang.org/book/img/trpl15-03.svg" alt="List that is not infinitely sized" width="400"/>

  - Trying to do this with `Box<T>` fails:

    ```rust
    enum List {
        Cons(i32, Box<List>),
        Nil,
    }

    use crate::List::{Cons, Nil};

    fn main() {
        let a = Cons(5, Box::new(Cons(10, Box::new(Nil))));
        let b = Cons(3, Box::new(a));
        let c = Cons(4, Box::new(a));
    }
    ```

  - We can also use references with lifetimes to solve this problem, but `Rc<T>` is better here.

    ```rust
    enum List {
        Cons(i32, Rc<List>),
        Nil,
    }

    use crate::List::{Cons, Nil};
    use std::rc::Rc;

    fn main() {
        let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
        let b = Cons(3, Rc::clone(&a)); // Notice that we don't need to use Rc<T> here, since no one will be owning b and c
        let c = Cons(4, Rc::clone(&a)); // Also notice that we're using Rc::clone() and passing reference to create owners
    }
    ```

  - `Rc::clone()` never makes deep copy, unlike `clone()`. `Rc::clone()` only increments the reference count, which doesn’t take much time.
  - To Increase Count: `Rc::clone()`, To Decrease Count: `Drop` does when the variable goes out of scope.

Note: `Rc<T>` can only allow multiple owners to read data and not to mutate it. For interior mutability there is another Smart Pointer named `RefCell<T>`.

#### `RefCell<T>`

- Allows interior mutability to the immutable data.
- Interior mutability is a design pattern in Rust that allows you to mutate data even when there are immutable references to that data; normally, this action is disallowed by the borrowing rules.
- It uses `unsafe` rust code to function.

- An example:

- Consider one `trait` named `Messanger` and another `struct` named `LimitTracker`.

```rust
pub trait Messenger {
    fn send(&self, msg: &str);
}

pub struct LimitTracker<'a, T: Messenger> {
  messenger: &'a T,
  value: usize;
}

impl<'a, T> LimitTracker<'a, T>
where
    T: Messenger,
{
    pub fn new(messenger: &T, max: usize) -> LimitTracker<T> {
      ...
    }

    pub fn set_value(&mut self, value: usize) { // Problem 1: We want mutable reference of self, but it includes immutable reference to messenger
      self.value = value;

      if (value > 100) {
        self.messenger.send("Error: You are over your quota!"); // Problem 2: send() is an immutable function in trait, but self should be mutable.
      }
    }
}
```

- `LimitTracker` takes in a reference of `struct` that implements `Messenger`, so that it can store it as one of it's field.
- Inside `LimitTracker`, the problem is that `set_value()` takes **mutable reference** of `self`, but the `messenger` is an immutable reference and it's function send is also `immutable`. So, how can we test this `set_value()`?

- This will fail to compile:

  ```rust
  // FAIL: send() is required to be mutable by LimitTracker but immutable due to trait Messenger
  #[cfg(test)]
  mod tests {
      use super::*;

      struct MockMessenger {
          sent_messages: Vec<String>,
      }

      impl MockMessenger {
          fn new() -> MockMessenger {
              MockMessenger {
                  sent_messages: vec![], // This is immutable
              }
          }
      }

      impl Messenger for MockMessenger {
          fn send(&self, message: &str) {
               // We're trying to push on immutbale field, we also can't make send() mutable
              self.sent_messages.push(String::from(message));
          }
      }

      #[test]
      fn it_sends_an_over_75_percent_warning_message() {
          let mock_messenger = MockMessenger::new();
          let mut limit_tracker = LimitTracker::new(&mock_messenger, 100);

          limit_tracker.set_value(80);

          assert_eq!(mock_messenger.sent_messages.len(), 1);
      }
  }
  ```

- Here's the solution using `RefCell<T>`:

  ```rust
  #[cfg(test)]
  mod tests {
      use super::*;
      use std::cell::RefCell;

      struct MockMessenger {
          // RefCell will make sent_messages mutable even though
          // it's parent MockMessenger can seem immutable to others
          sent_messages: RefCell<Vec<String>>,
      }

      impl MockMessenger {
          fn new() -> MockMessenger {
              MockMessenger {
                  // Now, RefCell will wrap the vector and will allow it to be mutable
                  // at places where it's parent is asked to be immutable
                  sent_messages: RefCell::new(vec![]),
              }
          }
      }

      impl Messenger for MockMessenger {
          fn send(&self, message: &str) {
              // MockMessenger will seem immutable to send() but
              // sent_messages is mutable, and items can be pushed into it
              self.sent_messages.borrow_mut().push(String::from(message)); // borrow_mut() will generate mutable reference for push()
          }
      }

      #[test]
      fn it_sends_an_over_75_percent_warning_message() {
          // --snip--

          assert_eq!(mock_messenger.sent_messages.borrow().len(), 1); // borrow() will generate the immutable reference, since we're only reading
      }
  }
  ```

- We use the `&` and `&mut` syntax with references. With `RefCell<T>`, we use the `borrow()` and `borrow_mut()` methods and they return `Ref<T>` and `RefMut<T>` respectively. They both implement `Deref` trait.

- `RefCell<T>` lets us have many immutable borrows or one mutable borrow at any point in time. It keeps a count of whenever we call the `borrow()`.
- In case of an error, it won't be just a compile error, but will appear on Runtime, and will cause a panic.

#### Differences between `Box<T>`, `Rc<T>` and `RefCell<T>`

| Property                 | `Box<T>`                        | `Rc<T>`                            | `RefCell<T>`                       |
| ------------------------ | ------------------------------- | ---------------------------------- | ---------------------------------- |
| Ownership                | Single Ownership                | Multiple Ownership                 | Single Ownership                   |
| Mutability of Inner Data | Immutable or Mutable            | Only Immutable                     | Immutable or Mutable               |
| Borrowing Rules Check    | Compiled Time (compiler errors) | Compiled Time (compiler errors)    | Runtime (panics at runtime)        |
| Multithreading           |                                 | Only for Single Threaded Scenarios | Only for Single Threaded Scenarios |

#### Using `RefCell<T>` with `Rc<T>`

- It'll give you superpowers.
- Now you can have a value that has multiple owners and can also mutate.
- To use it, you'll have to wrap it like this, `Rc<RefCell<T>>`.
- Here's our modified `Cons` list:

  ```rust
  enum List {
      Cons(Rc<RefCell<i32>>, Rc<List>),
      Nil,
  }
  ```

- Now, we can have a list like this:

  ```zsh
  b --|
      a---Nil
  c --|
  ```

- Here, `a` can have multiple owners `b` and `c`, and it's value can also mutate.

  ```rust
  fn main() {
      let value = Rc::new(RefCell::new(5)); // Created a value that can have multiple owners and can also mutate

      let a = Rc::new(Cons(Rc::clone(&value), Rc::new(Nil))); // Make a such that it can be owned by multiple people

      let b = Cons(Rc::new(RefCell::new(3)), Rc::clone(&a)); // Make b the owner of a
      let c = Cons(Rc::new(RefCell::new(4)), Rc::clone(&a)); // Made c the owner of a

      *value.borrow_mut() += 10; // Rc -> RefCell -> &mut -> inner_element, then 10 is added to the inner_element in place

      println!("a after = {:?}", a); // Value of a: Cons(RefCell { value: 15 }, Nil)
      println!("b after = {:?}", b); // Value of b: Cons(RefCell { value: 3 }, Cons(RefCell { value: 15 }, Nil))
      println!("c after = {:?}", c); // Value of c: Cons(RefCell { value: 4 }, Cons(RefCell { value: 15 }, Nil))
  }
  ```

- Other Types to provide interior mutability:
  - `Cell<T>`: It copies the data instead of giving references.
  - `Mutex<T>`: It provides interior mutability that's safe to use in multiple threads.

#### Memory Leak

- When we accidentally create memory that is never cleaned up is called _Memory Leak_.
- Rust’s memory safety guarantees make it difficult, but not impossible.
- Rust allows memory leaks by using `Rc<T>` and `RefCell<T>`.
- By using both of them together, it's possible to create a _reference cycle_.
- A _reference cycle_ happens when reference of `a` is owned by `b` and reference of `b` is owned by `a`.
- First of all, this will cause an infinite loop of references.
- Also, it'll be impossible for Rust to `Drop` the values of `a` and `b`, as their reference count will never be zero.
- This is one of Rust's limitations, and is referred to the _problem of Memory Leak_.

- Reference Cycle in Action:

  ```rust
  use crate::List::{Cons, Nil};
  use std::cell::RefCell;
  use std::rc::Rc;

  #[derive(Debug)]
  enum List {
      Cons(i32, RefCell<Rc<List>>), // We can replace the list in place now
      Nil
  }

  impl List {
      fn tail(&self) -> Option<&RefCell<Rc<List>>> {
          match self {
              Cons(_, item) => Some(item),
              Nil => None,
          }
      }
  }

  fn main() {
      let a = Rc::new(Cons(5, RefCell::new(Rc::new(Nil)))); // a = (5, Nil)


      let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a)))); // b = (10, a)

      if let Some(link) = a.tail() {
          *link.borrow_mut() = b; // a = (5, b)
      }

      // At this point, Rc Count of a: 2, b: 2

      // This will try to print the lists infinitely
      // and then will crash with the stack overflow error
      println!("a next item = {:?}", a.tail());
  }

  // In case, we comment out the println!()
  // At the end of scope, Rust will try to decrease the count of references
  // Rc Count of b will decrease to 1
  // Rc Count of a will decrease to 1
  // Neither a nor b will be dropped as their count is not 0 and
  // will still stay on the heap, causing Memory Leak
  ```

- You may take help of this diagram to understand better:

  <img src="https://doc.rust-lang.org/book/img/trpl15-04.svg" alt="List that is not infinitely sized" width="400"/>

Important Lesson:
You can't rely on Rust's memory safety while using `RefCell<T>` and `Rc<T>` together, or another combination with interior mutability, as it may cause the problem of _Memory Leak_.

Solutions to prevent Reference Cycles:

- Use Automated tests, Code reviews, and other Software development practices to minimize.
- Reorganize data structure so that some references express ownership and some references don't.

| Strong Count                                               | Weak Count                                                                             |
| ---------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Rust will only drop an element if this count becomes zero. | Rust will drop an element in case it gets out of scope, even if the count is not zero. |
| `Rc<T>` uses strong count.                                 | `Weak<T>` uses weak count.                                                             |
| It's hard to prevent reference cycle.                      | It's easier to prevent reference cycle.                                                |

- Preventing Reference Cycle using `Weak<T>`:

  ```rust
  use std::cell::RefCell;
  use std::rc::{Rc, Weak};

  #[derive(Debug)]
  struct Node {
      value: i32,
      parent: RefCell<Weak<Node>>, // Child won't own it's parent
      children: RefCell<Vec<Rc<Node>>>,
  }

  fn main() {
      let leaf = Rc::new(Node {
          value: 3,
          parent: RefCell::new(Weak::new()),
          children: RefCell::new(vec![])
      });

      let branch = Rc::new(Node {
          value: 5,
          parent: RefCell::new(Weak::new()),
          children: RefCell::new(vec![Rc::clone(&leaf)])
      });

      // The downgrade() changes Rc -> Weak
      *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

      // The upgrade() returns Some() or None, representing the value
      println!("Leaf parent: {:?}", leaf.parent.borrow().upgrade());
  }
  ```

- The output will look like this:

  ```zsh
  leaf parent = Some(Node { value: 5, parent: RefCell { value: (Weak) },
  children: RefCell { value: [Node { value: 3, parent: RefCell { value: (Weak) },
  children: RefCell { value: [] } }] } })
  ```

- Notice, at some places only `Weak` is there, and the whole element is not printed. This is Rust's way of preventing infinite output.
- This lack of inifinite output, indicates that reference cycle is prevented.

- Here's another example, using the same struct to show how strong count and weak count will change:

  ```rust
  fn main() {
      let leaf = Rc::new(Node {
          value: 3,
          parent: RefCell::new(Weak::new()),
          children: RefCell::new(vec![]),
      });

      // leaf strong = 1, weak = 0

      {
          let branch = Rc::new(Node {
              value: 5,
              parent: RefCell::new(Weak::new()),
              children: RefCell::new(vec![Rc::clone(&leaf)]),
          });

          // Since Rc is downgrading branch, branch's weak count will increase by 1
          *leaf.parent.borrow_mut() = Rc::downgrade(&branch);

          // branch strong = 1, weak = 1

          // leaf strong = 2, weak = 0
      }

      // leaf strong = 1, weak = 0
  }
  ```

### Concurrency

- Difference between Concurrency and Parallel programming:

| Concurrent Programming                                  | Parallel Programming                                       |
| ------------------------------------------------------- | ---------------------------------------------------------- |
| Where different parts of program execute independently. | Where different parts of program execute at the same time. |

#### Using Threads

- OS manages multiple processes at once.
- An executed program's code is run in a process.
- You can write programs such that there are indpendent pieces of code that run simultaneously.
- The features that run these parts simultaneously are called _threads_.
- Threads can run simultaneously, there’s no inherent guarantee about the order in which parts of your code on different threads will run. This causes the following problems:
  - **Race conditions**: Where threads are accessing data or resources in an inconsistent order.
  - **Deadlocks**: Where two threads are waiting for each other to finish using a resource the other thread has, preventing both threads from continuing.
  - **Bugs**: Hard to reproduce bugs, and only happens in certain situations.
- Many operating systems provide an API for creating new threads. This model where a language calls the operating system APIs to create threads is sometimes called **_1:1_**, meaning _1 OS Thread / 1 Language Thread_. The rust standard library provides the implementation for only _1:1_.

- Creating a new thread with `spawn`:

  ```rust
  use std::thread;
  use std::time::Duration;

  fn main() {
    thread::spawn(|| {
      for i in 1..10 {
        println!("hi number {} from the spawned thread", i);
        thread::sleep(Duration::from_millis(1));
      }
    });

    for i in 1..5 {
      println!("hi number {} from the main thread", i);
      thread::sleep(Duration::from_millis(1));
    }
  }
  ```

- The output will be:

  ```zsh
  hi number 1 from the main thread
  hi number 1 from the spawned thread
  hi number 2 from the main thread
  hi number 2 from the spawned thread
  hi number 3 from the main thread
  hi number 3 from the spawned thread
  hi number 4 from the main thread
  hi number 4 from the spawned thread
  hi number 5 from the spawned thread
  ```

- The spawned thread will automatically die as the main thread ends.
- That's why spawned thread ran 5 times, 4 times same as main thread and the 5th time, which is exectued to break the condition for the main thread's for loop condition.
- Which thread will execute first is not guaranteed, you may notice in our case, the main thread runs first, even though according to the code, the spawned thread should have ran first.
- In fact, there is not even a guarantee that this spwaned thread will even run at all.
- We can make sure that the spawned thread will definitely run and will execute completely, by using the `join()`

  ```rust
  use std::thread;
  use std::time::Duration;

  fn main() {
      // Let's store the thread in a variable
      let handle = thread::spawn(|| {
          for i in 1..10 {
              println!("hi number {} from the spawned thread!", i);
              thread::sleep(Duration::from_millis(1));
          }
      });

      for i in 1..5 {
          println!("hi number {} from the main thread!", i);
          thread::sleep(Duration::from_millis(1));
      }

      // This will make sure that the spawned thread
      // finishes before the main thread ends
      handle.join().unwrap();
  }
  ```

- The two threads will now continue alternating, but the main thread will wait because of the call to `handle.join()` and will not end until the spawned thread is finished.
- It is very important, where you call the `handle.join()`, as it may create an unexpected behaviour:

  ```rust
  use std::thread;
  use std::time::Duration;

  fn main() {
      let handle = thread::spawn(|| {
          for i in 1..10 {
              println!("hi number {} from the spawned thread!", i);
              thread::sleep(Duration::from_millis(1));
          }
      });

      handle.join().unwrap();

      for i in 1..5 {
          println!("hi number {} from the main thread!", i);
          thread::sleep(Duration::from_millis(1));
      }
  }
  ```

- This will give us this output:

  ```zsh
  hi number 1 from the spawned thread!
  hi number 2 from the spawned thread!
  hi number 3 from the spawned thread!
  hi number 4 from the spawned thread!
  hi number 5 from the spawned thread!
  hi number 6 from the spawned thread!
  hi number 7 from the spawned thread!
  hi number 8 from the spawned thread!
  hi number 9 from the spawned thread!
  hi number 1 from the main thread!
  hi number 2 from the main thread!
  hi number 3 from the main thread!
  hi number 4 from the main thread!
  ```

- So, make sure that you're calling the `handle.join()` to prevent undesired behaviour.

- When we use closure, Rust will infer that we want to only borrow the variable.

  ```rust
  use std::thread;

  fn main() {
      let v = vec![1, 2, 3];

      // Notice, here v is only borrowed here,
      // it's possible that the closure may outlive
      // and v may die early, so Rust will throw us
      // error, and will ask us to use move
      let handle = thread::spawn(|| {
          println!("Here's a vector: {:?}", v);
      });

      handle.join().unwrap();
  }
  ```

- So, we need to expicitly add the `move` keyword, to tell Rust to transfer ownership of `v` to the closure.

  ```rust
  use std::thread;

  fn main() {
      let v = vec![1, 2, 3];

      let handle = thread::spawn(move || {
          println!("Here's a vector: {:?}", v);
      });

      // Now, we cannot use v over here, inside the main thread for any reason

      handle.join().unwrap();
  }
  ```

#### Using Message Passing to Transfer Data Between Threads

_“Do not communicate by sharing memory; instead, share memory by communicating.” - Go Language Documentation_

- Rust sends messages between threads to accomplish concurrency.
- Rust uses _channel_ for the message-sending concurrency (it works similar to a river stream), it has two parts:
  - _Transmitter_: The upstream location
  - _Receiver_: The downstream location
- A channel is said to be _closed_ if either the transmitter or receiver half is dropped.
- You may create a channel just like this:

  - A channel can have multiple producer of values (multiple sources of river), but only 1 consumer of those values (all rivers will mix into one river).
  - A channel produces it's two parts, inside a tuple and are abbreviated as `tx` and `rx`, for transmitter and receiver respectively.

    ```rust
    // mpsc stands for multiple producer, single consumer
    use std::sync::mpsc;
    use std::thread;

    fn main() {
        let (tx, rx) = mpsc::channel();

        thread::spawn(move || {
            let val = String::from("hi");
            // We'll send the value to the receiver's end
            // and in case there's a problem at receiving
            // end, it'll thrown an error and cause a panic
            tx.send(val).unwrap();
        });

        // The recv()
        let received = rx.recv().unwrap();
        println!("Got: {}", received);
    }
    ```

- Ways to receive the values from the channel:

  - `recv()`: It'll block the main thread’s execution and wait until a value is sent down the channel.
  - `try_recv()`: This method **doesn't block**, but may not contain any value for some time. So, you'll need to call this every so often, by writing a loop.

- Sending and Receiving multiple values:

  ```rust
  use std::sync::mpsc;
  use std::thread;
  use std::time::Duration;

  fn main() {
      let (tx, rx) = mpsc::channel();

      let tx1 = tx.clone();
      thread::spawn(move || {
          let vals = vec![
              String::from("hi"),
              String::from("from"),
              String::from("the"),
              String::from("thread"),
          ];

          for val in vals {
              tx1.send(val).unwrap();
              thread::sleep(Duration::from_secs(1));
          }
      });

      thread::spawn(move || {
          let vals = vec![
              String::from("more"),
              String::from("messages"),
              String::from("from"),
              String::from("you"),
          ];

          for val in vals {
              tx.send(val).unwrap();
              thread::sleep(Duration::from_secs(1));
          }
      });

      // We’re not calling the recv function explicitly anymore:
      // When the channel is closed, iteration will end.
      for received in rx {
          println!("Got: {}", received);
      }
  }
  ```

#### Shared-State Concurrency

- Shared memory concurrency is like multiple ownership: multiple threads can access the same memory location at the same time.
- We can use _Mutex_ to allow access to data from one thread at a time.

##### Mutex

- _Mutex_ is an abbreviation of _Mutual Exclusion_.
- It locks the data such that others can use. Lock is a data structure that keeps track of who currently has exclusive access to the data.
- These are the rules that you'll have to follow while using a Mutex:
  - You must attempt to acquire the lock before using the data.
  - When you’re done with the data that the mutex guards, you must unlock the data so other threads can acquire the lock.
- Here's an example:

  ```rust
  use std::sync::Mutex;

  fn main() {
      let m = Mutex::new(5);

      {
          let mut num = m.lock() // It'll block the old thread, until we unlock the mutex
                         .unwrap(); // lock() may fail if the old thread panics, so unwrap() will also panic the current thread
          *num = 6; // Mutex returns a Smart Pointer named MutexGuard, that's why we need to dereference it to change it's value
      } // MutexGuard has a Drop trait implementation, which automatically unlocks the mutex when it goes out of scope

      println!("m = {:?}", m);
  }
  ```

- Here's an example wehere the varaible `counter` will be shared among 10 threads, where each of them will try to increment it by 1.

  - Why can't we directly use `Mutex` within multiple threads?
    - The threads use `move`, which moves the ownership of variable to the thread.
    - Rust won't allow us to move the ownership of lock counter in multiple threads.
  - Why can't we use `Rc<T>`, to provide multiple ownership to individual threads?
    - `Rc<T>` is not safe to share across threads. It is possible if we use `Rc<T>` in multiple threads, then both threads might update the reference count at same time.
    - It doesn’t use any concurrency primitives to make sure that changes to the count can’t be interrupted by another thread.
    - That could lead to Wrong Counts and Memory Leak.
  - What do we need then?
    - What we need is a type exactly like `Rc<T>` but one that makes changes to the reference count in a thread-safe way.
    - Fortunately, we have `Arc<T>` (atomically referenced counter), it is almost like `Rc<T>`, except the counts are maintained atomically.
    - Atomics are primitives that are safe to share across threads.

  ```rust
  use std::sync::{Arc, Mutex};
  use std::thread;

  fn main() {
      // Mutex is used to lock a variable so that other thread can use
      // Arc provides multiple ownership like Rc<T> and it is thread safe
      let counter = Arc::new(Mutex::new(0)); // Notice counter is immutable, it's because Mutex provides interior mutability, similar to RefCell
      let mut handles = vec![];

      for _ in 1..=10 {
          let counter = Arc::clone(&counter);
          let handle = thread::spawn(move || {
              let mut value = counter.lock().unwrap();
              *value += 1
          });

          handles.push(handle);
      }

      for handle in handles {
          handle.join().unwrap();
      }

      println!("The value of counter: {}", *counter.lock().unwrap());
  }
  ```

- The combination of `Arc<Mutex<T>>`, is analogous to `Rc<RefCell<T>>`.
- Keep in mind using `Mutex<T>` is risky, as logical errors may lead to _deadlocks_.

#### `Send` and `Sync` trait

- These two traits are part of the language itself, unlike otheer features of concurrency as they were part of the standard library.
- They are called `std::marker` traits.
- `Send` vs `Sync`

  | `Send`                                                               | `Sync`                                                                                   |
  | -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
  | It is safe to transfer ownership of a type between threads.          | It is safe to use that type's reference betweeen threads.                                |
  | Any type `T` that implements `Sync`                                  | Type `T` is `Sync`, if it's reference (`&T`) is `Send` or if type `T` implements `Sync`. |
  | Except `Rc<T>`, almost all types are `Send`. (use `Arc<T>` instead). | Primitive Types, `Mutex<T>` are `Sync` but `Rc<T>`, `RefCell<T>` are not.                |

- We don't need to implement `Send` and `Sync` for the types that are made up of those types that implements these traits.
- In case you need to implement thes traits for a particular type than it means you'll need to write some `unsafe` rust code. You can learn the Dark Arts of Unsafe Code from this book [“The Rustonomicon”](https://doc.rust-lang.org/nomicon/index.html).

### OOP (Object Oriented Programming)

- Characterstics of OOP:
  - _Objects contain data and behaviour_:
    - Programs should make up of objects.
    - An object packages both data and it's methods.
    - Rust offers `struct`, `enum` and `impl` blocks to provide this characterstic.
  - _Encapsulation_:
    - When the implementation details are hidden from the code that is using the object.
    - The only way to use an object is through it's public API.
    - This enables the programmer to change and refactor an object’s internals without needing to change the code that uses the object.
    - Rust encapsulates everything by default and offers `pub` keyword to make things public.
  - _Inheritance_:
    - When an object can inherit from another object’s definition, so that it can use it's parent object's data and behavior without defining them again.
    - Rust doesn't offers inheritance between `struct`, but has `trait` where you can use default methods that is both reusable and can be overridden.
    - The reasons to use inheritance are: _code reusability_ and _polymorphism_.
    - Polymorphism means that you don't need to explicitly define the type in code, but can be detected during runtime. It is useful when two types share same characterstics.
    - Rust offers polymorphism in a more general manner. It offers `generics` to generalize the accepted types and `trait bounds`, to constraint the allowed types.

Note: People think "polymorphism is synonymous with inheritance". But it is a more general concept which means that a certain code can be referred to multiple types. It is used when two types share some common characterstics. In inheritance, those types are only subclasses.

- Problems with Inheritance:

  - It adds the risk of sharing more code than necessary.
  - Subclasses are forced to share all the characterstics of the parents, even though sometimes it's not necessary or even undesired.
  - Sometimes calling the functions on the subclass doen't makes sense and even cause errors.
  - Due to this, some programming languages will only allow a subclass to inherit from one class, further restricting the flexibility of a program’s design.

- Rust is different, it takes a completely different approach by using trait objects instead of inheritance.

#### Defining a common behaviour using trait

- A `trait` object points to both:

  - An instance of a type implementing our specified trait
  - A table used to look up trait methods on that type at runtime.

| Property           | `struct` or `enum`          | `trait`                                      | Objects in other languages                 |
| ------------------ | --------------------------- | -------------------------------------------- | ------------------------------------------ |
| Stores Data        | Yes                         | No                                           | Yes                                        |
| Stores Behaviour   | No                          | Yes                                          | Yes                                        |
| Data and behaviour | Seperated by `impl` blocks. | Combined                                     | Combined                                   |
| Uses               | Store same items together   | Store common behaviour and allow abstraction | Store same items and their common behavior |

- An example:

  - _Problem:_ Let's say initially we have components such as `Button` and `Image` that may use a common functionality _to draw_ on the screen. It's possible that someday programmers want to introduce one more component named `SelectBox`. So, what we'll end up with are different types of structures that wants to use a common functionality.
  - _Solution:_ We can invent a common function named `draw()`, which will have different implementations for different types of components.
  - _How to build:_ We'll initialise a `trait` that can be shared among various components and a `struct` that can hold these compoenents.

  - The `trait` will look like this:

    ```rust
    // A common functionality shared between multiple components
    pub trait Draw {
        fn draw(&self);
    }
    ```

  - We can build a `struct` that holds the components that implements the `Draw`:

    ```rust
    pub struct Screen {
        // Box will allow to store the components on heap
        // dyn keyword will add the ability to detect a type that implements Draw on runtime
        pub components: Vec<Box<dyn Draw>>,
    }

    impl Screen {
        pub fn run(&self) {
            for component in self.components.iter() {
                component.draw();
            }
        }
    }
    ```

  - The difference with the alternative implementation using trait bounds in `struct` is that it restricts us to a `Screen` instance that has a list of components all of type `Button` or all of type `TextField`. At compile time, the definitions will be monomorphized.

    ```rust
    pub struct Screen<T: Draw> {
        pub components: Vec<T>,
    }

    impl<T> Screen<T>
    where
        T: Draw,
    {
        pub fn run(&self) {
            for component in self.components.iter() {
                component.draw();
            }
        }
    }
    ```

  - Programmers can now create new components like this:

    ```rust
    use gui::Draw;

    pub struct Button {
        // Some fields
    }

    impl Draw for Button {
        fn draw(&self) {
            // code to actually draw something
        }
    }
    ```

  - Users of this library can now use it like this:

    ```rust
    use gui::{Button, Screen};

    fn main() {
        let screen = Screen {
            components: vec![
                Box::new(SelectBox {
                    width: 75,
                    height: 10,
                    options: vec![
                        String::from("Yes"),
                        String::from("Maybe"),
                        String::from("No"),
                    ],
                }),
                Box::new(Button {
                    width: 50,
                    height: 10,
                    label: String::from("OK"),
                }),
            ],
        };

        screen.run();
    }
    ```

  - This concept is similar to the concept like _duck typing_: if it walks like a duck and quacks like a duck, then it must be a duck!

  - Use Cases:
    - Generics with trait bounds: If you’ll only ever have homogeneous collections. For Example, all elements of vector will be of type `Button`.
    - `Box<dyn T>`: You can use heterogeneous collections. For Example, elements can be a mix of `Button`, `TextField` etc.

- Static Dispatch vs Dynamic Dispatch:

| Static Dispatch                                                | Dynamic Dispatch                                                               |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Concrete types are decided at compile time.                    | The compiler emits code that at runtime will figure out which method to call.  |
| Compiler writes some new code for various concrete types.      | At runtime, it is decided whether a selected type can follow the requirements. |
| When we use trait bounds on generics, static dispatch happens. | When we want to perform dynamic dispatch, we can use the `dyn` keyword.        |
| No runtime cost is added.                                      | Some runtime cost is added.                                                    |

#### The State Pattern

- The _state pattern_ is an object-oriented design pattern.
- The current state is stored inside the struct, along with it's value(s).

  ```rust
  pub struct Post {
      // Box and dyn are used because the state variable
      // will have different states during the life of Post
      state: Option<Box<dyn State>>,
      content: String,
  }
  ```

- There are _state objects_, you can create a new object by implementing this trait.

  ```rust
  trait State {
      // The first two functions, results in transitions to different states.
      fn request_review(self: Box<Self>) -> Box<dyn State>;
      fn approve(self: Box<Self>) -> Box<dyn State>;
      // This function, can be called on any state object,
      // similar to the above two functions, except instead
      // of causing a state transition, it will return value
      // as if we conditionally returned output for each state
      fn content<'a>(&self, _post: &'a Post) -> &'a str {
          ""
      }
  }
  ```

  - The state pattern is built such that, methods defined on _state objects_ will cause changes in the `Post`, but the methods defined on `Post` will have no idea what these changes will look like. Hence, _state objects_ will encapsulate behaviour changes from the main _struct_.

  - You can look at it's complete implementation over [here](https://gist.github.com/utkarshg6/642859eef3d79fde55eeafb6cb4ed520).
  - Some downsides of State Pattern:

    - Extra Modifications: If we add a new state, we'll need to modify other states too. It's due to the reason that one state can only make transitions to another state.
    - Code Duplication: It leads us to write common code inside state objects, as we cannot write directly in trait's default implementation because traits don't know about the concrete type.

  - There's another implementation of state pattern in Rust. It doesn't follow the classic OOP pattern, as we'll require to store the object in new variable, whenever a state transition will happen. Here's the [code](https://gist.github.com/utkarshg6/507560be53345b20aca8304f477fa0b0).

    ```rust
    // This will cause a state transition from PendingReview to Published
    let post = post.approve();
    ```

### Pattern Matching

- `match` Arms

  ```rust
  match VALUE {
      PATTERN => EXPRESSION,
      PATTERN => EXPRESSION,
      PATTERN => EXPRESSION,
  }
  ```

- Conditional `if let` statements

  ```rust
  fn main() {
      let favorite_color: Option<&str> = None;
      let is_tuesday = false;
      let age: Result<u8, _> = "34".parse();

      if let Some(color) = favorite_color {
          println!("Using your favorite color, {}, as the background", color);
      } else if is_tuesday {
          println!("Tuesday is green day!");
      } else if let Ok(age) = age {
          if age > 30 {
              println!("Using purple as the background color");
          } else {
              println!("Using orange as the background color");
          }
      } else {
          println!("Using blue as the background color");
      }
  }
  ```

- `while let` loops

  ```rust
      let mut stack = Vec::new();

      stack.push(1);
      stack.push(2);
      stack.push(3);

      while let Some(top) = stack.pop() {
          println!("{}", top);
      }
  ```

- `for` loops

  ```rust
      let v = vec!['a', 'b', 'c'];

      for (index, value) in v.iter().enumerate() {
          println!("{} is at index {}", value, index);
      }
  ```

- `let` statements

  ```rust
      let (x, y, z) = (1, 2, 3);
  ```

- function parameters

  ```rust
  fn print_coordinates(&(x, y): &(i32, i32)) {
      println!("Current location: ({}, {})", x, y);
  }

  fn main() {
      let point = (3, 5);
      print_coordinates(&point);
  }
  ```

#### Forms of Pattern

| Refutable                                                                                                                                | Irrefutable                                                                                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Patterns that can fail to match for some possible value.                                                                                 | Patterns that'll match for any possible value.                                                                                                                          |
| For Example, in `if let Some(x) = a_value`, the `Some(x)` can fail to match if `a_value` is `None`.                                      | For Example, in `let x = 5;`, the `x` matches anything and therefore cannot fail to match.                                                                              |
| The `if let` and `while let` expressions accept refutable and irrefutable patterns, but the compiler warns against irrefutable patterns. | Function parameters, `let` statements, and `for` loops can only accept irrefutable patterns, because the program cannot do anything meaningful when values don’t match. |

- Using a refutable pattern where Rust requires an irrefutable pattern:

  ```rust
  // FAIL: The code doesn't know what to do with a None value
  // Some(x) is a refutable pattern
  // let requires an irrefutable pattern
  let Some(x) = some_option_value;
  ```

- Using irrefutable pattern where Rust requires a refutable pattern:

```rust
// WARN: It doesn’t make sense to use if let with an irrefutable pattern
// x is an irrefutable pattern
// if let is a refutable pattern
if let x = 5 {
    println!("{}", x);
};
```

- In `match` statements, all arms use refutable pattern except the last one that uses `_`, which uses irrefutable pattern.

#### Pattern Syntax

##### Inside the `match` expression

- Each `match` expression creates a new scope, hence varaibles defined in scope will shadow the variables defined outside.

  ```rust
      let x = Some(5);
      let y = 10;

      match x {
          Some(50) => println!("Got 50"),
          Some(y) => println!("Matched, y = {:?}", y), // This y will shadow the y defined outside of this scope
          _ => println!("Default case, x = {:?}", x),
      }

      println!("at the end: x = {:?}, y = {:?}", x, y);

      // Output =>
      // Matched, y = 5
      // at the end: x = Some(5), y = 10
  ```

- It's possible to use the "or" using the `|` syntax

  ```rust
      let x = 1;

      match x {
          1 | 2 => println!("one or two"),
          3 => println!("three"),
          _ => println!("anything"),
      }

      // Output =>
      // one or two
  ```

- You may also specify ranges in the arm, only numbers and `char` values are allowed.

  ```rust
      let x = 5;

      match x {
          1..=5 => println!("one through five"),
          _ => println!("something else"),
      }

      let y = 'c';

      match y {
          'a'..='j' => println!("early ASCII letter"),
          'k'..='z' => println!("late ASCII letter"),
          _ => println!("something else"),
      }

      // Output =>
      // one through five
      // early ASCII letter
  ```

##### Destructuring

- Destructuring structs

  ```rust
  struct Point {
      x: i32,
      y: i32,
  }

  fn main() {
      let p = Point { x: 0, y: 7 };

      // It's possible to destructure a struct into variables
      let Point { x: a, y: b } = p;
      assert_eq!(0, a);
      assert_eq!(7, b);

      // A shorthand, will directly store respective values in x and y
      let Point { x, y } = p;
      assert_eq!(0, x);
      assert_eq!(7, y);
  }
  ```

- Destructring as well as matching the structs

  ```rust
  fn main() {
      let p = Point { x: 0, y: 7 };

      match p {
          Point { x, y: 0 } => println!("On the x axis at {}", x),
          Point { x: 0, y } => println!("On the y axis at {}", y),
          Point { x, y } => println!("On neither axis: ({}, {})", x, y),
      }
  }
  ```

- Destructuring enums

  ```rust
  enum Color {
      Rgb(i32, i32, i32),
      Hsv(i32, i32, i32),
  }

  enum Message {
      Quit,
      Move { x: i32, y: i32 },
      Write(String),
      ChangeColor(Color),
  }

  fn main() {
      let msg = Message::ChangeColor(Color::Hsv(0, 160, 255));

      match msg {
          Message::Quit => {
              println!("The Quit variant has no data to destructure.")
          }
          Message::Move { x, y } => {
              println!(
                  "Move in the x direction {} and in the y direction {}",
                  x, y
              );
          }
          Message::Write(text) => println!("Text message: {}", text),
          Message::ChangeColor(r, g, b) => println!(
              "Change the color to red {}, green {}, and blue {}",
              r, g, b
          ),
          Message::ChangeColor(Color::Rgb(r, g, b)) => println!(
              "Change the color to red {}, green {}, and blue {}",
              r, g, b
          ),
          Message::ChangeColor(Color::Hsv(h, s, v)) => println!(
              "Change the color to hue {}, saturation {}, and value {}",
              h, s, v
          ),
      }
  }
  ```

- We can mix, match, and nest destructuring patterns in even more complex ways

  ```rust
      let ((feet, inches), Point { x, y }) = ((3, 10), Point { x: 3, y: -10 });
  ```

##### Ignoring Values

- You may use `_` or `..` to ignore values:

  - `_` : It is used when you want to ignore a warning of unused variable, inside match expression for the remaining values or using a name that starts with underscore.
  - `..` : Ignore remaining parts of the value.

- You may use `_` for ignoring an unused variable

  ```rust
  fn main() {
      let _x = 5;
      let y = 10;
  }
  ```

- It's possible to use `_` in functions

  ```rust
  fn foo(_: i32, y: i32) {
      println!("This code only uses the y parameter: {}", y);
  }

  fn main() {
      foo(3, 4);
  }
  ```

- There is a subtle difference between using only `_` and using a name that starts with an underscore.

  - The syntax `_x` still binds the value to the variable.

    ```rust
    // FAIL: s lost it's ownership to _s, but was attempted to use again for printing
    let s = Some(String::from("Hello!"));

    // _s binds the value, the value of s is moved
    if let Some(_s) = s {
        println!("found a string");
    }

    println!("{:?}", s);
    ```

  - Whereas `_` doesn’t bind at all.

    ```rust
        let s = Some(String::from("Hello!"));

        // _ never binds the value, hence s stays the owner
        if let Some(_) = s {
            println!("found a string");
        }

        println!("{:?}", s);
    ```

Note: Ignoring a function parameter can be especially useful in some cases, for example, when implementing a trait when you need a certain type signature but the function body in your implementation doesn’t need one of the parameters. The compiler will then not warn about unused function parameters, as it would if you used a name instead.

- Ignoring remaining parts of `struct` with `..`

  ```rust
  struct Point {
      x: i32,
      y: i32,
      z: i32,
  }

  let origin = Point { x: 0, y: 0, z: 0 };

  match origin {
      // It prevents using _ multiple times
      Point { x, .. } => println!("x is {}", x),
  }
  ```

- Skipping middle values using `..`

  ```rust
  fn main() {
      let numbers = (2, 4, 8, 16, 32);

      match numbers {
          (first, .., last) => {
              println!("Some numbers: {}, {}", first, last);
          }
      }
  }
  ```

- You can only use `..` once per tuple

  ```rust
  fn main() {
      let numbers = (2, 4, 8, 16, 32);

      match numbers {
          (.., second, ..) => {
              println!("Some numbers: {}", second)
          },
      }
  }
  ```

##### Match Guard

- _Match Guard_ is an additional `if` condition specified after the pattern in a match arm that must also match along with the pattern matching, _for that arm to be chosen_.

  ```rust
      let num = Some(4);

      match num {
          Some(x) if x % 2 == 0 => println!("The number {} is even", x),
          Some(x) => println!("The number {} is odd", x),
          None => (),
      }
  ```

- The downside of this additional expressiveness is that the compiler doesn't try to check for exhaustiveness when match guard expressions are involved.
- Using Match Guard with `|` operator

  ```rust
      let x = 4;
      let y = false;

      match x {
          // It works like this => (4 | 5 | 6) if y => ...
          // And not like this => 4 | 5 | (6 if y) => ...
          4 | 5 | 6 if y => println!("yes"),
          _ => println!("no"),
      }

      // Output =>
      // no
  ```

- `@` Bindings

  ```rust
  // The at operator (@) lets us create a variable that holds a value at the same time
  // we’re testing that value to see whether it matches a pattern.

  enum Message {
      Hello { id: i32 },
  }

  let msg = Message::Hello { id: 5 };

  match msg {
      Message::Hello {
          id: id_variable @ 3..=7,
      } => println!("Found an id in range: {}", id_variable), // Value of "id" is stored in "id_variable", hence it was knwon here
      Message::Hello { id: 10..=12 } => {
          println!("Found an id in another range") // Range was specified, Value of "id" is not stored inside any variable, hence it is unknown here
      }
      Message::Hello { id } => println!("Found some other id: {}", id), // Range was not specified, Value of "id" is stored inside "id", hence it was known here
  }
  ```

## Advanced Features

### Unsafe Rust

_Rust has a second language hidden inside it that doesn’t enforce the memory safety guarantees: it’s called **unsafe Rust** and works just like regular Rust, but gives us extra superpowers._

- Why it exists?

  - It’s better for Rust to reject some valid programs rather than accept some invalid programs.
  - That makes the static analysis of the Rust compiler conservative.
  - Although the code might be okay, if the Rust compiler doesn’t have enough information to be confident, it will reject the code.
  - In these cases, you can use unsafe code to tell the compiler, “Trust me, I know what I’m doing.”
  - Another reason Rust has an unsafe alter ego is that the underlying computer hardware is inherently unsafe. Hence, it'll allow you to write low-level systems code, such as directly interacting with the OS, or even write your own OS.

- Any Downsides?

  - Use it at your own risk.
  - Problems due to memory unsafety, such as null pointer dereferencing, can occur.

- Answers to some common misconceptions:

  - It’s important to understand that `unsafe` doesn’t turn off the borrow checker or disable any other of Rust’s safety checks: if you use a reference in unsafe code, it will still be checked.
  - Hence, you'll get only the above mentioned features along with some safety.
  - Also, it does not necessarily mean that code inside `unsafe` is necessarily dangerous or that it will definitely have memory safety problems.
  - It is programmer's responsibilty to ensure that the code is memory safe.

- How to write code safely using `unsafe`?

  - Keep unsafe blocks small and it'll be easier to investigate the memory bugs.
  - You can also wrap unsafe code in a safe abstraction. It prevents the uses of unsafe from leaking out in all the other places.

- What Superpowers can I get?
  - Dereference a raw pointer
  - Call an unsafe function or method
  - Access or modify a mutable static variable
  - Implement an unsafe trait
  - Access fields of `union`s

#### Dereferencing a Raw Pointer

- Raw Pointers are meant for unsafe rust and are similar to references. They are of two types:
  - `*const T`: Immutable Raw Pointer
  - `*mut T`: Mutable Raw Pointer
- Here `*` is not a dereference operator but a part of the type name.
- Unlike references and Smart Pointers, they break the following rules of Rust's safety:
  - They are allowed to ignore the borrowing rules by having both immutable and mutable pointers or multiple mutable pointers to the same location
  - Aren’t guaranteed to point to valid memory
  - Are allowed to be null
  - Don’t implement any automatic cleanup
- This is how you can create raw pointers out of a variable.

  ```rust
  let mut num = 5;

  let r1 = &num as *const i32;
  let r2 = &mut num as *mut i32;
  ```

- We can create raw pointers in safe code; we just can’t dereference raw pointers outside an unsafe block.

  ```rust
  let mut num = 5;

  // Notice it's possible to create raw pointers inside safe code
  let r1 = &num as *const i32;
  let r2 = &mut num as *mut i32;

  // But to dereference a raw pointer you'll require an unsafe block
  unsafe {
      println!("r1 is: {}", *r1);
      println!("r2 is: {}", *r2);
  }
  ```

- We broke the Rust's safety measures, as we are able to use a mutable and immutable reference to a value. Now, as a programmer we made sure that these references are used properly inside the `unsafe` block.

- Uses of creating raw pointers:
  - Mostly used when interfacing with C code.
  - Calling an Unsafe Function or Method.
  - Building safe abstractions over unsafe code.

#### Call an unsafe function or method

- Defining an unsafe function:

  ```rust
  unsafe fn dangerous() {}
  ```

- Calling an unsafe function:

  ```rust
  // By calling an unsafe function within an unsafe block,
  // we’re saying that we’ve read this function’s documentation
  // and take responsibility for upholding the function’s contracts.
  unsafe {
      dangerous();
  }
  ```

##### Wrappping unsafe code in safe abstractions

- We want to create a function that can split a vector into two by index

  ```rust
  // FAIL: Rust won't allow us to have two immutable borrow of the same vector
  // Only we know that the two immutable borrow aren't overlapping and won't
  // cause any trouble so we would like to silent the compiler by using unsafe
  fn split_at_mut(slice: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
      let len = slice.len();

      assert!(mid <= len);

      (&mut slice[..mid], &mut slice[mid..])
  }
  ```

- Here is it's implementation using unsafe

  ```rust
  use std::slice;

  // Notice the function isn't using unsafe in it's signature, hence unsafe is
  // wrapped in a safe abstraction
  fn split_at_mut(slice: &mut [i32], mid: usize) -> (&mut [i32], &mut [i32]) {
      let len = slice.len();
      let ptr = slice.as_mut_ptr(); // Raw pointer

      assert!(mid <= len);

      // This is an acceptable use of unsafe
      // We need unsafe block to call these functions because
      // we're slicing and adding to the raw pointers, which may
      // have a chance to become invalid, iff programmer hasn't
      // written in properly
      unsafe {
          (
              slice::from_raw_parts_mut(ptr, mid), // It will give the slice of range [ptr, ptr + mid)
              slice::from_raw_parts_mut(ptr.add(mid), len - mid),
          )
      }
  }
  ```

- If you want to create a raw pointer with unexpected behaviour, you can do this

  ```rust
  // FAIL: It won't point to a valid i32 value
  use std::slice;

  let address = 0x01234usize;
  let r = address as *mut i32;

  let slice: &[i32] = unsafe { slice::from_raw_parts_mut(r, 10000) };
  ```

##### Call the code from other languages using `extern`

- Rust uses `extern` keyword to use _Foreign Function Interface (FFI)_, it is a way for a programming language to define functions and enable a different (foreign) programming language to call those functions.
- Functions declared within extern blocks **are always unsafe to call** from Rust code.
- This is how you can call `C` code in Rust:

  ```rust
  // After extern you need to specify ABI (Application Binary Interface)
  // Here we are using extern to use ABI of other languages
  extern "C" {
      fn abs(input: i32) -> i32;
  }

  fn main() {
      unsafe {
          println!("Absolute value of -3 according to C: {}", abs(-3));
      }
  }
  ```

- It is possible to write Rust code such that other languages can call.

  ```rust
  // This code is not unsafe
  #[no_mangle] // This doesn't allows the compiler to rename the functions name
  pub extern "C" fn call_from_c() {   // Here we are using extern to create an ABI for other languages
      println!("Just called a Rust function from C!");
  }
  ```

#### Accessing or Modifying a Mutable Static Variable

- In Rust, **global** variables are called _static variables_.
- It is problematic as it may cause a data race if two threads are accessing the same mutable global variable.
- This is how you can create a global or static variable.

  ```rust
  static HELLO_WORLD: &str = "Hello, world!";

  fn main() {
      println!("name is: {}", HELLO_WORLD);
  }
  ```

- The references for static variable is `'static` by default. So, we need to specify it's lifetime anywhere.
- Also, it's completely safe to access an immutable static variable.

##### Constants and Static Variable

| Constants                                     | Static Variable                                                                                                            |
| --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Dynamic address in memory                     | Fixed address in memory                                                                                                    |
| Constants duplicate their data whenever used. | Using the value will always access the same data.                                                                          |
| Constants are never mutable.                  | Static variable can be both mutable and immutable, and for modifying mutable static variable, you'll need to use `unsafe`. |

##### Implementing Static Variables

- This is how you can implement static variables.

  ```rust
  static mut COUNTER: u32 = 0;

  fn add_to_count(inc: u32) {
      unsafe {
          COUNTER += inc;
      }
  }

  fn main() {
      add_to_count(3);

      unsafe {
          println!("COUNTER: {}", COUNTER);
      }
  }
  ```

- Notice that, it's not causing us any trouble because this code is single threaded, but if we tried to mutate the static variable in multiple threads it could lead to data races.
- Static Variables (_or Global Variables_) are unsafe. That's because it's difficult to ensure that there are no data races for a global variable.
- It’s preferable to use the concurrency techniques and thread-safe smart pointers.

#### Implementing an unsafe trait

- A trait is unsafe when at least _one of its methods_ has some invariant that the compiler can’t verify.

- Here's an implementation:

  ```rust
  unsafe trait Foo {
      // methods go here
  }

  unsafe impl Foo for i32 {
      // method implementations go here
  }

  fn main() {}
  ```

- If we implement a type that contains a type that is not `Send` or `Sync` (i.e. doesn't already implements the safe ways of sending a type in multiple threads), such as _raw pointers_, and we want to mark that type as `Send` or `Sync`, we must use unsafe.

#### Accessing Fields of a Union

- A `union` is similar to a `struct`, but only one declared field is used in a particular instance at one time.
- Unions are primarily used to interface with unions in C code.
- Accessing union fields is unsafe because Rust can’t guarantee the type of the data currently being stored in the union instance.

### Advanced Traits

#### Associated Types

- This is how you can create an associate type inside a trait

  ```rust
  pub trait Iterator {
      type Item; // You see, this is the associated type

      fn next(&mut self) -> Option<Self::Item>; // Now, this trait can use this type in it's signatures
  }
  ```

- Here's an implementation of this trait on one of our types Counter:

  ```rust
  impl Iterator for Counter {
      type Item = u32;

      fn next(&mut self) -> Option<Self::Item> {
          // --snip--
  ```

- Now, you might be wondering, can't we do something like this with using traits with generics?

  ```rust
  pub trait Iterator<T> {
      fn next(&mut self) -> Option<T>;
  }
  ```

- Here are the differences between Associated Types and Traits with generics?

  | Traits with Associated Types                                                      | Traits with Generics                                                              |
  | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
  | There will be only one implementation for a type.                                 | There can be multiple implementations for a type, using individual concrete type. |
  | For Example, `impl Iterator for Counter`                                          | For Example, `impl Iterator<String> for Counter`, and many more.                  |
  | Using functions of these traits will not require you to provide type annotations. | You'll need to provide type annotations to provide which iteration to use.        |

#### Operator Overloading

- This is how you can add two types by using the `+` operator

  ```rust
  // This library `std::ops` contains all the overloadable operators
  use std::ops::Add;

  #[derive(Debug, Copy, Clone, PartialEq)]
  struct Point {
      x: i32,
      y: i32,
  }

  impl Add for Point {
      type Output = Point; // associated type will restrict the number of implementations

      fn add(self, other: Point) -> Point { // This fn will decide how the `+` will behave for the type Point
          Point {
              x: self.x + other.x,
              y: self.y + other.y,
          }
      }
  }

  fn main() {
      assert_eq!(
          Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
          Point { x: 3, y: 3 }
      );
  }
  ```

- This is how the trait `Add` is defined in the Rust's library `std::ops`

  ```rust
  // This syntax `<Rhs=Self>` is called "default parameters"
  // In case, someone implementing this trait doesn't define the type
  // then the type defined in default parameter will be used everywhere in this trait
  trait Add<Rhs=Self> {
      type Output;

      fn add(self, rhs: Rhs) -> Self::Output; // The "default parameter" is used here inside argument `rhs`
  }
  ```

- You can also customize `Rhs`, that'll mean you'll be adding a value of different type to the main type

  ```rust
  use std::ops::Add;

  struct Millimeters(u32);
  struct Meters(u32);

  impl Add<Meters> for Millimeters { // Modified Rhs to Meters, otherwise the default will be Self (or Millimeters)
      type Output = Millimeters;

      fn add(self, other: Meters) -> Millimeters {
          Millimeters(self.0 + (other.0 * 1000))
      }
  }
  ```

- You’ll use default type parameters in two main ways:
  - To extend a type without breaking existing code
  - To allow customization in specific cases most users won’t need

#### Function with same name in multiple traits

- Following things are allowed:

  - Multiple traits to have functions with same name
  - A type implementing all these traits

- Here's an implementation:

  ```rust
  trait Pilot {
      fn fly(&self);
  }

  trait Wizard {
      fn fly(&self);
  }

  struct Human;

  impl Pilot for Human {
      fn fly(&self) {
          println!("This is your captain speaking.");
      }
  }

  impl Wizard for Human {
      fn fly(&self) {
          println!("Up!");
      }
  }

  impl Human {
      fn fly(&self) {
          println!("*waving arms furiously*");
      }
  }

  fn main() {
      let person = Human;
      Pilot::fly(&person); // You'll have to pass the reference to the person, because it takes self as an argument
      Wizard::fly(&person); // if we had two types that both implement one trait, this self would recognize the correct type
      person.fly(); // The direct implementation will get called first, instead you can also use Human::fly(&person)
  }
  ```

- Let's see what happens if the _associated function_ of a _struct_ and a _trait_ has same name

  ```rust
  trait Animal {
      // Notice, there is no self passed inside,
      // multiple types can implement this trait,
      // Calling this function won't be direct,
      // as the trait won't be able to infer
      // which type's implementation to call
      fn baby_name() -> String;
  }

  struct Dog;

  impl Dog {
      // This is an associated function, which can be called
      // using `Dog::baby_name()`, just like any other
      // associated fn without self
      fn baby_name() -> String {
          String::from("Spot")
      }
  }

  impl Animal for Dog {
      fn baby_name() -> String {
          String::from("puppy")
      }
  }

  fn main() {
      println!("A baby dog is called a {}", Dog::baby_name()); // The direct implementation will get called
      println!("A baby dog is called a {}", Animal::baby_name()); // Won't work, this associated fn don't has self, remember? It can't infer the type
      println!("A baby dog is called a {}", <Dog as Animal>::baby_name()); // We need to tell Rust that we want to use the implementation of Animal for Dog
  }
  ```

- This is the fully qualified syntax, used for all associated functions (including methods)

  ```rust
  <Type as Trait>::function(receiver_if_method, next_arg, ...); // You don't need to provide all the information that Rust can infer
  ```

#### Supertraits

- Sometimes, you might need one trait to use another trait’s functionality.
- The traits you would be using to build your own trait is called _Supertrait_.
- One important thing is that, now your trait can only be implemented on the types that already implements the Supertrait.

- Here's an implementation. This `OutlinePrint` can **only** be implemented on any trait that implements `Display`.

  ```rust
  use std::fmt;

  trait OutlinePrint: fmt::Display { // It is also like bounding a trait with another trait
      fn outline_print(&self) {
          let output = self.to_string(); // It works only because all the types already implements `Display`
          let len = output.len();
          println!("{}", "*".repeat(len + 4));
          println!("*{}*", " ".repeat(len + 2));
          println!("* {} *", output);
          println!("*{}*", " ".repeat(len + 2));
          println!("{}", "*".repeat(len + 4));
      }
  }
  ```

- For reference, you can also implement `fmt::Display` on your type, like this:

  ```rust
  struct Point {
      x: i32,
      y: i32,
  }

  use std::fmt;

  impl fmt::Display for Point {
      fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
          write!(f, "({}, {})", self.x, self.y)
      }
  }
  ```

- Now, we know that `Point` implements `Display`, now we can implement `OutlinePrint` like this:

  ```rust
  impl OutlinePrint for Point {}
  ```

- The output will look like this for the fn `outline_print`

  ```zsh
  **********
  *        *
  * (1, 3) *
  *        *
  **********
  ```

#### Newtype Pattern

_The **Orphan Rule** states that we’re allowed to implement a trait on a type as long as either the trait or the type are local to our crate._

- The _newtype pattern_ , helps us to get around this restriction.
- All we need to do is to create a new type in a tuple struct.
- Let's say, we want to implement `Display` on `Vec<T>`. Both of them are not local to our code, thereby orphan rule will prevent us from implementing it.
- Now, we can use this newtype pattern for getting a workaround. What we'll be building is a wrapper that will make the `Vec<T>` local to us.

  ```rust
  use std::fmt;

  struct Wrapper(Vec<String>); // A new struct, which is just a wrapper for Vec<String>

  impl fmt::Display for Wrapper { // Now, we can implement Display on Wrapper, but we can't do directly on Vec<T>
      fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
          write!(f, "[{}]", self.0.join(", ")) // self.0 is used to take the Vector out
      }
  }

  fn main() {
      // The only disadvantage is that, we'll need to create
      // a vector inside a wrapper to use the Display trait
      let w = Wrapper(vec![String::from("hello"), String::from("world")]);
      println!("w = {}", w);
  }
  ```

- If we wanted the new type to have every method the inner type has, [implementing the Deref trait](https://doc.rust-lang.org/book/ch15-02-deref.html#treating-smart-pointers-like-regular-references-with-the-deref-trait) on the Wrapper to return the inner type would be a solution.
- If we don’t want the Wrapper type to have all the methods of the inner type — for example, to restrict the Wrapper type’s behavior — we would have to implement just the methods we do want manually.

### Advanced Types

#### Uses of the Newtype Pattern

- _Type Safety_: We can wrap `u32` values with structs `Millimeters` and `Meters`. Now, if a value is stored in `Millimeters`, it is safe to say that this value can't call functions defined for `Meters`, and vice versa is true.
- _Abstraction_: We could provide a `People` type to wrap a `HashMap<i32, String>` that stores a person’s ID associated with their name. Code using `People` would only interact with the public API we provide, such as a method to add a name string to the People collection; that code **wouldn’t need to know** that we assign an `i32` ID to names internally.

#### Type Aliases

- This is how you can create a type alias:

  ```rust
  type Kilometers = i32;
  ```

- How it works?

  - Values with type `Kilometers` will be treated same as `i32`.
  - We aren't creating a new type, we're just adding a _synonym_ to `i32`, called `Kilometers`

- Hence, doing something like this is totally fine:

  ```rust
  let x: i32 = 5;
  let y: Kilometers = 5;

  println!("x + y = {}", x + y); // This will work
  ```

- The advantage is that it will give us the flexibility that any function with an argument of `i32`, we can pass `Kilometers` value to it.
- The disadvantage is that we don't get the type checks as we get in the newtype pattern.

- You can create an alias where you want to prevent naming a complex type.

  ```rust
  type Thunk = Box<dyn Fn() + Send + 'static>; // A type that stores closure

  let f: Thunk = Box::new(|| println!("hi")); // We don't need to specify the longer type, instead we can say just "Thunk"

  fn takes_long_type(f: Thunk) { // Again, we don't need to specify the longer type, just "Thunk"
    ...
  }
  ```

Fun Fact: Thunk is a word for code to be evaluated at a later time, so it’s an appropriate name for a closure that gets stored

- We can also shorten a `Result` values of I/O operations like this

  ```rust
  type Result<T> = std::result::Result<T, std::io::Error>;
  ```

- Now, `Result<usize, Error>` can be replaced with `Result<usize>` and `Result<(), Error>` can be replaced with `Result<()>`. Also, we can use the `?` operator, since it's the same type.

#### The never type `!`

- This type never returns. In type theory lingo it is known as the _empty type_ because it has no values.
- Rust prefers to call it the never type because it stands in the place of the return type when a function will never return.

  ```rust
  // People read it as, "the function bar(), returns never", and are called diverging functions
  fn bar() -> ! {
      // --snip--
  }
  ```

- Rust never allows a variable to have different possible data types.

  ```rust
  // FAIL: You can't create a variable "guess", that may have either number or string
  let guess = match guess.trim().parse() {
      Ok(_) => 5,
      Err(_) => "hello",
  };
  ```

- But this is possible:

  ```rust
  let guess: u32 = match guess.trim().parse() {
      Ok(num) => num,
      Err(_) => continue, // Wait a minute, how's this even allowed?
  };
  ```

- The `continue` has a never type (`!`), which means it'll never return any value. That is, when Rust computes the type of `guess`, it looks at both match arms, the former with a value of `u32` and the latter with a `!` value. Because `!` can never have a value, Rust decides that the type of guess is `u32`.

- You can remember it this way, the `!` can get coerced to any other type.

- This is the original implementation of `unwrap!()`. Here, the return type is coerced to a single type `T`, and that's because `panic!()` ends the program and has a never type (`!`).

  ```rust
  impl<T> Option<T> {
      pub fn unwrap(self) -> T {
          match self {
              Some(val) => val,
              None => panic!("called `Option::unwrap()` on a `None` value"),
          }
      }
  }
  ```

- The value of the expression of `loop` is also of the type `!`.

  ```rust
  print!("forever ");

  loop {
      print!("and ever ");
  }
  ```

#### Dynamically Sized Traits and the `Sized` trait

- _DSTs_ or _unsized_ types let us write code using values whose size can only be known at runtime.

- So, Rust doesn't let us create strings with `str` (not `&str`):

  ```rust
  let s1: str = "Hello there!";
  let s2: str = "How's it going?";
  ```

- Why's that?

  - Rust needs to know a fixed size of a type. Here `s1` takes 12 bytes and `s2` takes `15` bytes.
  - It's not possible to accomodate all the strings in a single fixed size.

- What's the solution?

  - `&str` is the solution.
  - It stores two values: the address of the `str` and its length.
  - So, that makes `&str` will only need two `usize`, one for the address and the other for the length.
  - That's why, we always know the size of a `&str`, no matter how long the string it refers to is.

- In general, this is the way in which dynamically sized types are used in Rust.
- The golden rule of dynamically sized types is that we must always put values of dynamically sized types behind a pointer of some kind.

- The traits can be Dynamically Sixed too. All we need to do is to put them behind a pointer, such as `&dyn Trait` or `Box<dyn Trait>`.

- The `Sized` trait

  - A trait that determines whether or not a type's size is known at compile time.
  - You may create a generic function like this:

    ```rust
    fn generic<T>(t: T) {
        // --snip--
    }
    ```

  - But, Rust treats it as if it was re-written like this:

    ```rust
    // This means generic functions will only work on
    // types who's size is known at the compile time
    fn generic<T: Sized>(t: T) {
        // --snip--
    }
    ```

  - It's possible to get over with this restriction:

    ```rust
    // The ?Sized means “T may or may not be Sized”
    // Now, this fn will accept T whose size may or may not be known at compile time
    // The ?Trait syntax with this meaning is only available for Sized, not any other traits.
    // Also, notice, we're using `&T` and not `T`, now we'll use `T` behind some kind of pointer, here it's reference
    fn generic<T: ?Sized>(t: &T) {
        // --snip--
    }
    ```

### Advanced Functions and Closures

#### Pass functions to the functions

- Just like you can pass closures to the functions, you can also pass "funtions to the functions".
- There's a type represented by `fn`, (don't confuse it with `Fn`, that is a closure trait).
- This `fn` type is called _Function Pointer_.

- Here's how you can use it:

  ```rust
  fn add_one(x: i32) -> i32 {
    x + 1
  }

  fn do_twice(f: fn(i32) -> i32, arg: i32) -> i32 {
    f(arg) + f(arg)
  }

  fn main() {
    let answer = do_twice(add_one, 5);
    println!("Answer: {}", answer);
  }
  ```

- Function pointers implement all three of the closure traits (`Fn`, `FnMut`, and `FnOnce`), so you can always pass a function pointer as an argument for a function that expects a closure.
- So, instead of passing a closure, you can simply enter a function name, and it will work.

  ```rust
  // You can either do this
  let list_of_numbers = vec![1, 2, 3];
  let list_of_strings: Vec<String> =
      list_of_numbers
      .iter()
      .map(|i| i.to_string()) // Provide the closure
      .collect();


  // Or you can simply enter the function name
  let list_of_numbers = vec![1, 2, 3];
  let list_of_strings: Vec<String> =
      list_of_numbers
      .iter()
      .map(ToString::to_string) // Enter the function name, wohoo!
      .collect();
  ```

- You can use enum variants as an initializer function. Also, we now know we can pass a function inside a function, so here's what we can also do:

  ```rust
  enum Status {
      Value(u32), // So, this works as an initializer function too
      Stop,
  }

  let list_of_statuses: Vec<Status> = (0u32..20).map(Status::Value).collect(); // This will create Status instances of the variant Value
  ```

- You can do the same thing by using closures, it's more of a personal preference. You can use whichever feels more clearer to you.

#### Return Closures

- First of all, you can't return functions, that's not allowed in rust.
- Technically, you are not alllowed to use `fn` (Funciion Pointer) as a return type, but you can return closures.
- Closures are represented by traits.
- To return a type that implements a trait, you can do either of the two:
  - Return a concrete type
  - Use dynamic dispatch (it'll allow the function to know concrete type at runtime).
- Closures don't have the concrete type, you can't send them directly, you'll need to use dynamic dispatch.

  ```rust
  // FAIL: This will also not work
  fn returns_closure() -> dyn Fn(i32) -> i32 {
      |x| x + 1
  }
  ```

- Now, Rust needs one more thing, the size of the closure. Remember the `Sized` trait.
- The solution to this problem, is to wrap the return type with some sort of pointer, in case of strings we use references. For example, using `&str` instead of `str`.
- Here, we'll use `Box<T>` to simply store it on the heap.

  ```rust
  // This is how you can successfully return a closure
  fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
      Box::new(|x| x + 1)
  }
  ```

- In case you want to understand better how you can use traits with dynamic dispatch, you can check [here](#defining-a-common-behaviour-using-trait).

### Macros

- Rust code that writes more rust code are called _Macros_. This kind of programming is called metaprogramming.
- Here are the following things that you can only do with macros and not functions:

  - Macros can take variable number of parameters, unlike functions. You can call `println!("hello")` with one argument or `println!("hello {}", name)` with two arguments.
  - Macros are expanded before the compiler interprets the meaning of the code. For example, macros can implement a trait on a given type. Functions can't because it gets called at runtime and a trait needs to be implemented at compile time.

- Drawbacks of Macros:
  - It's hard to read, write and maintain.
  - You can define functions anywhere, but you need to bring the macros in scope before you can call them.

#### Declarative Macros

- They are the most widely used types of macros.
- Also referred to as "macros by example", “`macro_rules!` macros,” or just plain “macros.”
- They are similar to `match` statements, except they match on literal rust code, instead of some value.
- This is how the `vec!` macro is defined. It is a simplified version, and the implementation of standard library has some extra code for preallocating memory for different types.

```rust
#[macro_export]
macro_rules! vec {
    ( $( $x:expr ),* ) => {
        {
            let mut temp_vec = Vec::new();
            $(
                temp_vec.push($x);
            )*
            temp_vec
        }
    };
}
```

- Explanation:
  - `#[macro_export]` - You can't export the macro without this line. For using this macro, you'll have to bring the crate into scope where this macro is defined.
  - `macro_rules! name-of-macro` - Then we declare the macro with the `macro_rules!` along with the name of the macro without the exclamation mark. In our case, `vec`.
  - `( $( $x:expr ),* ) =>` - This is the match arm of the macro. In our case, the macro has only one match arm, if such an expression is passed to the macro which doesn't fall into it, it'll fail. Some complex macros will have multiple match arms.
    - `( ) =>` - A parantheses surrounds the whole pattern. It indicates that this is a match arm.
    - `$( )` - Anything inside this parantheses will capture values.
    - `$x: expr` - This matches any Rust expression and gives the expression the name `$x`.
    - `,` - It means that the literal `,` might appear after the code that matches the code in `$()`.
    - `*` - It means that the pattern matches zero or more of whatever precedes the `*`.

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

### Doc Tests

- You can write doc tests above the item, using the `Examples` with the docs comment `///` like this:

  ````rust
  /// Adds one to the number given.
  ///
  /// # Examples
  ///
  /// ```
  /// let arg = 5;
  /// let answer = my_crate::add_one(arg);
  ///
  /// assert_eq!(6, answer);
  /// ```
  pub fn add_one(x: i32) -> i32 {
      x + 1
  }
  ````

- Running `cargo test` will run the code examples in your documentation as tests! In case we change the function, the test will panic, and we'll require to update the docs to make it work.

  ```zsh
     Doc-tests my_crate

  running 1 test
  test src/lib.rs - add_one (line 5) ... ok

  test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.27s
  ```

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

- The names of static variables are in `SCREAMING_SNAKE_CASE` by convention.

- The names of a _type_ in rust uses CamelCase. For example, consider `T` in `Result<T>`.
- Documentation Comments use `///`, and is converted to HTML, unlike simple comments `//`. So simply add the documentation above the items. [Learn more](#documentation).

### Documentation

- Always remember that, _programmers are interested in knowing how to use your crate as opposed to how your crate is implemented_.
- You'll be using `///` for documentation. Notice that you'll need to add docs comment above your item, not inside the `{}`. Using this`///`, we're documenting the item that follows it.

  ````rust
  /// Adds one to the number given.
  ///
  /// # Examples
  ///
  /// ```
  /// let arg = 5;
  /// let answer = my_crate::add_one(arg);
  ///
  /// assert_eq!(6, answer);
  /// ```
  pub fn add_one(x: i32) -> i32 {
      x + 1
  }
  ````

- You can use `cargo doc` to generate the docs as HTML, and can open it through `target/doc` directory or by running the command `cargo doc --open`.

- Just like `Examples` as shown in the above code, you can use other sections too. Here are other [commonly used sections](https://doc.rust-lang.org/book/ch14-02-publishing-to-crates-io.html#commonly-used-sections).

- Running `cargo test` will run the code examples in your documentation as tests! In case we change the function, the test will panic, and we'll require to update the docs to make it work.

  ```zsh
     Doc-tests my_crate

  running 1 test
  test src/lib.rs - add_one (line 5) ... ok

  test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.27s
  ```

- To document a whole crate, `//!` is used in the crate root file (`src/lib.rs`). Using this `//!`, we’re documenting the item that contains this comment.

  ```rust
  //! # My Crate
  //!
  //! `my_crate` is a collection of utilities to make performing certain
  //! calculations more convenient.
  ```

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

- To generate docs, you can access them through `target/doc`:

  ```zsh
  cargo doc
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

- To install a package from crates.io into your system, you may use this command (only binary crates can be installed and ensure that `$HOME/.cargo/bin` is in your `$PATH`):

  ```zsh
  cargo install <package-name>
  ```

- To list out custom cargo commands:

  ```zsh
  cargo --list
  ```

#### The `opt-level`

- The `opt-level` setting controls the number of optimizations Rust will apply to your code, with a range of `0` to `3`.

  ```toml
  // Filename: Cargo.toml
  [profile.dev]
  opt-level = 0 // Less Optimization, less compiling time

  [profile.release]
  opt-level = 3 // More Optimizations, more compiling time
  ```

- You can override any default setting by adding a different value for it in `Cargo.toml`. To override, you can add these two lines below the above lines.

  ```toml
  // Filename: Cargo.toml
  [profile.dev]
  opt-level = 1
  ```

- To learn more about customizing profiles, you can read the docs [here](https://doc.rust-lang.org/cargo/reference/profiles.html).

For more information about Cargo, check out [its documentation](https://doc.rust-lang.org/cargo/).

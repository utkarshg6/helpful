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

- This is how `let s1 = String::from("hello");` is stored in Rust:

  - Left - Parts of String that are stored on the stack:
    1. Pointer: Points to the memory that holds the contents of the string.
    2. Length: How much memory, in bytes, the contents of the String is currently using.
    3. Capacity: The total amount of memory, in bytes, that the String has received from the allocator.
  - Right - The memory on the heap that holds the contents.

  ![Image](https://doc.rust-lang.org/book/img/trpl04-01.svg)

- Difference between String, String Literal, String Slice

  | Property          | String                                      | String Literal                 | String Slice                       |
  | ----------------- | ------------------------------------------- | ------------------------------ | ---------------------------------- |
  | Definition        | `let string = String::from("some_string");` | `let string_literal = "1234";` | `let string_slice = &string[1..3]` |
  | Mutable           | :heavy_check_mark:                          | :x:                            | :x:                                |
  | Memory Management | Heap (but deallocates when out of scope)    | Stack                          | -                                  |
  | Use Cases         | Taking Input, or any String Manipulation    | Defining Constant Strings      | Slicing and Borrowing              |

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

#### Data Race

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

#### Dangling References

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

## Slicing

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

### Which is better `&String` or `&str`?

- Short Answer: `&str`.
- Reason: It allows us to use the same function on both `&String` values and `&str` values.

  ```rust
  fn first_word(s: &String) -> &str { // This sucks, only allows &String

  fn first_word(s: &str) -> &str { // Rustaceans prefer this, since it allows both `&String` and `&str`.
  ```

- Basically, now our function will work on `&my_string[0..6]`, `&my_string[..]`, `&my_string`, `&my_string_literal[0..6]`, `&my_string_literal[..]`, and `my_string_literal`.

### Slicing an array

- We can similarly slice an array.

  ```rust
  let a = [1, 2, 3, 4, 5];

  let slice = &a[1..3]; // It is of type &[i32]
  ```

### Ranges

- You can create a range with `..` operator.
- The following are equal:
  - `1..5` ~ `1..=4`
  - `0..4` ~ `..4`
  - `1..len` ~ `1..`
  - `0..len` ~ `..`

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

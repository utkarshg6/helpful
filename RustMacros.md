# Rust Macros

## Syntax Extensions

- Rust Macros are built on the mechanism of Syntax Extension.
- We can use the term Syntax Extension to refer all of rust's different macro kinds.

Let's understand how Rust source is processed by the compiler?

### Stage 1 : Tokenization

- In this stage, the source text is converted into _tokens_.
- Tokens are equivalent to "words" in a written text.
- For example, in the text string: `The quick brown fox jumps over the lazy dog`, there are 9 tokens seperated by `" "`.
- Tokens are the indivisible lexical units.
- Rust has various kinds of tokens, such as:
  - Identifiers: `foo`, `Bambous`, `self`, `we_can_dance`, `LaCaravane`, …
  - Literals: `42`, `72u32`, `0_______0`, `1.0e-40`, `"ferris was here"`, …
  - Keywords: `_`, `fn`, `self`, `match`, `yield`, `macro`, …
  - Symbols: `[`, `:`, `::`, `?`, `~`, `@`, …
- Notice that `:` and `::` are distinct tokens. So, rust needs to distinguish between multi-character symbol tokens.
- Rust has two lexers:
  - [`rustc_lexer`](https://github.com/rust-lang/rust/tree/master/compiler/rustc_lexer): Only emits single character symbols as tokens
  - Lexer in [`rustc_parse`](https://github.com/rust-lang/rust/tree/master/compiler/rustc_parse): It sees multi-character symbols as distinct tokens.
- Also, `self` is both an identifier and a keyword. It doesn't makes our lives easier, but that's how things work in Rust.
- In programming languages like C/C++, there is a macro layer at this level. So, the code written below compiles:

  ```c
  #define SUB void
  #define BEGIN {
  #define END }

  SUB main() BEGIN
      printf("Oh, the horror!\n");
  END
  ```

- Rust does it differently, it doesn't have a macro layer at this stage.

### Stage 1.5 : Token Trees

- Consider this stream of Tokens:

  ```
  a + b + (c + d[0]) + e
  ```

- It would be parsed into the following token trees:

  ```
  «a» «+» «b» «+» «(   )» «+» «e»
            ╭────────┴──────────╮
             «c» «+» «d» «[   ]»
                          ╭─┴─╮
                           «0»

  ```

- You can call smaller entities as leaves which are branching out from a tree.

### Stage 2 : Parsing

- In this stage the stream of tokens is converted into [Abstract Syntax Tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree).
- For example, the token stream `1 + 2` gets transformed into this tree:

  ```
  ┌─────────┐   ┌─────────┐
  │ BinOp   │ ┌╴│ LitInt  │
  │ op: Add │ │ │ val: 1  │
  │ lhs: ◌  │╶┘ └─────────┘
  │ rhs: ◌  │╶┐ ┌─────────┐
  └─────────┘ └╴│ LitInt  │
                │ val: 2  │
                └─────────┘
  ```

- This AST contains the structure of _entire_ program.
- At an earlier stage, the compiler may know what a variable `a` is.
- But at this stage compiler has no way of knowing what variable `a` is and where it comes from.
- After this stage the macros are processed.

Here's a wrap up:

- This is a stream of token:

  ```
  a + b + (c + d[0]) + e
  ```

- This is a token tree:

  ```
  «a» «+» «b» «+» «(   )» «+» «e»
            ╭────────┴──────────╮
             «c» «+» «d» «[   ]»
                          ╭─┴─╮
                           «0»

  ```

- This is an AST:

  ```
                ┌─────────┐
                │ BinOp   │
                │ op: Add │
              ┌╴│ lhs: ◌  │
  ┌─────────┐ │ │ rhs: ◌  │╶┐ ┌─────────┐
  │ Var     │╶┘ └─────────┘ └╴│ BinOp   │
  │ name: a │                 │ op: Add │
  └─────────┘               ┌╴│ lhs: ◌  │
                ┌─────────┐ │ │ rhs: ◌  │╶┐ ┌─────────┐
                │ Var     │╶┘ └─────────┘ └╴│ BinOp   │
                │ name: b │                 │ op: Add │
                └─────────┘               ┌╴│ lhs: ◌  │
                              ┌─────────┐ │ │ rhs: ◌  │╶┐ ┌─────────┐
                              │ BinOp   │╶┘ └─────────┘ └╴│ Var     │
                              │ op: Add │                 │ name: e │
                            ┌╴│ lhs: ◌  │                 └─────────┘
                ┌─────────┐ │ │ rhs: ◌  │╶┐ ┌─────────┐
                │ Var     │╶┘ └─────────┘ └╴│ Index   │
                │ name: c │               ┌╴│ arr: ◌  │
                └─────────┘   ┌─────────┐ │ │ ind: ◌  │╶┐ ┌─────────┐
                              │ Var     │╶┘ └─────────┘ └╴│ LitInt  │
                              │ name: d │                 │ val: 0  │
                              └─────────┘                 └─────────┘

  ```

### Stage 3 : Processing Macros from AST

- After the AST is constructed, the macros are porocessed.
- The following syntax extensions are used for processing macros:

  | Syntax                | Type                     | Example                                               |
  | --------------------- | ------------------------ | ----------------------------------------------------- |
  | `#[ $arg ]`           | Attributes               | `#[derive(Clone)]`, `#[no_mangle]`, ...               |
  | `#![ $arg ]`          | Attributes               | `#![allow(dead_code)]`, `#![crate_name="blang"]`, ... |
  | `$name ! $arg`        | Function Like Macro      | `println!("Hi!")`, `concat!("a", "b")`, ...           |
  | `$name ! $arg0 $arg1` | Used to construct macros | `macro_rules! dummy { () => {}; }`                    |

- The first two are attributes, which annotate items, expressions and statements. They are divided into:

  | Attribute Type              | Implemented By   | Example                                                           |
  | --------------------------- | ---------------- | ----------------------------------------------------------------- |
  | Built-in Attributes         | Compiler         | `#[test]`, `#[ignore]`, `#[cfg]`, `#![allow(clippy::filter_map)]` |
  | Procedural Macro Attributes | Procedural Macro | `#[MyProceduralMacro]`                                            |
  | Derive Attributes           | Procedural Macro | `#[derive(Clone)]`                                                |

- To differentiate between the third and the fourth type, Rust looks for brackets `(...)`, `[...]`, or `{...}`.
- For example, if it's `println!(...)`, `vec![...]` or `lazy_static! {...}`, it's of the third type.
- If it's like this `macro_rules! dummy { () => {}; }`, then it's of the fourth type.
- At this point, Rust doesn't cares what's inside the brackets, it can even allow invalid Rust code.
- These Syntax Extension can appear in place of:

  | Item                         | Allowed |
  | ---------------------------- | ------- |
  | Patterns                     | ✓       |
  | Statements                   | ✓       |
  | Expressions                  | ✓       |
  | Items (including impl items) | ✓       |
  | Types                        | ✓       |
  | Identifiers                  | ✗       |
  | Match arms                   | ✗       |
  | Struct fields                | ✗       |

- In conclusion, Syntax extensions are parsed as part of the abstract syntax tree.

### Stage 4 : Expansion

- This stage involves traversing the AST, locating syntax extension invocations and replacing them with their expansion.
- After the syntax extensions are expanded inside the AST, the compiler will start constructing it's semantic understanding of the program.
- A sytax extension can be expanded into:
  - an expression,
  - a pattern,
  - a type,
  - zero or more items, or
  - zero or more statements.
- Here's an example how the invocation will work inside AST:

  - Consider this:

    ```rust
    let eight = 2 * four!();
    ```

  - Here's the partial AST:

    ```
    ┌─────────────┐
    │ Let         │
    │ name: eight │   ┌─────────┐
    │ init: ◌     │╶─╴│ BinOp   │
    └─────────────┘   │ op: Mul │
                    ┌╴│ lhs: ◌  │
         ┌────────┐ │ │ rhs: ◌  │╶┐ ┌────────────┐
         │ LitInt │╶┘ └─────────┘ └╴│ Macro      │
         │ val: 2 │                 │ name: four │
         └────────┘                 │ body: ()   │
                                    └────────────┘

    ```

  - Let's assume `four!()` is expanded into `1 + 3`. So, this is how the final AST will look like:

    ```
    ┌─────────────┐
    │ Let         │
    │ name: eight │   ┌─────────┐
    │ init: ◌     │╶─╴│ BinOp   │
    └─────────────┘   │ op: Mul │
                    ┌╴│ lhs: ◌  │
         ┌────────┐ │ │ rhs: ◌  │╶┐ ┌─────────┐
         │ LitInt │╶┘ └─────────┘ └╴│ BinOp   │
         │ val: 2 │                 │ op: Add │
         └────────┘               ┌╴│ lhs: ◌  │
                       ┌────────┐ │ │ rhs: ◌  │╶┐ ┌────────┐
                       │ LitInt │╶┘ └─────────┘ └╴│ LitInt │
                       │ val: 1 │                 │ val: 3 │
                       └────────┘                 └────────┘
    ```

- Every expression that's expanded from a macro is a complete AST Node. In other words, the expression will be surrounded by paranthesis.

  ```rust
  let eight = 2 * (1 + 3);
  ```

- There are important implications of the fact that a macro is expanded into a complete Node:

  - A macro can only expand to the kind of AST node the parser expects at that position.
  - As a consequence of the above, a macro cannot expand to incomplete or syntactically invalid constructs.

- Another takeaway is that, the expansion occurs, as many an AST requires upto the recursion depth of `128`:

  - Consider this:

    ```rust
    let x = four!();
    ```

  - This is expanded into:

    ```rust
    let x = 1 + three!();
    ```

  - This can be further expanded into:

    ```rust
    let x = 1 + 3;
    ```

# Rust Macros

## Syntax Extensions

- Rust Macros are built on the mechanism of Syntax Extension.
- We can use the term Syntax Extension to refer all of rust's different macro kinds.

Let's understand how Rust source is processed by the compiler?

Stage 1 : Tokenization

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

Stage 1.5: Token Trees

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

Stage 2: Parsing

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

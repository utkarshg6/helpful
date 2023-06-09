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
- In programming languages C/C++, there is a macro layer at this level. So, the code below compiles:

  ```c
  #define SUB void
  #define BEGIN {
  #define END }

  SUB main() BEGIN
      printf("Oh, the horror!\n");
  END
  ```

- Rust does it differently, it doesn't have a macro layer at this stage.

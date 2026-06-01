# Rust Vocabulary — Master Glossary

A running list of Rust terms as I learn them, organized by session.

> This is a living document — updated as I progress through the course.

---

## Session 1 — Introduction to Rust

| Term | Meaning |
|---|---|
| Compiler | The tool that checks your code before it runs. In Rust, it rejects unsafe or incorrect code before execution |
| Memory bug | When a program reads or writes to memory it should not — a common cause of crashes |
| Concurrency | Multiple parts of a program running at the same time. Rust prevents data corruption in concurrent code |
| Garbage collector | A background process in languages like Python or JavaScript that cleans up unused memory automatically. Rust does not have one |
| Ownership | Rust's system for tracking which part of a program is responsible for each piece of memory |
| Production | When software is live and real users are using it |

---

## Session 2 — Rust Basics

| Term | Meaning |
|---|---|
| `let` | Declares a variable. Immutable by default |
| `mut` | Makes a variable mutable — allows its value to change |
| `const` | A constant value that never changes. Always needs a type annotation |
| Shadowing | Reusing a variable name by declaring a new binding with `let` |
| `i32` / `i64` | Signed integers — can be positive or negative |
| `u64` | Unsigned integer — can never be negative. Used for amounts like lamports |
| `bool` | A true or false value |
| `String` | Owned, growable text. Used for text you build or modify at runtime |
| `&str` | Borrowed text. Used for fixed string literals |

---

## Session 3 — Ownership, Borrowing & Memory

| Term | Meaning |
|---|---|
| Stack | Fast memory for values with a known size at compile time. Automatically cleaned up |
| Heap | Flexible memory for values that can grow at runtime |
| Move | When a heap value is assigned to a new variable, ownership transfers and the original is gone |
| Copy | When a stack value is assigned, a full copy is made and both remain valid |
| Clone | Explicitly making a full deep copy of a heap value |
| Reference (`&`) | Borrowing a value without taking ownership |
| Mutable reference (`&mut`) | Borrowing a value with permission to change it. Only one allowed at a time |
| Borrow | Using a value through a reference without owning it |
| Scope | The region of code where a variable is valid, defined by `{ }` |
| Dangling reference | A reference to a value that has already been dropped. Rust prevents these |

---

## Session 4 — Collections, Enums & Error Handling

| Term | Meaning |
|---|---|
| `match` | Checks a value against multiple patterns. Every case must be handled |
| Enum | A type that can be one of several named variants |
| `Vec<T>` | A growable list of same-type values |
| `HashMap<K, V>` | A key-value store |
| `Option<T>` | A value that is either `Some(value)` or `None`. Rust's replacement for null |
| `Some` | The `Option` variant that contains a value |
| `None` | The `Option` variant that contains nothing |
| `Result<T, E>` | Either `Ok(value)` on success or `Err(reason)` on failure |
| `Ok` | The success variant of `Result` |
| `Err` | The failure variant of `Result` |
| `panic!` | Immediately stops the program. For unrecoverable bugs only |

---

## Session 5 — Structs, impl Blocks & State Design

| Term | Meaning |
|---|---|
| Struct | A custom type that groups related named fields |
| Instance | A specific value created from a struct |
| Field | A named value inside a struct |
| Dot notation | How you access a struct field: `wallet.balance` |
| `impl` block | Where you define methods and functions that belong to a struct |
| Method | A function inside an `impl` block that operates on an instance |
| `&self` | Read-only access to the instance |
| `&mut self` | Mutable access — lets the method change fields |
| `self` | Takes full ownership of the instance inside the method |
| Associated function | A function in `impl` with no `self`. Called with `::`. Typically a constructor |
| `#[derive(Debug)]` | Enables debug printing of a struct with `{:?}` |

---

## Session 6 — Error Handling

| Term | Meaning |
|---|---|
| `panic!` | Stops the program immediately. For unrecoverable situations only |
| Recoverable error | An expected failure the program can handle and keep running |
| Unrecoverable error | An error the program cannot continue from |
| `?` operator | Propagates an error upward automatically. Replaces repetitive `match` |
| Error propagation | Passing an error up to the calling function instead of handling it immediately |
| Custom error enum | An enum whose variants are specific error types |
| `thiserror` crate | A Rust package for defining custom error types cleanly |

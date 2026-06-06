---
title: "Session 4 — Collections, Enums & Error Handling"
description: "How Rust stores lists of data, handles different states, and deals with things that can go wrong."
---

# Session 4 — Collections, Enums & Error Handling

**Recording:** [Google Drive](https://drive.google.com/file/d/1lEWFvHlaUxhzTcLFQje7kCbM6dkQ6KeN/view?usp=sharing)
**Instructor:** Emmanuel
**Course:** Rust School

---

## What this session covers

This session introduces the tools Rust uses to store groups of data, model different states, and handle things that can go wrong. The instructor covers `match` for pattern matching, `Vec` for growable lists, `HashMap` for key-value lookups, `Option` for values that might not exist, and `Result` for operations that might fail. By the end you will understand how Solana programs handle errors and why `Result` appears everywhere in Rust code.

---

## Terms you will learn

- **`match`** — checks a value against multiple patterns and runs the matching branch. Every case must be handled or the code won't compile
- **Enum** — a type that can be one of several named states (variants)
- **`Vec<T>`** — a growable list of same-type values. Like an array that can expand at runtime
- **`HashMap<K, V>`** — a key-value store. Give it a key, get back a value
- **`Option<T>`** — a value that is either `Some(value)` or `None`. Rust's replacement for null
- **`Some`** — the variant of `Option` that contains a value
- **`None`** — the variant of `Option` that contains nothing
- **`Result<T, E>`** — the result of an operation that either succeeded (`Ok`) or failed (`Err`)
- **`Ok`** — the variant of `Result` that means success
- **`Err`** — the variant of `Result` that means failure, with a reason
- **Panic** — an unrecoverable error that stops the program immediately
- **Recoverable error** — an expected problem that can be handled gracefully. Use `Result`
- **Unrecoverable error** — a programming bug that should stop everything. Use `panic!`

---

## Key questions this session answers

- How do I store a list of values that can grow?
- How do I look up a value by name or key?
- What happens when something might not exist?
- How does Rust handle errors without exceptions?
- What is the difference between a recoverable and unrecoverable error?
- Why does every Solana instruction handler return `Result`?

---

## Pages

- [Lesson Recap →](./02-lesson-recap.md)
- [Study Guide →](./03-study-guide.md)
- [Homework →](./04-homework.md)
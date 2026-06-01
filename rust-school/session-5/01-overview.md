---
title: "Session 5 — Structs, impl Blocks & State Design"
description: "How Rust models real-world data as structured objects and attaches behaviour with impl blocks."
---

# Session 5 — Structs, impl Blocks & State Design

**Recording:** [Google Drive](https://drive.google.com/file/d/1bFHNJng7zua8O0iFktyGBZ3cyiksqFKG/view?usp=sharing)
**Course:** Rust School

---

## What this session covers

This session covers structs — how Rust groups related data under one name — and `impl` blocks, which attach behavior to that data. The instructor builds a `Wallet` struct live in class, adds methods with `impl`, and connects everything directly to how Solana programs store and manage on-chain account data. By the end you will understand that every Solana account is a Rust struct.

---

## Terms you will learn

- **Struct** — a custom type that groups related fields under one name. Like a blueprint for a piece of data
- **Instance** — a specific value created from a struct definition. The actual data, not the blueprint
- **Field** — a named value inside a struct (for example, `balance: u64`)
- **Dot notation** — the way to access a field from an instance: `wallet.balance`
- **`impl` block** — where you define methods and functions that belong to a struct
- **Method** — a function inside an `impl` block that operates on an instance
- **`&self`** — gives a method read-only access to the instance without taking ownership
- **`&mut self`** — gives a method the ability to modify the instance. The variable must be `mut`
- **`self`** — takes full ownership of the instance inside the method. The original is gone after the call
- **Associated function** — a function inside `impl` that does not take `self`. Called with `::`. Typically used as a constructor
- **`#[derive(Debug)]`** — an attribute that lets you print a struct with `{:?}` for debugging

---

## Key questions this session answers

- How do I group related data — like a name, balance, and status — under one name?
- What is the difference between a struct definition and an instance?
- How do I read and change individual fields?
- What is the difference between `&self`, `&mut self`, and `self` in a method?
- What is an associated function and when do I use `::` instead of `.`?
- How does all of this connect to Solana account data?

---

## Pages

- [Synthesis →](./02-synthesis.md)
- [Lesson →](./03-lesson.md)
- [Study Guide →](./04-study-guide.md)
- [Homework →](./05-homework.md)
- [Exercise →](./06-exercise.md)

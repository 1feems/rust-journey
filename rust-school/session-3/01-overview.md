---
title: "Session 3 — Ownership, Borrowing & Memory"
description: "How Rust manages memory without a garbage collector — and why this is the most important session in the course."
---

# Session 3 — Ownership, Borrowing & Memory

**Recording:** [Google Drive](https://drive.google.com/file/d/1SgKLgvU9qtFQhkBqTyWHgEG9IR34K-sJ/view?usp=sharing)
**Instructor:** Nicholas
**Course:** Rust School

---

## What this session covers

This session explains how Rust manages memory — without a garbage collector. The instructor covers where data lives (stack vs heap), what happens when you assign a variable or pass it to a function (move vs copy), and how to share data safely using references. By the end you will understand ownership — the concept that makes Rust unique — well enough to read and fix ownership errors from the compiler.

---

## Terms you will learn

- **Stack** — fast memory for values whose size is known at compile time (integers, booleans, characters). Automatically cleaned up when a function ends
- **Heap** — flexible memory for values that can grow at runtime (strings, vectors, structs). Must be managed carefully
- **Ownership** — Rust's rule that every value has exactly one owner. When the owner is done, the value is dropped
- **Owner** — the variable that is responsible for a value. When the owner goes out of scope, the value is freed
- **Scope** — the region of code where a variable is valid, defined by curly braces `{ }`
- **Move** — when a heap value is assigned to a new variable, ownership transfers and the original is gone
- **Copy** — when a stack value is assigned, a full copy is made and both are valid
- **Clone** — explicitly making a full deep copy of a heap value so both copies are valid
- **Reference (`&`)** — borrowing a value without taking ownership. The original stays valid
- **Mutable reference (`&mut`)** — borrowing a value with permission to change it. Only one allowed at a time
- **Dangling reference** — a reference to a value that no longer exists. Rust prevents these at compile time
- **Borrow** — using a value through a reference without owning it

---

## Key questions this session answers

- Where does data actually live in memory — and why does it matter?
- What happens to a variable after you assign it to another variable?
- Why can't you use a `String` after you've passed it to a function?
- How can you use a value in two places without moving it?
- What is the difference between `&` and `&mut`?
- Why does Rust prevent you from having two mutable references at the same time?

---

## Pages

- [Synthesis →](./02-synthesis.md)
- [Lesson →](./03-lesson.md)
- [Study Guide →](./04-study-guide.md)
- [Homework →](./05-homework.md)
- [Exercise →](./06-exercise.md)

---
title: "Session 6 — Error Handling"
description: "How Rust handles things going wrong — safely, predictably, and without crashing."
---

# Session 6 — Error Handling

**Recording:** (check Telegram group for session resources)
**Instructor:** Sanjay
**Course:** Rust School

---

## What this session covers

Every program you build will eventually encounter something going wrong — a file that doesn't exist, a request that fails, a balance that runs out. This session teaches you how Rust handles those situations safely. You will learn the difference between errors that crash the program and errors you can recover from, and how to write code that handles both clearly.

---

## Terms you will learn

- **Panic** — a command that stops the program completely. Used only for critical, unrecoverable situations
- **Unrecoverable error** — an error so serious the program cannot continue. Handled with `panic!`
- **Recoverable error** — an expected failure the program can handle and keep running. Handled with `Result`
- **`Result<T, E>`** — a type that represents either success (`Ok`) or failure (`Err`)
- **`Ok`** — the `Result` variant that means the operation worked. Contains the success value
- **`Err`** — the `Result` variant that means the operation failed. Contains the reason
- **`?` operator** — shorthand that propagates an error upward automatically, without writing a full `match`
- **Error propagation** — passing an error up to the calling function to handle, instead of handling it immediately
- **Custom error enum** — an enum whose variants represent specific error types, replacing inconsistent strings
- **`thiserror` crate** — a Rust package that makes defining custom error types cleaner and more standard

---

## Key questions this session answers

- What is the difference between a recoverable and an unrecoverable error?
- When should I use `panic!` and when should I use `Result`?
- How do I write a function that can fail without crashing the program?
- What does `Ok` and `Err` mean and how do I handle both?
- What does the `?` operator do and why does it make code cleaner?
- Why use an enum for errors instead of a String?
- What is the `thiserror` crate and when would I use it?

---

## Pages

- [Synthesis →](./02-synthesis.md)
- [Lesson →](./03-lesson.md)
- [Study Guide →](./04-study-guide.md)
- [Homework →](./05-homework.md)
- [Exercise →](./06-exercise.md)

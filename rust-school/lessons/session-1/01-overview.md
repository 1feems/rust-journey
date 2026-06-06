---
title: "Session 1 — Introduction to Rust"
description: "Why Rust exists, what the compiler actually does, and why Solana chose it."
---

# Session 1 — Introduction to Rust

**Recording:** [Loom folder](https://www.loom.com/share/folder/69cc455474cf45c891d4d43dd25a0143)
**Instructor:** Haripria
**Course:** Rust School

---

## What this session covers

This session answers one question: why does Rust exist? The instructor explains what problems Rust was built to solve — memory bugs and concurrency bugs — and why its strict compiler is the feature, not the obstacle. By the end you will understand why Solana chose Rust and what to expect from the rest of the course.

---

## Terms you will learn

- **Compiler** — the tool that checks your code before it runs. In Rust, it catches bugs at this stage instead of letting them reach production
- **Memory bug** — when a program reads or writes to memory it should not. A common cause of crashes and security holes in other languages
- **Concurrency** — multiple things running at the same time. Rust prevents data corruption when concurrent code shares memory
- **Garbage collector** — a background process in many languages that cleans up unused memory for you. Rust does not have one — it handles memory through ownership instead
- **Ownership** — Rust's system for tracking which part of a program is responsible for each piece of data. Covered in depth in Session 3
- **Production** — when software is live and real users are using it. Where bugs that slipped past development show up

---

## Key questions this session answers

- Why is everyone suddenly talking about Rust?
- What makes Rust different from other languages like Python or JavaScript?
- Why does the compiler feel so strict — and why is that actually a good thing?
- Why did Solana choose Rust for smart contracts?
- What will this course teach you, and in what order?

---

## Pages

- [Lesson Recap →](./02-lesson-recap.md)
- [Study Guide →](./03-study-guide.md)
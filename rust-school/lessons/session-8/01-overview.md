# Session 8 — Traits and Abstractions

**Course:** Rust School
**Instructor:** Sanjay

---

## What this session covers

Traits are one of the most important tools in Rust. They let you define a shared set of behaviors that different types can all agree to follow. This session walks through what traits are, how to implement them on structs, and two ways to use them in functions — one that works at compile time (faster) and one that works at runtime (more flexible).

---

## Terms you will learn

- **Trait** — A contract. It defines what functions a type must have, without saying how they work.
- **impl Trait for Struct** — How you implement a trait on a specific type (like making Dog follow the Animal contract).
- **Static dispatch** — When Rust knows exactly which type it's dealing with at compile time. Uses `impl Trait` syntax. Fast.
- **Dynamic dispatch** — When Rust figures out the type at runtime. Uses `Box<dyn Trait>`. Flexible.
- **Box** — A smart pointer that stores data in the heap instead of the stack. Required for dynamic dispatch.
- **Stack** — Fast memory. Size must be known at compile time.
- **Heap** — Flexible memory. Size can grow or change at runtime.
- **V-table** — A lookup table in the heap that stores function pointers for dynamic dispatch.

---

## Key questions this session answers

- What is a trait and why does Rust use them instead of classes?
- How do you make two different structs share the same function?
- What's the difference between `impl Trait` and `Box<dyn Trait>`?
- Why can't you store two different types in the same Rust vector — and how do traits fix that?
- What is Box and why do you need it for dynamic dispatch?
- What is the stack vs the heap, and when does each one get used?

---

## Pages

- [Lesson Recap →](./02-lesson-recap.md)
- [Study Guide →](./03-study-guide.md)
- [Homework →](./04-homework.md)
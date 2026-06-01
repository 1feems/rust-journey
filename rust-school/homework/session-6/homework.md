---
title: "Session 6 — Homework"
description: "Explore real production Rust error handling in the IronCloud codebase."
---

# Session 6 — Homework

**Lesson Connection:** Session 6 — Error Handling

---

## Homework Assigned

Explore the IronCloud codebase and study how error handling is implemented in a real production Rust system.

---

## Homework Goal

Learning syntax is one thing. Understanding how large real-world systems are actually built is a completely different skill. This homework builds that second skill — reading production code and recognising patterns you have just learned.

---

## What To Do

The IronCloud repository link will be shared on the Telegram group.

Once you have it, explore the source folder and look for the following:

**1. How errors are defined**
Are they using enums, structs, or custom types? Do they use `thiserror`? What variants exist?

**2. Where Result is used**
Find functions that return `Result`. Ask yourself: what operation can fail here? How does the system respond if it does?

**3. The ? operator in action**
Find places where `?` is used. Notice how errors move from one function up to the next without deep nested match blocks.

**4. Production patterns**
Look for structured error messages, validation checks, layered error handling, and places where `panic!` is deliberately avoided.

---

## What To Submit

Share one of the following on the Telegram group or wherever the bootcamp collects submissions:

- A short write-up (a few sentences) describing what you found
- A screenshot of a piece of code that caught your attention
- A code snippet with your own annotation explaining what it does
- A small post describing what you discovered and learned

You do not need to understand the entire codebase. Focus on observation and pattern recognition.

---

## Stretch Challenge

After reading the IronCloud code, try to apply one pattern you saw to your own banking system example from class. Define a custom error enum for it, use `?` to propagate errors between two functions, and write a `main` that handles the final `Result`.

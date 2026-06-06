---
title: "Session 2 — Homework"
description: "Practice variables, loops, and functions by building small Rust programs."
---

# Session 2 — Homework

**Lesson Connection:** Session 2 — Rust Basics

---

## Homework Assigned

Practice loop patterns. Write small programs that use loops to print shapes in the terminal — rows of stars, triangles, or diamond patterns.

---

## Homework Goal

Build comfort with loop logic before adding more complexity. By the end you should be able to:
- Write any of the three loop types from memory
- Use a counter variable inside a loop
- Nest one loop inside another
- Understand why Rust's immutability rules matter even in simple programs

---

## Final Expected Outcome

A Rust program that:
- Prints a simple shape using nested loops
- Compiles and runs without errors
- Uses at least one `for` loop and at least one counter or conditional

---

## Concepts Practiced

- `let` and `mut`
- `for` loops with ranges
- `while` loops with counters
- Nested loops
- `println!` formatting

---

## What You Are Building

A terminal program that prints patterns using asterisks (`*`). Start with a simple row, then build up to a square, then a triangle.

This is not about art — it's about training your hands and your eyes to read loop logic.

---

## Step-by-Step Homework Breakdown

### Step 1 — Print a Single Row

**What You Need To Do**

Print five stars on one line using a `for` loop.

**Why This Step Matters**

This is the simplest loop. Getting it right before nesting matters.

**Suggested Starter Code**

```rust
fn main() {
    for _ in 0..5 {
        print!("* ");
    }
    println!();   // moves to next line after the row
}
```

**Expected Result**

```
* * * * *
```

---

### Step 2 — Print a Square

**What You Need To Do**

Print a 5x5 square of stars using two nested `for` loops. Outer loop = rows, inner loop = columns.

**Why This Step Matters**

Nested loops are everywhere — iterating accounts within blocks, processing rows in a table. This pattern is worth drilling.

**Suggested Starter Code**

```rust
fn main() {
    for _row in 0..5 {
        for _col in 0..5 {
            print!("* ");
        }
        println!();
    }
}
```

**Expected Result**

```
* * * * *
* * * * *
* * * * *
* * * * *
* * * * *
```

---

### Step 3 — Print a Triangle

**What You Need To Do**

Print a right-aligned triangle where row 1 has 1 star, row 2 has 2 stars, and so on up to 5.

**Why This Step Matters**

The inner loop range changes based on the outer loop variable. This is a common pattern: "do something N times where N depends on context."

**Suggested Starter Code**

```rust
fn main() {
    for row in 1..=5 {
        for _ in 0..row {
            print!("* ");
        }
        println!();
    }
}
```

**Expected Result**

```
*
* *
* * *
* * * *
* * * * *
```

---

### Step 4 — Use a `while` Loop

**What You Need To Do**

Rewrite Step 2 (the square) using a `while` loop instead of a `for` loop.

**Why This Step Matters**

`while` loops are useful when you don't know the exact range upfront — only the condition. Rewriting a `for` loop as a `while` loop makes clear how they relate.

**Suggested Starter Code**

```rust
fn main() {
    let mut row = 0;
    while row < 5 {
        let mut col = 0;
        while col < 5 {
            print!("* ");
            col += 1;
        }
        println!();
        row += 1;
    }
}
```

**Expected Result**

```
* * * * *
* * * * *
* * * * *
* * * * *
* * * * *
```

---

## Common Mistakes

- **Forgetting `mut` on the counter:** `let row = 0` — then `row += 1` will fail. Counters always need `mut`.
- **Off-by-one with ranges:** `0..5` gives 0,1,2,3,4 (not 5). Use `0..=5` if you want to include 5.
- **Missing `println!()` after inner loop:** Without it, all rows print on one line.
- **Using `print!` vs `println!`:** `print!` stays on the same line. `println!` adds a newline.

---

## Debugging Hints

- If your shape looks like one long row, you're missing the `println!()` at the end of each row
- If your triangle has one too many or too few rows, check whether your range is `0..row` or `0..=row`
- If you get a "cannot assign twice to immutable variable" error, add `mut` to your counter declaration

---

## Stretch Goals

- Print a diamond pattern (triangle going up, then down)
- Print a hollow square (stars only on the border, spaces in the middle)
- Refactor your shape-printing logic into a function that takes a `size: u32` parameter

---

## Completion Checklist

- [ ] Step 1 compiles and prints a single row of stars
- [ ] Step 2 prints a 5x5 square
- [ ] Step 3 prints a growing triangle
- [ ] Step 4 rewrites the square with a `while` loop
- [ ] I understand why the counter variable needs `mut`
- [ ] I understand the difference between `0..5` and `0..=5`

---

## Paste Your Code Here

When you're done, paste your final solution below and ask Claude to review it.

```rust
// paste your code here
```

> Ask Claude: **"Review my Session 2 homework"**

---

## Reflection Questions

- Which loop type felt most natural to write?
- What happened when you forgot `mut` on the counter — what did the compiler say?
- How would you modify the triangle to point downward instead of upward?
- Where might a nested loop show up in a real program?

---

## Personal Notes

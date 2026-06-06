---
title: "Session 3 — Homework"
description: "Build the Safe Data Transfer Program — pass data between functions using ownership and borrowing."
---

# Session 3 — Homework

**Lesson Connection:** Session 3 — Ownership, Borrowing & Memory

---

## Homework Assigned

Build the **Safe Data Transfer Program.**

- Pass a user's name and message between multiple functions
- Fix ownership errors by converting to references
- Add a mutable borrow to modify data in place
- Deliberately break borrowing rules to read the compiler errors

---

## Homework Goal

Make the ownership and borrowing rules real by seeing what breaks and what fixes it. By the end you should be able to:
- Recognize a "value moved" compiler error and know how to fix it
- Choose between passing by value, `&`, or `&mut` depending on what the function needs
- Read a dangling reference error and understand why Rust stops you

---

## Final Expected Outcome

A Rust program that:
- Stores a name as a `String`
- Passes it to two different functions without moving ownership
- Modifies a `String` in place using a mutable reference
- Compiles cleanly — no errors, no unused variable warnings

---

## Concepts Practiced

- Ownership and move
- Immutable references (`&`)
- Mutable references (`&mut`)
- Borrowing rules
- Dangling reference prevention

---

## What You Are Building

A small program that takes a name, greets it, shouts it, and appends a signature — all without cloning or duplicating data unnecessarily. The point is to use the right kind of reference for each job.

---

## Step-by-Step Homework Breakdown

### Step 1 — Write the Broken Version

**What You Need To Do**

Write `greet` and `shout` functions that each take ownership of a `String`. Call both from `main` using the same variable. Read the error.

**Why This Step Matters**

The compiler error here is the lesson. You need to see the "value used after move" error before the fix makes sense.

**Suggested Starter Code**

```rust
fn greet(name: String) {
    println!("Hello {}", name);
}

fn shout(name: String) {
    println!("HEY {}", name.to_uppercase());
}

fn main() {
    let name = String::from("Amara");
    greet(name);
    shout(name);   // ← what does the compiler say here?
}
```

**Expected Result**

A compiler error: `value used after move`. Read the full message — it tells you exactly where ownership was moved.

---

### Step 2 — Fix It with Borrowing

**What You Need To Do**

Change both function signatures to accept `&String` instead of `String`. Update the call sites to pass `&name`.

**Why This Step Matters**

This is the most common fix for ownership errors. Instead of giving the function the value, you lend it a reference.

**Suggested Starter Code**

```rust
fn greet(name: &String) {
    println!("Hello {}", name);
}

fn shout(name: &String) {
    println!("HEY {}", name.to_uppercase());
}

fn main() {
    let name = String::from("Amara");
    greet(&name);
    shout(&name);
    println!("{}", name);  // still valid — we only borrowed
}
```

**Expected Result**

```
Hello Amara
HEY AMARA
Amara
```

---

### Step 3 — Add a Mutable Borrow

**What You Need To Do**

Write a third function `append_signature` that takes `&mut String` and appends `" — signed"` using `.push_str()`. Call it from `main` and print the result.

**Why This Step Matters**

`&mut` is the correct tool when a function needs to modify borrowed data. This shows the difference between read-only and read-write references.

**Suggested Starter Code**

```rust
fn append_signature(report: &mut String) {
    report.push_str(" — signed");
}

fn main() {
    let mut report = String::from("Session 3 report");
    append_signature(&mut report);
    println!("{}", report);
}
```

**Expected Result**

```
Session 3 report — signed
```

---

### Step 4 — Break the Rules

**What You Need To Do**

Try creating two mutable references to the same `String` at the same time. Then try having an immutable and a mutable reference at the same time. Read both compiler errors.

**Why This Step Matters**

The errors here are the compiler preventing data races. Understanding what triggers them makes it easier to write correct code the first time.

**Code to Try**

```rust
fn main() {
    let mut s = String::from("hello");

    // Test 1: two mutable references
    let r1 = &mut s;
    let r2 = &mut s;   // ← what error do you get?
    println!("{} {}", r1, r2);

    // Test 2: mutable + immutable at the same time
    let r3 = &s;
    let r4 = &mut s;   // ← what error do you get?
    println!("{} {}", r3, r4);
}
```

**Expected Result**

Two different compiler errors. Read both messages carefully — the compiler explains exactly which rule was violated.

---

### Step 5 — Try a Dangling Reference

**What You Need To Do**

Write a function that creates a `String` inside it and tries to return `&str` pointing to that local value. See what the compiler says.

**Why This Step Matters**

This is the dangling reference case. The local `String` is dropped when the function ends — but the reference would still point to that memory. Rust catches this at compile time.

**Code to Try**

```rust
fn make_greeting() -> &str {
    let s = String::from("hello");
    &s   // ← what does the compiler say?
}
```

**Expected Result**

A lifetime error. The fix is to return `String` instead of `&str` — transfer ownership rather than borrow from something that's about to be dropped.

---

## Common Mistakes

- **Passing by value when you only need to read:** Pass `&String` instead of `String` unless the function needs to consume or store the value.
- **Forgetting `mut` on the variable when you want `&mut`:** The variable itself must be declared `mut` before you can create a `&mut` reference to it.
- **Holding a borrow longer than needed:** Rust's borrow checker tracks the last use of a reference. If you get an error about conflicting borrows, check whether you're using both references at the same time.

---

## Debugging Hints

- "value used after move" → change the function to take `&String` instead of `String`
- "cannot borrow as mutable more than once" → you have two `&mut` references — release the first before creating the second
- "cannot borrow as mutable because it is also borrowed as immutable" → you have `&` and `&mut` active at the same time — restructure so they don't overlap
- Lifetime errors on return types → you're returning a reference to a local value — return the value itself instead

---

## Stretch Goals

- Refactor Step 2 to use `&str` instead of `&String` in the function signatures — both work, but `&str` is more idiomatic
- Write a function that takes ownership of a `String`, modifies it, and returns it — transferring ownership back to the caller
- Try to store two `&mut` references in a `Vec` and see what happens

---

## Completion Checklist

- [ ] Step 1: wrote the broken version and read the compiler error
- [ ] Step 2: fixed with `&` — both functions work, `name` is still valid after
- [ ] Step 3: `append_signature` modifies the report in place with `&mut`
- [ ] Step 4: triggered both mutable-borrow conflict errors and read the messages
- [ ] Step 5: saw the dangling reference error and understand why it happens
- [ ] I can explain when to use `&`, `&mut`, or pass by value

---

## Paste Your Code Here

When you're done, paste your final working version below and ask Claude to review it.

```rust
// paste your code here
```

> Ask Claude: **"Review my Session 3 homework"**

---

## Reflection Questions

- What was the first compiler error you didn't expect? What did it actually mean?
- What's the difference between passing `String` and passing `&String` to a function?
- When would you choose `&mut` over returning a new value from a function?
- How would the Safe Data Transfer pattern appear in a Solana program passing account data?

---

## Personal Notes

# Lesson Recap #5: Structs, impl Blocks & State Design

Structs are Rust’s way of grouping related data and attaching behavior through `impl` blocks.

---

## Key Takeaways

- A struct defines the shape of data but does not create values by itself
- Instances are created by filling in every field
- `impl` blocks attach methods and associated functions to a struct
- `&self` reads data while `&mut self` modifies data
- Structs become the foundation of Solana account state

---

## Why This Matters

Structs are Rust’s replacement for traditional object-oriented classes. Instead of combining inheritance and mutable objects, Rust separates:

- data (`struct`)
- behavior (`impl`)

This gives clearer ownership and memory safety.

In Solana development, accounts are represented as structs. Understanding structs is the bridge between beginner Rust and real blockchain state management.

---

## Important Rules

- Every field must be initialized when creating a struct instance
- The entire instance must be `mut` to modify fields
- Struct update syntax can move ownership of heap data like `String`
- Methods using `&mut self` require a mutable variable
- Associated functions use `::`, methods use `.`

---

## Key Terms

| Term | Meaning |
|---|---|
| Struct | A custom type that groups related fields together |
| Instance | A concrete value created from a struct |
| Field | A named value inside a struct |
| `impl` block | Where methods and associated functions are defined |
| Method | A function attached to a struct using `self` |
| Associated function | A function without `self`, usually used as a constructor |
| `&self` | Immutable borrow of an instance |
| `&mut self` | Mutable borrow allowing field changes |
| Dot notation | Accessing fields or methods with `.` |
| `#[derive(Debug)]` | Enables debug printing with `{:?}` |

---

## Key Concepts

**Defining a Struct:**
A struct acts like a blueprint describing what fields exist and their types. The struct itself does not create data.

```rust
struct User {
    username: String,
    email: String,
    active: bool,
}
```

---

**Creating an Instance:**
An instance is a real value created from the struct blueprint. Every field must be initialized.

```rust
let user1 = User {
    username: String::from("alice"),
    email: String::from("alice@example.com"),
    active: true,
};
```

---

**Methods with `impl`:**
Methods attach behavior to a struct. `&self` reads data while `&mut self` allows modification.

```rust
impl User {
    fn deactivate(&mut self) {
        self.active = false;
    }
}
```

---

**Associated Functions:**
Associated functions belong to the struct type itself and are usually used as constructors.

```rust
impl User {
    fn new(username: String) -> User {
        User {
            username,
            email: String::from(""),
            active: true,
        }
    }
}
```
---

## Step-by-Step Walkthrough

Tracing:

```rust
w.deposit(500);
```

1. Rust automatically passes `&mut w` into the method
2. `self.balance` refers to `w.balance`
3. `500` is added to the current balance
4. The original struct instance is updated in place
5. Ownership does not move because the method only borrows mutably

Final state:

```rust
balance = 500;
```

---
## Common Confusions

* “Why can’t I modify this field?”
  The variable itself was not declared `mut`.

* “Why does Rust use `self` in methods?”
  `self` represents the current struct instance.

* “Why use `String` instead of `&str`?”
  `String` owns its data and avoids lifetime complexity.

* “When do I use `.` vs `::`?”
  `.` calls methods on instances. `::` calls associated functions on the type itself.

---

## Common Mistakes

- Forgetting `mut` before calling a `&mut self` method
- Using struct update syntax and accidentally moving ownership
- Trying to print structs without `#[derive(Debug)]`
- Calling associated functions with dot notation instead of `::`

These usually happen because of Rust’s ownership and borrowing rules.

---

# Source

- Recording: https://drive.google.com/file/d/1bFHNJng7zua8O0iFktyGBZ3cyiksqFKG/view?usp=sharing
- Transcript: `pipeline/transcripts/rust-school/cleaned/lesson-5-structs-and-state-design.txt`
- Course: Rust School
- Session: 5

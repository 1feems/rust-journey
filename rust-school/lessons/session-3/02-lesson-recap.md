# Lesson Recap #3: Ownership, Borrowing & Memory

Rust manages memory without a garbage collector using one rule: every value has exactly one owner, and when the owner is done, the value is cleaned up.

---

## Key Takeaways

- Every value has one owner — when the owner goes out of scope, the value is dropped automatically
- Stack types (integers, booleans) copy automatically — both variables stay valid
- Heap types (`String`, `Vec`) move ownership — the original is gone after the move
- `&` borrows a value without taking ownership — many immutable borrows allowed at once
- `&mut` borrows with permission to modify — only one at a time, no mixing with `&`
- Rust prevents dangling references at compile time — you cannot reference freed memory

---

## Why This Matters

Most languages pick one of two trade-offs: manual memory control (C, C++) which is fast but dangerous, or garbage collection (Python, Java) which is safe but causes unpredictable pauses.

Rust does neither. Ownership rules enforced at compile time mean:
- No memory is accidentally freed twice
- No garbage collector pausing your program
- No dangling references reaching production

For Solana this matters because programs have strict compute budgets — a GC pause would blow that budget unpredictably.

---

## Important Rules

- One owner per value — always
- Stack types copy; heap types move (unless you use `clone()` or borrow with `&`)
- `&` = immutable borrow — many allowed at once
- `&mut` = mutable borrow — only one at a time, no immutable borrows at the same time
- References cannot outlive the data they point to

---

## Key Terms

| Term | Meaning |
|---|---|
| Stack | Fast memory for values with a known size at compile time. Automatically cleaned up |
| Heap | Flexible memory for values that can grow at runtime |
| Ownership | Rust's rule: every value has exactly one owner. When the owner is done, the value is dropped |
| Move | When a heap value is assigned to a new variable, ownership transfers. The original is invalidated |
| Copy | When a stack value is assigned to a new variable, a full copy is made. Both remain valid |
| Clone | Explicitly making a full deep copy of a heap value. Both copies stay valid |
| Reference (`&`) | A way to borrow a value without taking ownership. Read-only |
| Mutable reference (`&mut`) | A borrow that allows modifying the value. Only one allowed at a time |
| Dangling reference | A reference to a value that has already been dropped. Rust refuses to compile this |
| Scope | The region of code where a variable is valid, defined by `{ }` curly braces |

---

## Key Concepts

**Stack vs Heap:**
The stack holds values with a known size — integers, booleans. The heap holds values that can grow — strings, vectors. A pointer on the stack points to the actual data on the heap.

```rust
let x = 5;                        // lives on the stack
let s = String::from("hello");   // pointer on stack, data on heap
```

---

**Move vs Copy:**
Stack types copy automatically. Heap types transfer ownership — the original is gone.

```rust
let x = 5;
let y = x;
println!("{}", x);  // ✅ x was copied — both valid

let s1 = String::from("hello");
let s2 = s1;
println!("{}", s1);  // ❌ s1 was moved into s2 — original is gone
```

---

**Borrowing with `&`:**
Pass a reference instead of the value to keep ownership in the original variable.

```rust
fn print_name(name: &String) {
    println!("{}", name);
}
let name = String::from("Rust");
print_name(&name);        // lend name — don't give it away
println!("{}", name);     // ✅ name still valid
```

---

**Mutable Borrowing with `&mut`:**
One mutable borrow at a time — no immutable borrows at the same time.

```rust
fn update(name: &mut String) {
    name.push_str(" School");
}
let mut name = String::from("Rust");
update(&mut name);
println!("{}", name);   // Rust School
```

---

## Step-by-Step Walkthrough

Tracing the safe data transfer program:

```rust
greet(&name);
shout(&name);
```

1. `main` owns `name`
2. `greet` receives a reference — reads `name` but does not own it
3. When `greet` ends, the reference is dropped — `name` is still valid in `main`
4. `shout` receives another reference — same pattern
5. When `main` ends, `name` is dropped — memory is freed

Final state:

```rust
// name is dropped here — memory freed automatically, no garbage collector needed
```

---

## Common Confusions

* "Why can't I use `s1` after assigning it to `s2`?"
  `s1` was a `String` (heap type) — it was moved into `s2`. Use `&s1` to borrow instead, or `s1.clone()` to make a full copy.

* "What is the difference between `&` and `&mut`?"
  `&` is read-only — you can look but not touch. `&mut` gives permission to modify. Only one `&mut` is allowed at a time.

* "Why do I need `mut` on the variable when I'm passing `&mut`?"
  The variable itself must be declared `mut` before you can create a mutable reference to it. Both the variable declaration and the borrow need `mut`.

* "What is a dangling reference?"
  A reference to a value that has already been freed. Rust refuses to compile this — you cannot return a reference to a local variable from a function.

---

## Common Mistakes

- Using a value after moving it into another variable or function
- Creating two mutable references to the same value at the same time
- Mixing a mutable reference with an immutable reference at the same time
- Forgetting `mut` on the variable when you need `&mut`

These usually happen because Rust's borrowing rules feel unfamiliar at first — but every error message tells you exactly what rule was broken.

---

# Source

- Recording: [Google Drive](https://drive.google.com/file/d/1SgKLgvU9qtFQhkBqTyWHgEG9IR34K-sJ/view?usp=sharing)
- Transcript: `pipeline/transcripts/rust-school/cleaned/lesson-3-ownership-borrowing-memory.txt`
- Course: Rust School
- Session: 3

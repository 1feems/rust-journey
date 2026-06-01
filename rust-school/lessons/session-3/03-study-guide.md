# Study Guide — Rust School Session 3: Ownership, Borrowing & Memory

> Use this after watching the class recording and reading the synthesis.
> Ownership is Rust's most important concept. This guide builds your mental model before you need it for structs, collections, and Solana programs.

---

## 1. Lesson Recap

- Stack vs heap: simple values like integers live on the stack. Strings and structs live on the heap because their size can change. Rust tracks exactly when to free heap memory — no garbage collector needed
- Move vs copy: stack types like `i32` and `bool` are copied when assigned — both copies are valid. Heap types like `String` are moved — the original is gone after the move
- Clone: if you need both copies of a heap value, call `.clone()` — it makes a full deep copy, but it costs more memory and time
- The ownership rule: every value has exactly one owner. When the owner goes out of scope, the value is dropped and memory is freed
- Borrowing with `&`: lets a function use a value without taking ownership. The original stays valid. Many immutable borrows can exist at once
- Mutable borrowing with `&mut`: lets a function modify the value. Only one mutable borrow allowed at a time — no immutable borrows allowed at the same time
- Dangling references are a compile error in Rust. You cannot return a reference to a value that gets dropped when the function ends

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| Stack | Fast memory for values with a known, fixed size | Integers, booleans, characters | Automatically cleaned up when a function ends |
| Heap | Flexible memory for values that can grow | Strings, Vecs, structs | Requires careful management — Rust handles this automatically |
| Ownership | Every value has exactly one owner. When the owner is done, the value is dropped | Every value in Rust | Replaces the garbage collector |
| Move | When a heap value is assigned to a new variable, ownership transfers. The original is gone | `let s2 = s1;` with a `String` | Prevents two variables from pointing at the same heap data |
| Copy | When a stack value is assigned, both copies are valid | `let y = x;` with an `i32` | Stack values are cheap to copy |
| Clone | Explicitly making a full deep copy of a heap value | `let s2 = s1.clone();` | Keeps both copies valid — use when you need both |
| Reference (`&`) | A way to borrow a value without taking ownership | `&value` in a function call | Read-only, does not move the value |
| Mutable reference (`&mut`) | A borrow that allows modifying the value | `&mut value` in a function call | Only one allowed at a time |
| Scope | The region of code where a variable is valid, marked by `{ }` | Any block of code | When a variable leaves scope, it is dropped |
| Dangling reference | A reference to a value that has already been dropped | Trying to return `&local_var` | Rust refuses to compile this — prevents use-after-free bugs |

---

## 3. Big Idea

Every value in Rust has exactly one owner. You can lend it out temporarily with `&`, but only one part of your code is ever responsible for it at a time.

**Memory hook:** Think of ownership like a library book. Only one person checks it out at a time. Lending is fine — but you still own the book.

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| Stack vs heap | Stack = fast, fixed size. Heap = flexible, needs tracking | `i32` on stack; `String` on heap | Simple values on stack, growing values on heap |
| The ownership rule | One owner per value. Owner gone = value gone | `let s2 = s1;` — s1 is gone | No garbage collector needed |
| Move | Heap types transfer ownership on assignment | `let s2 = s1;` invalidates `s1` | The original variable is gone after a move |
| Copy | Stack types get duplicated automatically | `let y = x;` — both x and y work | Cheap to copy, so Rust just does it |
| Clone | Explicit full copy of heap data | `s1.clone()` | Expensive but keeps both |
| Borrowing | Use a value without taking ownership | `&s` — s stays valid after the call | Pass a reference, not the value |
| Mutable borrowing | Modify a value without taking ownership | `&mut s` — s must be `mut` | Only one mutable reference at a time |
| Dangling reference | Reference to freed memory | Returning `&local_var` | Rust refuses to compile it |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| A function to read a value | Pass `&value` | Borrow read-only — original stays valid |
| A function to modify a value | Pass `&mut value` | Borrow with write permission |
| A function to permanently take a value | Pass `value` by move | Ownership transfers to the function |
| Both the original and a copy of a heap value | `.clone()` | Makes a full deep copy — both are valid |
| A type that copies automatically | Stack types: `i32`, `bool`, `char`, `f64` | These implement Copy — no move needed |
| To avoid a dangling reference | Return the value, not a reference to it | Ownership transfers to the caller |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| Why can't I use `s1` after `let s2 = s1`? | `String` is a heap type. Assignment moves ownership to `s2`. `s1` is gone. | Use `&s1` to borrow, or `s1.clone()` to keep both |
| Can I have two mutable references? | No. Only one mutable reference allowed at a time. | This prevents data races |
| Can I have a mutable and immutable reference at once? | No. They cannot exist at the same time. | Rust enforces this at compile time |
| What is the difference between `String` and `&str`? | `String` is owned heap data. `&str` is a borrowed reference to text. | Use `String` for fields; `&str` for function parameters that just read |
| Why can't I return a reference to a local variable? | The variable is dropped when the function ends. The reference would point to freed memory. | Return the value itself instead |
| Why do integers not move? | `i32`, `bool`, and similar types implement the `Copy` trait — they are cheap to duplicate. | Heap types like `String` do not — copying them is expensive |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Using a moved variable | Error: value borrowed here after move | Pass `&value` or `.clone()` instead |
| Forgetting `mut` on the variable | Cannot borrow as mutable | Both the variable and the `&mut` parameter need `mut` |
| Two mutable references at once | Compiler error — borrow checker rejects it | One `&mut` at a time |
| Trying to return a reference to a local | Dangling reference — compiler error | Return the value, not a reference |
| Using `.clone()` everywhere | Works but wastes memory — code becomes slow | Prefer `&` when you only need to read |

---

## 8. Compiler Errors As Clues

| Error | What it means | What to check |
|---|---|---|
| `value borrowed here after moved` | A heap value was moved and then used | Pass `&value` or use `.clone()` |
| `cannot borrow as mutable` | The variable is not declared `mut` | Add `mut` to the `let` declaration |
| `cannot borrow as mutable, as it is also borrowed as immutable` | A `&` and `&mut` borrow exist at the same time | End the immutable borrow before creating the mutable one |
| `cannot have two mutable references` | Two `&mut` borrows exist at once | Only one mutable reference is allowed at a time |
| `returns a reference to data owned by the current function` | You tried to return a dangling reference | Return the value itself, not a reference to it |

---

## 9. Practice

### Practice 1 — Spot The Move

**Prompt:** What is wrong with this code?

```rust
let s1 = String::from("hello");
let s2 = s1;
println!("{}", s1);
```

**Check:** `s1` was moved into `s2`. After a move, the original is invalid. Fix: use `&s1` or `s1.clone()`.

### Practice 2 — Add A Borrow

**Prompt:** Rewrite this function so `s` stays valid after the call.

```rust
fn print_name(name: String) {
    println!("{}", name);
}

let s = String::from("Alice");
print_name(s);
println!("{}", s);  // error: s was moved
```

**Check:**

```rust
fn print_name(name: &String) {
    println!("{}", name);
}

let s = String::from("Alice");
print_name(&s);
println!("{}", s);  // s is still valid
```

### Practice 3 — Mutable Borrow

**Prompt:** Fix the code so `add_world` can modify `s`.

```rust
fn add_world(s: &mut String) {
    s.push_str(", world");
}

let s = String::from("hello");
add_world(&mut s);
```

**Check:** `s` must be declared `let mut s` for `&mut s` to work.

```rust
let mut s = String::from("hello");
add_world(&mut s);
println!("{}", s);  // hello, world
```

### Practice 4 — Dangling Reference

**Prompt:** Why does this not compile?

```rust
fn get_name() -> &String {
    let name = String::from("Alice");
    &name
}
```

**Check:** `name` is dropped when `get_name` ends. The reference would point to freed memory. Fix: return the `String` itself.

```rust
fn get_name() -> String {
    String::from("Alice")
}
```

---

## 10. Tiny Drills

Short repetitions. These should take 1-3 minutes each.

- Say what happens: `let s1 = String::from("hello"); let s2 = s1;`
- Say why this is different: `let x = 5; let y = x;`
- Name the two borrowing rules
- Say when you would use `.clone()`
- Explain a dangling reference in one sentence
- Say what `&` gives a function vs what `&mut` gives it

---

## 11. Active Recall

Answer without looking:

1. What is the difference between the stack and the heap?
2. What is the ownership rule?
3. What happens when a heap value is moved?
4. What types are copied instead of moved?
5. What does `.clone()` do and when should you use it?
6. What does `&` do? What does `&mut` do?
7. What are the two rules for borrowing?
8. What is a dangling reference and how does Rust prevent it?
9. Why can you have many `&` borrows but only one `&mut` borrow?
10. What do you return instead of a reference to a local variable?

---

## 12. Relevance To Solana & Blockchain

| Lesson idea | Where it appears in Solana/blockchain | Why it matters |
|---|---|---|
| `&mut` | Accounts passed as writable in Anchor instructions | Only one writer per account at a time |
| Single mutable reference rule | Parallel transaction safety | Two instructions cannot accidentally corrupt the same account |
| Ownership rule | Account lifecycle | Clear responsibility for who controls each account |
| No garbage collector | Transaction speed | Solana's high throughput requires no GC pauses |
| Dangling reference prevention | Program safety | Programs cannot hold stale references to freed account data |

**Memory hook:** Passing a Solana account as writable is a `&mut` borrow — Rust's rules enforce it at the program level.

---

## 13. Relevance To AI Agents

| Lesson idea | Where it appears in AI agents | Why it matters |
|---|---|---|
| Ownership | Agent memory ownership | Clear rules about who controls each piece of agent state |
| Borrowing | Passing state between agent components | Agents can share data without transferring control |
| `&mut` | Updating agent memory | Changes to state are intentional and controlled |
| Move | Consuming a task or message | Once an agent processes a message, it is done |
| Dangling reference | Stale task references | Agents should not hold references to completed or deleted tasks |

**Memory hook:** An agent's state is owned data — borrow it to inspect, mutate it to update, move it to transfer.

---

## 14. Transcript Coverage Check

| Transcript topic | Covered? | Where |
|---|---|---|
| Stack vs heap (restaurant analogy) | Yes | Recap, terms, key concepts |
| Ownership rule | Yes | Terms, key concepts, active recall |
| Move (heap) | Yes | Terms, practice 1 |
| Copy (stack) | Yes | Terms, practice 1 |
| Clone | Yes | Terms, how to choose |
| Borrowing with `&` | Yes | Terms, practice 2 |
| Mutable borrowing with `&mut` | Yes | Terms, practice 3 |
| Two borrowing rules | Yes | Confusions, drills, active recall |
| Dangling reference | Yes | Terms, practice 4, errors |
| `String` vs `&str` | Yes | Confusions |
| Box question from Q&A | Not covered — Box is an advanced topic |

---

## 15. Final Study Checklist

- [ ] I know the ownership rule in one sentence
- [ ] I know the difference between move and copy
- [ ] I know what `.clone()` does and when to use it
- [ ] I know the difference between `&` and `&mut`
- [ ] I know the two borrowing rules
- [ ] I know what a dangling reference is and why Rust prevents it
- [ ] I completed all four practice exercises
- [ ] I can explain the Solana/blockchain relevance
- [ ] I can explain the AI agent relevance

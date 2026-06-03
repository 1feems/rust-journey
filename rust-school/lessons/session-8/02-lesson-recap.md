# Lesson Recap #8: Traits and Abstractions

Traits are Rust's way of saying "any type that wants to be called an Animal must be able to do these things" — and then letting different types (Cat, Dog) each decide how to do them.

---

## Key Takeaways

- A trait is a contract — it lists functions that a type must implement, without saying how.
- You implement a trait on a struct using `impl TraitName for StructName`.
- `impl Trait` in a function signature means "any type that implements this trait" — Rust checks this at compile time (static dispatch).
- `Box<dyn Trait>` means Rust checks the type at runtime (dynamic dispatch) — needed when you don't know the type until the program is running.
- You need `Box` with `dyn` because dynamic types don't have a known size at compile time — Box puts them in the heap where size can be flexible.
- In Solana, traits are everywhere — Anchor programs use trait-like patterns to define what accounts and instructions must look like.

---

## Why This Matters

Traits let different types share behavior without copying code. Without traits, if you had a Dog and a Cat and wanted them both to speak, you'd have to write separate functions for each one. With traits, you write one function that works for any type that implements the Animal trait. This is how real Rust programs handle shared behavior across many different types — and it's exactly how Solana's Anchor framework is built under the hood.

---

## Important Rules

- A trait only defines the function signatures — not the function bodies (unless you write a default implementation).
- To use a trait in a function, the parameter type must say `impl TraitName` or `Box<dyn TraitName>`.
- You cannot store two different types in the same Rust vector — unless you use `Box<dyn Trait>`.
- `dyn Trait` cannot be used without a smart pointer like `Box`.
- `impl Trait` is faster (compile time) but makes the binary slightly larger. `Box<dyn Trait>` is slightly slower (runtime) but keeps the binary smaller.

---

## Key Terms

| Term | Meaning |
|---|---|
| Trait | A contract that defines what functions a type must have |
| `impl TraitName for StructName` | How you make a struct follow a trait's contract |
| Static dispatch | Rust resolves the type at compile time — uses `impl Trait` — fast |
| Dynamic dispatch | Rust resolves the type at runtime — uses `Box<dyn Trait>` — flexible |
| Box | A smart pointer that stores a value in the heap |
| Stack | Fast memory — size must be known at compile time |
| Heap | Flexible memory — size can be unknown or grow at runtime |
| V-table | A table in the heap that stores function pointers for dynamic dispatch |

---

## Key Concepts

**Defining a trait:**
A trait lists the functions a type must have — like a job description that doesn't explain how to do the work, just what work needs to get done.

```rust
pub trait Animal {
    fn name(&self) -> &str;
    fn speak(&self);
}
```

---

**Implementing a trait on a struct:**
Each struct decides how to fulfill the trait's contract in its own way.

```rust
struct Cat {
    name: String,
}

impl Animal for Cat {
    fn name(&self) -> &str {
        &self.name
    }
    fn speak(&self) {
        println!("Meow!");
    }
}

struct Dog {
    name: String,
}

impl Animal for Dog {
    fn name(&self) -> &str {
        &self.name
    }
    fn speak(&self) {
        println!("Woof!");
    }
}
```

---

**Static dispatch — `impl Trait`:**
Rust knows the type at compile time. Function pointers live on the stack. Fast.

```rust
fn speak(animal: &impl Animal) {
    println!("{}", animal.name());
    animal.speak();
}

let cat = Cat { name: String::from("Amy") };
speak(&cat); // works
```

---

**Dynamic dispatch — `Box<dyn Trait>`:**
Rust figures out the type at runtime. Function pointers live in the heap (V-table). Needed when you don't know the type up front.

```rust
fn jump(animal: &Box<dyn Animal>) {
    animal.speak();
}
```

---

**Storing multiple types in a vector:**
Rust vectors can only hold one type — but `Box<dyn Trait>` lets you store different types that all implement the same trait.

```rust
let animals: Vec<Box<dyn Animal>> = vec![
    Box::new(Cat { name: String::from("Amy") }),
    Box::new(Dog { name: String::from("Rex") }),
];

for animal in &animals {
    animal.speak();
}
```

---

## Step-by-Step Walkthrough

Tracing what happens when you call `speak(&cat)`:

```rust
let cat = Cat { name: String::from("Amy") };
speak(&cat);
```

1. Rust sees `speak(&cat)` and checks: does Cat implement Animal? Yes.
2. The `speak` function receives `&cat` as `animal: &impl Animal`.
3. Inside the function, `animal.name()` calls Cat's implementation — returns `"Amy"`.
4. `animal.speak()` calls Cat's implementation — prints `"Meow!"`.
5. The function pointer for `speak` was resolved at compile time — it's already in the stack.

Final output:
```
Amy
Meow!
```

---

## Common Confusions

* "What's the difference between a trait and a struct?"
  A struct is a type that holds data. A trait is a contract that says what functions a type must have. They work together — you define data in the struct, and behavior in the trait.

* "Why do I need `Box` with `dyn`?"
  `dyn Trait` means the type is figured out at runtime, so Rust doesn't know its size at compile time. Box puts it in the heap where size doesn't have to be fixed — that's what makes it possible.

* "Why is `impl Trait` faster than `Box<dyn Trait>`?"
  With `impl Trait`, the function pointer lives on the stack — one step to access it. With `Box<dyn Trait>`, Rust goes to the heap, finds the V-table, then finds the function pointer — two steps. Stack access is faster than heap access.

* "Can I use `dyn Trait` without Box?"
  No. Rust requires a smart pointer (like Box) with `dyn` because the size of the type isn't known at compile time.

---

## Common Mistakes

- Forgetting `&self` in a trait function signature — Rust will complain the function doesn't match the trait.
- Using `dyn Trait` without `Box` — compiler will refuse it.
- Trying to store Cat and Dog directly in the same `Vec<Cat>` — won't compile. Use `Vec<Box<dyn Animal>>` instead.
- Not making trait functions `pub` when they need to be accessed from outside the module.

---

## Source

- Recording: check Telegram group for session resources
- Transcript: `pipeline/transcripts/rust-school/cleaned/lesson-8-traits-and-abstractions.txt`
- Course: Rust School
- Session: 8

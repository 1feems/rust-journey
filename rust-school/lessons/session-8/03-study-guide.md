# Study Guide — Rust School Session 8: Traits and Abstractions

> Use this after watching the class recording and reading the lesson recap.
> Traits are how Solana programs define shared behavior across different account types and instructions — understanding them now is foundational.

---

## 1. Lesson Recap

- A trait is a contract — it says what functions a type must have
- You implement a trait with `impl TraitName for StructName`
- `impl Trait` in a function = static dispatch — type known at compile time, fast
- `Box<dyn Trait>` in a function = dynamic dispatch — type known at runtime, flexible
- You need `Box` with `dyn` because heap memory is needed for unknown-size types
- You can store multiple types in one vector using `Vec<Box<dyn Trait>>`

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| Trait | A contract listing what functions a type must have | When you want different types to share behavior | Lets you write one function that works for many types |
| `impl TraitName for StructName` | How you make a struct follow a trait | When you define a new type that should behave like the trait | Required — the trait does nothing until a struct implements it |
| Static dispatch | Type is resolved at compile time | `fn speak(animal: &impl Animal)` | Fastest option — function pointer is on the stack |
| Dynamic dispatch | Type is resolved at runtime | `fn jump(animal: &Box<dyn Animal>)` | Needed when type is unknown until the program runs |
| Box | Smart pointer that stores a value in the heap | `Box::new(Cat { ... })` | Required for dynamic dispatch — gives heap allocation |
| V-table | Lookup table in the heap for dynamic function pointers | Behind the scenes with `dyn Trait` | How Rust finds the right function at runtime |

---

## 3. Big Idea

A trait is a shared job description. Different types (Dog, Cat) each decide how to do the job — but anything that holds a reference to the trait can call those functions without caring which specific type it's dealing with.

**Memory hook:** Trait = contract. impl = signing the contract. Box<dyn> = "figure it out at runtime."

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| Define a trait | Use `trait` keyword + list function signatures | `pub trait Animal { fn speak(&self); }` | No function bodies in the trait — just signatures |
| Implement a trait | `impl TraitName for StructName` + write the function bodies | `impl Animal for Cat { fn speak(&self) { ... } }` | Every function in the trait must be implemented |
| Static dispatch | `impl Trait` — Rust locks in the type at compile time | `fn speak(a: &impl Animal)` | One type per call, resolved before the program runs |
| Dynamic dispatch | `Box<dyn Trait>` — Rust checks the type when the program runs | `fn jump(a: &Box<dyn Animal>)` | Multiple possible types, resolved at runtime |
| Mixed-type vector | `Vec<Box<dyn Trait>>` — store different types that share a trait | `vec![Box::new(Cat{...}), Box::new(Dog{...})]` | Only way to hold multiple types in one Rust collection |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| One type, known at compile time | `impl Trait` | Fastest — function pointer on the stack |
| Unknown type at runtime | `Box<dyn Trait>` | Heap allocation lets the type be resolved later |
| Multiple types in one vector | `Vec<Box<dyn Trait>>` | Vectors need one type — Box<dyn> makes them compatible |
| Smaller binary size | `Box<dyn Trait>` | Dynamic dispatch keeps type info out of the binary |
| Maximum speed | `impl Trait` | No V-table lookup — direct stack access |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| "Trait vs struct — what's the difference?" | Struct = data. Trait = behavior. They work together. | `struct Cat { name: String }` + `impl Animal for Cat { ... }` |
| "Why does `dyn` need Box?" | `dyn Trait` is unknown size — Box puts it in the heap where size is flexible | `Box<dyn Animal>` works. `dyn Animal` alone won't compile. |
| "impl Trait vs Box<dyn Trait>?" | `impl` is compile-time (fast). `Box<dyn>` is runtime (flexible). | Use `impl` by default. Use `Box<dyn>` when you need mixed types or runtime flexibility. |
| "Can I put Cat and Dog in the same Vec?" | Not directly — but yes with `Vec<Box<dyn Animal>>` | `vec![Box::new(cat), Box::new(dog)]` compiles fine |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Forgetting `&self` in a trait function | Compiler error: function doesn't match trait signature | Always include `&self` when the function needs access to the struct's data |
| Using `dyn Trait` without Box | Compiler error: trait object must include `Box` or other pointer | Always write `Box<dyn Trait>`, never just `dyn Trait` |
| Storing two types directly in one Vec | Compiler error: mismatched types | Use `Vec<Box<dyn TraitName>>` to hold mixed types |
| Not implementing all trait functions | Compiler error: missing required method | Check the trait definition — every function listed must be implemented |

---

## 8. Compiler Errors As Clues

| Error | What it means | What to check |
|---|---|---|
| `the trait bound ... is not satisfied` | The type you passed doesn't implement the required trait | Check that you wrote `impl TraitName for YourType` |
| `doesn't have a size known at compile-time` | You're using `dyn Trait` without Box | Wrap it: `Box<dyn Trait>` |
| `not all trait items implemented` | You forgot to write one of the trait's functions | Check the trait definition and add the missing function |
| `mismatched types` in a Vec | You're trying to store two different types without using Box<dyn Trait> | Change the Vec type to `Vec<Box<dyn TraitName>>` |

---

## 9. Practice

### Practice 1 — Define and implement a trait

**Prompt:** Define a trait called `Shape` with one function: `area(&self) -> f64`. Then create two structs — `Circle` (with a `radius: f64` field) and `Rectangle` (with `width: f64` and `height: f64` fields) — and implement `Shape` for both. Print the area of each.

**Check:**

```rust
pub trait Shape {
    fn area(&self) -> f64;
}

struct Circle {
    radius: f64,
}

struct Rectangle {
    width: f64,
    height: f64,
}

impl Shape for Circle {
    fn area(&self) -> f64 {
        3.14159 * self.radius * self.radius
    }
}

impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
}

fn print_area(shape: &impl Shape) {
    println!("Area: {}", shape.area());
}

fn main() {
    let c = Circle { radius: 5.0 };
    let r = Rectangle { width: 4.0, height: 6.0 };
    print_area(&c);
    print_area(&r);
}
```

---

### Practice 2 — Use a trait in a function

**Prompt:** Write a function called `describe` that takes `&impl Shape` and prints the area. Call it with both a Circle and a Rectangle.

**Check:**

```rust
fn describe(shape: &impl Shape) {
    println!("This shape has an area of {:.2}", shape.area());
}
```

---

### Practice 3 — Mixed-type vector

**Prompt:** Create a `Vec<Box<dyn Shape>>` that holds both a Circle and a Rectangle. Loop over it and print the area of each.

**Check:**

```rust
let shapes: Vec<Box<dyn Shape>> = vec![
    Box::new(Circle { radius: 3.0 }),
    Box::new(Rectangle { width: 2.0, height: 5.0 }),
];

for shape in &shapes {
    println!("Area: {:.2}", shape.area());
}
```

---

### Practice 4 — Stretch: dynamic dispatch function

**Prompt:** Write a function called `total_area` that takes a `Vec<Box<dyn Shape>>` and returns the sum of all areas.

**Check:**

```rust
fn total_area(shapes: &Vec<Box<dyn Shape>>) -> f64 {
    shapes.iter().map(|s| s.area()).sum()
}
```

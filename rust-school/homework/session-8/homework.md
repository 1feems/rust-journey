# Exercise / Homework Guide — Animal Kingdom

## Lesson Connection
Session 8 — Traits and Abstractions

---

# Homework Assigned

- Define a trait called `Animal` with two functions: `name` and `speak`
- Create two structs: `Cat` and `Dog`, each with a `name` field
- Implement the `Animal` trait for both `Cat` and `Dog`
- Write a function that takes `&impl Animal` and calls both trait functions
- Create a `Vec<Box<dyn Animal>>` that holds both a Cat and a Dog, and loop over it calling `speak` on each

---

# Homework Goal

Put Session 8's tools together into one small program that models a group of animals using traits. By the end you should be able to:

- Define a trait and know what it's for
- Implement a trait on multiple structs
- Use `impl Trait` in a function signature
- Use `Box<dyn Trait>` to store different types in one vector

---

# Final Expected Outcome

By the end of this homework, you should have:

- A working `Animal` trait with `name` and `speak` functions
- `Cat` and `Dog` structs that each implement `Animal` differently
- A function that accepts any Animal and calls its trait functions
- A vector that holds both Cat and Dog and loops over them

---

# Concepts Practiced

- Defining traits
- Implementing traits on structs
- Static dispatch (`impl Trait`)
- Dynamic dispatch (`Box<dyn Trait>`)
- Storing mixed types in a `Vec`

---

# Step-by-Step Homework Breakdown

## Step 1 — Define the Animal trait

### What You Need To Do

Write a trait called `Animal`. It should have two functions:
- `name(&self) -> &str` — returns the animal's name
- `speak(&self)` — prints a sound

### Why This Step Matters

This is the contract. Every struct that wants to be an Animal must agree to have both of these functions.

### Suggested Starter Code

```rust
pub trait Animal {
    fn name(&self) -> &str;
    fn speak(&self);
}
```

### Expected Result

No output yet — this just sets up the contract.

---

## Step 2 — Create Cat and Dog structs and implement the trait

### What You Need To Do

Create two structs: `Cat` and `Dog`. Each has a `name: String` field. Implement `Animal` for both — Cat says "Meow!" and Dog says "Woof!".

### Why This Step Matters

This is where the types fulfill the contract. Each one decides how to do the job in its own way.

### Suggested Starter Code

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

### Expected Result

Still no output — but Cat and Dog now implement Animal.

---

## Step 3 — Write a function using static dispatch

### What You Need To Do

Write a function called `greet` that takes `animal: &impl Animal`. Inside, print the animal's name and then call speak.

### Why This Step Matters

This is static dispatch — Rust knows which type it is at compile time. It's the default, fast way to use traits in functions.

### Suggested Starter Code

```rust
fn greet(animal: &impl Animal) {
    println!("Hello, {}!", animal.name());
    animal.speak();
}
```

### Expected Result

```
Hello, Amy!
Meow!
```

---

## Step 4 — Store mixed types in a vector using dynamic dispatch

### What You Need To Do

In `main`, create a `Vec<Box<dyn Animal>>` with at least one Cat and one Dog. Loop over the vector and call `speak` on each.

### Why This Step Matters

This is the key reason traits exist — you can hold different types together and call the same function on all of them.

### Suggested Starter Code

```rust
fn main() {
    let cat = Cat { name: String::from("Amy") };
    let dog = Dog { name: String::from("Rex") };

    greet(&cat);
    greet(&dog);

    let animals: Vec<Box<dyn Animal>> = vec![
        Box::new(Cat { name: String::from("Luna") }),
        Box::new(Dog { name: String::from("Bruno") }),
    ];

    for animal in &animals {
        animal.speak();
    }
}
```

### Expected Result

```
Hello, Amy!
Meow!
Hello, Rex!
Woof!
Meow!
Woof!
```

---

# Common Mistakes

**Forgetting `&self` in the trait function:**
```rust
// ❌ Wrong
fn speak();

// ✅ Correct
fn speak(&self);
```

**Using `dyn Animal` without Box:**
```rust
// ❌ Wrong — won't compile
let animals: Vec<dyn Animal> = vec![];

// ✅ Correct
let animals: Vec<Box<dyn Animal>> = vec![];
```

**Storing Cat and Dog directly in the same Vec:**
```rust
// ❌ Wrong — mismatched types
let animals = vec![cat, dog];

// ✅ Correct
let animals: Vec<Box<dyn Animal>> = vec![Box::new(cat), Box::new(dog)];
```

---

# Debugging Hints

- `the trait bound ... is not satisfied` — you forgot to write `impl Animal for YourType`
- `doesn't have a size known at compile-time` — you wrote `dyn Animal` without Box, wrap it in `Box<dyn Animal>`
- `not all trait items implemented` — you missed one of the trait functions, check the trait definition
- `mismatched types` in a Vec — all items must be `Box<dyn Animal>`, not raw Cat or Dog

---

# Stretch Goals

- Add a third animal type (e.g. `Bird`) and add it to the vector
- Add a third function to the trait: `legs(&self) -> u8` — returns how many legs the animal has. Implement it for all three types.
- Write a `total_legs` function that takes a `Vec<Box<dyn Animal>>` and returns the sum of all legs

---

# Completion Checklist

- [ ] I defined the `Animal` trait with `name` and `speak`
- [ ] I implemented `Animal` for both Cat and Dog
- [ ] I wrote a `greet` function using `&impl Animal`
- [ ] I created a `Vec<Box<dyn Animal>>` and looped over it
- [ ] I understand why `Box` is needed with `dyn`
- [ ] I understand the difference between `impl Trait` and `Box<dyn Trait>`
- [ ] My code compiles and runs without errors

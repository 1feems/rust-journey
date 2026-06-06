---
title: "Session 5 — Homework"
description: "Build the Block Builder — a blockchain-style data structure using structs, impl blocks, and Vec."
---

# Exercise / Homework Guide — Block Builder

## Lesson Connection
Session 5 — Structs, impl Blocks & State Design

---

# Homework Assigned

- Create a `Transaction` struct with fields for sender, receiver, and amount
- Create a `Block` struct with a block ID and a list of transactions
- Store multiple transactions inside a block using `Vec<Transaction>`
- Create an associated function to initialise a new block
- Create methods to add transactions to a block
- Create methods to print or display block details
- Practice ownership when moving transaction data into a block
- Experiment with nested structs and vectors

---

# Homework Goal

Put Session 5's tools together into one small program that models something real. By the end you should be able to:

- Define multiple structs and understand how they relate to each other
- Store a `Vec` of structs inside another struct
- Write associated functions and methods on each struct
- Move ownership of data from one struct into another
- Read and print structured data using `#[derive(Debug)]`

---

# Final Expected Outcome

By the end of this homework, the student should have:

- A `Transaction` struct with at least three fields
- A `Block` struct that contains a `Vec<Transaction>`
- An associated function `Block::new(id: u32) -> Block` that initialises an empty block
- A method `add_transaction(&mut self, tx: Transaction)` that appends to the block's list
- A method that prints the block's ID and all its transactions
- A working `main` that creates a block, adds transactions, and prints the result

---

# Concepts Practiced

- Struct definition and instantiation
- `Vec<T>` inside a struct
- Nested structs
- `impl` blocks with associated functions and methods
- `&self` and `&mut self`
- Ownership when moving structs into a `Vec`
- `#[derive(Debug)]`

---

# What You Are Building

A simplified blockchain block. A block has an ID and contains a list of transactions. Each transaction records who sent funds, who received them, and how much. You build up a block by pushing transactions into it, then print the full block to verify everything is correct.

This is the same pattern Solana uses to group instructions into a transaction: structured data, owned by a parent container, built up incrementally and then processed.

---

# Step-by-Step Homework Breakdown

## Step 1 — Define the Transaction Struct

### What You Need To Do

Define a `Transaction` struct with three fields: `sender` (`String`), `receiver` (`String`), and `amount` (`u64`). Add `#[derive(Debug)]` so it can be printed.

### Why This Step Matters

This is the smallest unit of data in your program. Getting the struct definition right — owned `String` fields, correct types — is the foundation everything else builds on.

### Suggested Starter Code

```rust
#[derive(Debug)]
struct Transaction {
    sender: String,
    receiver: String,
    amount: u64,
}
```

### Expected Result

The code compiles. You can create a transaction:
```rust
let tx = Transaction {
    sender: String::from("Alice"),
    receiver: String::from("Bob"),
    amount: 1000,
};
println!("{:?}", tx);
```
```
Transaction { sender: "Alice", receiver: "Bob", amount: 1000 }
```

---

## Step 2 — Define the Block Struct

### What You Need To Do

Define a `Block` struct with two fields: `id` (`u32`) and `transactions` (`Vec<Transaction>`). Add `#[derive(Debug)]`.

### Why This Step Matters

A `Vec<Transaction>` inside a struct is how you model a one-to-many relationship in Rust. The `Block` owns its transactions — when the `Block` is dropped, all the transactions inside it are dropped too.

### Suggested Starter Code

```rust
#[derive(Debug)]
struct Block {
    id: u32,
    transactions: Vec<Transaction>,
}
```

### Expected Result

The code compiles. You have two structs — one nested inside the other via `Vec`.

---

## Step 3 — Add an Associated Function and Methods

### What You Need To Do

Write an `impl Block` block with:
- An associated function `new(id: u32) -> Block` that creates an empty block
- A method `add_transaction(&mut self, tx: Transaction)` that pushes a transaction into the block
- A method `print_block(&self)` that prints the block ID and each transaction

### Why This Step Matters

`Block::new()` is the constructor pattern — it gives you a clean, controlled way to initialise state. `add_transaction` uses `&mut self` because it changes the block's data. `print_block` uses `&self` because it only reads. This distinction is enforced by the compiler.

### Suggested Starter Code

```rust
impl Block {
    fn new(id: u32) -> Block {
        Block {
            id,
            transactions: Vec::new(),
        }
    }

    fn add_transaction(&mut self, tx: Transaction) {
        self.transactions.push(tx);
    }

    fn print_block(&self) {
        println!("Block #{}", self.id);
        for tx in &self.transactions {
            println!(
                "  {} → {} : {} lamports",
                tx.sender, tx.receiver, tx.amount
            );
        }
    }
}
```

### Expected Result

You can call `Block::new(1)` to create a block. `add_transaction` and `print_block` compile and work.

---

## Step 4 — Wire It Together in `main`

### What You Need To Do

In `main`, create a block using `Block::new`, create two or three transactions, add them to the block with `add_transaction`, and call `print_block` to verify.

### Why This Step Matters

This step tests that ownership works correctly end to end. When you call `block.add_transaction(tx)`, `tx` is moved into the block's `Vec`. After that call, `tx` no longer exists as a standalone variable — the block owns it.

### Suggested Starter Code

```rust
fn main() {
    let mut block = Block::new(1);

    block.add_transaction(Transaction {
        sender: String::from("Alice"),
        receiver: String::from("Bob"),
        amount: 1000,
    });

    block.add_transaction(Transaction {
        sender: String::from("Bob"),
        receiver: String::from("Carol"),
        amount: 500,
    });

    block.add_transaction(Transaction {
        sender: String::from("Carol"),
        receiver: String::from("Alice"),
        amount: 250,
    });

    block.print_block();
}
```

### Expected Result

```
Block #1
  Alice → Bob : 1000 lamports
  Bob → Carol : 500 lamports
  Carol → Alice : 250 lamports
```

---

# Common Mistakes

**Forgetting `mut` on the block variable:**
```rust
let block = Block::new(1);
block.add_transaction(tx);  // ❌ cannot borrow `block` as mutable
let mut block = Block::new(1);
block.add_transaction(tx);  // ✅
```

**Trying to use a transaction after moving it into the block:**
```rust
let tx = Transaction { sender: String::from("Alice"), ... };
block.add_transaction(tx);
println!("{:?}", tx);  // ❌ value moved — tx now belongs to the block
```

**Using `&str` for struct fields and getting lifetime errors:**
```rust
struct Transaction { sender: &str }   // ❌ missing lifetime specifier
struct Transaction { sender: String } // ✅
```

**Iterating the transactions Vec without borrowing:**
```rust
for tx in self.transactions {     // ❌ moves out of self.transactions
for tx in &self.transactions {    // ✅ borrows each element
```

---

# Debugging Hints

- "cannot move out of `self.transactions`" when iterating → use `&self.transactions` in the `for` loop
- "cannot borrow as mutable" on `add_transaction` → check that the block variable is declared `let mut`
- "use of moved value" after passing a transaction → the `Vec::push` took ownership; create a new transaction or clone it
- "doesn't implement `Debug`" → add `#[derive(Debug)]` above the struct definition

---

# How This Connects to Solana

- **A Solana transaction is a list of instructions** — the same structure as your `Block` holding a `Vec<Transaction>`. Instructions are grouped, processed together, and either all succeed or all fail.
- **Account structs in Anchor** — `#[account] pub struct Block { ... }` is exactly this pattern. Fields are stored on-chain. Methods in `impl Block` define what the program can do with that account's data.
- **Ownership when writing to accounts** — in a Solana instruction, you pass account data as `&mut AccountData`. The borrow checker ensures you cannot accidentally use account data after it has been consumed or moved.
- **`Vec` inside on-chain accounts** — Solana accounts have a fixed size at creation. Storing a `Vec` on-chain requires pre-allocating space. Understanding how `Vec` grows in memory in this homework prepares you to reason about account sizing in Anchor.

---

# Stretch Goals

- Add a `block_id` field to each `Transaction` that records which block it belongs to
- Add a second `impl Block` block with a method `total_volume(&self) -> u64` that sums all transaction amounts
- Add a `Blockchain` struct that holds `Vec<Block>` and an `add_block` method
- Try `#[derive(Debug, Clone)]` on `Transaction` and explore what `clone()` does to ownership

---

# Completion Checklist

- [ ] `Transaction` struct compiles with `#[derive(Debug)]`
- [ ] `Block` struct contains `Vec<Transaction>` and compiles
- [ ] `Block::new()` creates an empty block correctly
- [ ] `add_transaction` pushes to the Vec — block must be `mut`
- [ ] `print_block` prints all transactions without consuming them
- [ ] `main` creates a block, adds transactions, and prints correct output
- [ ] I understand why `add_transaction` takes `&mut self`
- [ ] I understand what happens to ownership when a transaction is pushed into the Vec

---

# Reflection Questions

- What happened to a transaction variable after you passed it to `add_transaction`? Why?
- Why does `print_block` use `&self` instead of `&mut self`?
- Why does iterating `self.transactions` require `&self.transactions`?
- How does this `Block` + `Vec<Transaction>` pattern relate to how a Solana transaction groups instructions?

---

# Personal Notes

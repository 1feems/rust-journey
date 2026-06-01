---
title: "Session 4 — Homework"
description: "Build the Ledger — a small account manager using HashMap, enums, Vec, and Result."
---

# Session 4 — Homework

**Lesson Connection:** Session 4 — Collections, Enums & Error Handling

---

## Homework Assigned

Build the **Ledger / Account Manager.**

- Use a `HashMap` to track account names and balances
- Create an enum for transaction types
- Use a `Vec` to store transaction history
- Use `Result` to represent success or failure

---

## Homework Goal

Put Session 4's tools together into one small program that models something real. By the end you should be able to:
- Create and query a `HashMap`
- Define an enum and use `match` on it
- Store structured data in a `Vec`
- Return and handle `Result` from a function

---

## Final Expected Outcome

A Rust program that:
- Stores at least two accounts in a `HashMap`
- Defines a `TransactionType` enum with Deposit, Withdrawal, and Transfer variants
- Adds transactions to a `Vec`
- Has a function that returns `Result` for a withdrawal (fail if balance too low)
- Prints a readable summary

---

## Concepts Practiced

- `HashMap` — insert, get, entry
- Enums with `match`
- `Vec` — push and iterate
- `Option` — handle missing accounts
- `Result` — model success and failure
- Structs — group related data

---

## What You Are Building

A command-line ledger. You create accounts, record transactions, and print balances. The program should handle: account not found, insufficient balance, and successful transfers — all using Rust's type system rather than if/else chains and print statements.

---

## Step-by-Step Homework Breakdown

### Step 1 — Set Up the HashMap

**What You Need To Do**

Create a mutable `HashMap<String, u64>` called `accounts`. Insert at least two accounts with starting balances. Print each balance.

**Why This Step Matters**

This is the data layer. Everything else builds on it.

**Suggested Starter Code**

```rust
use std::collections::HashMap;

fn main() {
    let mut accounts: HashMap<String, u64> = HashMap::new();
    accounts.insert(String::from("Alice"), 5000);
    accounts.insert(String::from("Bob"), 3000);

    for (name, balance) in &accounts {
        println!("{}: {} lamports", name, balance);
    }
}
```

**Expected Result**

```
Alice: 5000 lamports
Bob: 3000 lamports
```

---

### Step 2 — Create the Transaction Enum

**What You Need To Do**

Define a `TransactionType` enum with three variants: `Deposit`, `Withdrawal`, and `Transfer`. Write a `match` that prints what each type means.

**Why This Step Matters**

Enums make the state explicit — instead of a string like `"deposit"` that could be mistyped, you have a type-checked variant that the compiler knows about.

**Suggested Starter Code**

```rust
#[derive(Debug)]
enum TransactionType {
    Deposit,
    Withdrawal,
    Transfer,
}

fn describe(tx: &TransactionType) {
    match tx {
        TransactionType::Deposit    => println!("Funds added to account"),
        TransactionType::Withdrawal => println!("Funds removed from account"),
        TransactionType::Transfer   => println!("Funds moved between accounts"),
    }
}

fn main() {
    describe(&TransactionType::Deposit);
    describe(&TransactionType::Withdrawal);
    describe(&TransactionType::Transfer);
}
```

**Expected Result**

```
Funds added to account
Funds removed from account
Funds moved between accounts
```

---

### Step 3 — Add a Transaction Struct and History Vec

**What You Need To Do**

Define a `Transaction` struct with fields: `from`, `to`, `amount`, and `tx_type`. Create a `Vec<Transaction>` and add a few transactions to it. Print the history.

**Why This Step Matters**

This groups related data together and gives you a log of what happened — the same pattern used in blockchain account history.

**Suggested Starter Code**

```rust
struct Transaction {
    from:    String,
    to:      String,
    amount:  u64,
    tx_type: TransactionType,
}

fn main() {
    let mut history: Vec<Transaction> = Vec::new();

    history.push(Transaction {
        from:    String::from("Alice"),
        to:      String::from("Bob"),
        amount:  1000,
        tx_type: TransactionType::Transfer,
    });

    for tx in &history {
        println!("{:?} — {} → {} : {} lamports",
            tx.tx_type, tx.from, tx.to, tx.amount);
    }
}
```

**Expected Result**

```
Transfer — Alice → Bob : 1000 lamports
```

---

### Step 4 — Write a `withdraw` Function Using `Result`

**What You Need To Do**

Write a function `withdraw(accounts: &mut HashMap<String, u64>, name: &str, amount: u64) -> Result<u64, String>`.

- If the account doesn't exist → return `Err("account not found")`
- If the balance is too low → return `Err("insufficient balance")`
- Otherwise → subtract the amount and return `Ok(new_balance)`

**Why This Step Matters**

Every Solana instruction handler returns `Result`. This is the pattern you'll write hundreds of times.

**Suggested Starter Code**

```rust
fn withdraw(
    accounts: &mut HashMap<String, u64>,
    name: &str,
    amount: u64,
) -> Result<u64, String> {
    match accounts.get_mut(name) {
        None => Err(String::from("account not found")),
        Some(balance) => {
            if *balance < amount {
                Err(String::from("insufficient balance"))
            } else {
                *balance -= amount;
                Ok(*balance)
            }
        }
    }
}

fn main() {
    let mut accounts: HashMap<String, u64> = HashMap::new();
    accounts.insert(String::from("Alice"), 5000);

    match withdraw(&mut accounts, "Alice", 1000) {
        Ok(new_balance)  => println!("Withdrew 1000. New balance: {}", new_balance),
        Err(reason)      => println!("Failed: {}", reason),
    }

    match withdraw(&mut accounts, "Alice", 9999) {
        Ok(new_balance)  => println!("Withdrew 9999. New balance: {}", new_balance),
        Err(reason)      => println!("Failed: {}", reason),
    }
}
```

**Expected Result**

```
Withdrew 1000. New balance: 4000
Failed: insufficient balance
```

---

## Common Mistakes

**Using `v[i]` instead of `.get(i)` on a Vec:**
```rust
let tx = history[10];  // ❌ panics if index doesn't exist
let tx = history.get(10);  // ✅ returns Option
```

**Calling `.unwrap()` on a HashMap lookup:**
```rust
let balance = accounts.get("Carol").unwrap();  // ❌ panics if Carol doesn't exist
```
Use `match` or `.unwrap_or(&0)` instead.

**Forgetting `*` when modifying a value through `get_mut`:**
```rust
Some(balance) => balance -= amount,   // ❌ can't subtract from a reference directly
Some(balance) => *balance -= amount,  // ✅ dereference first
```

**Non-exhaustive match on enum:**
```rust
match tx_type {
    TransactionType::Deposit => println!("deposit"),
    // ❌ missing Withdrawal and Transfer
}
```

---

## Debugging Hints

- "cannot find type `HashMap`" → add `use std::collections::HashMap;` at the top
- "cannot move out of borrowed content" when iterating a HashMap → iterate with `&accounts` not `accounts`
- "expected `u64`, found `&u64`" from `.get()` → `.get()` returns a reference — dereference with `*` or use `.copied()`
- "match arms have incompatible types" → make sure every arm of your `match` returns the same type

---

## How This Connects to Solana

The Ledger pattern is a simplified version of what Solana programs do:
- `HashMap` ≈ account state stored on-chain (key = address, value = data)
- `TransactionType` enum ≈ the instruction enum that routes program logic
- `Result` return from `withdraw` ≈ `Result<(), ProgramError>` from every instruction handler
- Insufficient balance check ≈ the kind of validation Solana programs do before modifying account state

---

## Stretch Goals

- Add a `deposit` function that also returns `Result`
- Add a `transfer` function that calls `withdraw` and `deposit` internally
- Use `.entry().or_insert()` to create an account with a default balance if it doesn't exist
- Add a `print_ledger` function that takes `&HashMap<String, u64>` and prints all balances

---

## Completion Checklist

- [ ] Step 1: HashMap with two accounts, balances print correctly
- [ ] Step 2: `TransactionType` enum and `match` — all three variants handled
- [ ] Step 3: `Transaction` struct, `Vec<Transaction>`, history prints correctly
- [ ] Step 4: `withdraw` returns `Result` — success and both failure cases work
- [ ] I can explain when to use `Option` vs `Result`
- [ ] I understand why `match` must cover every variant

---

## Paste Your Code Here

When you're done, paste your final solution below and ask Claude to review it.

```rust
// paste your code here
```

> Ask Claude: **"Review my Session 4 homework"**

---

## Reflection Questions

- What happened when you tried to use `.unwrap()` on an account that didn't exist?
- How did returning `Result` change how you called the `withdraw` function?
- Where in a Solana program would you use each of these: `Vec`, `HashMap`, enum, `Option`, `Result`?
- What would you add to this ledger to make it feel more like a real program?

---

## Personal Notes

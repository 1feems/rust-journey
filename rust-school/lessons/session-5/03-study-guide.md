# Study Guide — Rust School Session 5: Structs, impl Blocks & State Design

> Use this after watching the class recording and reading the synthesis.
> Structs are the direct bridge from Rust to Solana. Every Solana account is a Rust struct.

---

## 1. Lesson Recap

- A struct is a blueprint — it defines what fields a piece of data has and what type each field is. Creating an instance fills in the blanks
- Access fields with dot notation. To change any field, the entire instance must be declared `mut` — you cannot make individual fields mutable
- Field init shorthand lets you skip `field: field` repetition when a parameter name matches the field name. Struct update syntax (`..other`) copies remaining fields from an existing instance
- `impl` blocks attach methods to a struct. `&self` = read-only, `&mut self` = modify, `self` = take ownership
- Associated functions (no `self`) are called with `::` and are typically used as constructors — like `Wallet::new()`
- `#[derive(Debug)]` and `{:?}` let you print a whole struct for debugging
- Every Solana account is a Rust struct — this session is the direct bridge from Rust to Solana

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| Struct | A custom type that groups related named fields | `struct Wallet { balance: u64 }` | Models real-world things with multiple properties |
| Instance | A specific value created from a struct definition | `let wallet = Wallet { balance: 0 };` | The actual data — not just the blueprint |
| Field | A named value inside a struct with a declared type | `balance: u64` | Each field holds one piece of the struct's data |
| Dot notation | How you access or update a struct field | `wallet.balance` | The standard way to read or write a field |
| `impl` block | Where you define methods and associated functions for a struct | `impl Wallet { ... }` | Attaches behavior to data |
| Method | A function in an `impl` block that takes `self` in some form | `fn deposit(&mut self, ...)` | Called with `.` on an instance |
| `&self` | Read-only access to the instance. Does not take ownership | `fn balance(&self) -> u64` | Use when the method only needs to read |
| `&mut self` | Mutable access — lets the method change fields. Instance must be `mut` | `fn deposit(&mut self, amount: u64)` | Use when the method needs to modify the struct |
| `self` | Takes full ownership of the instance | `fn consume(self)` | Instance is gone after the call — rare |
| Associated function | A function in `impl` with no `self` parameter. Called with `::` | `Wallet::new()` | Idiomatic constructor pattern in Rust |
| `#[derive(Debug)]` | An attribute that enables debug printing of a struct | Put above `struct` definition | Lets you use `{:?}` to print the whole struct |
| `{:?}` | The format specifier for debug printing | `println!("{:?}", wallet)` | Prints all fields at once for debugging |
| Field init shorthand | Skip `name: name` when variable matches field name | `Wallet { balance }` instead of `Wallet { balance: balance }` | Less repetition |
| Struct update syntax | `..other` — copies remaining fields from an existing instance | `User { email: new_email, ..user1 }` | Quick way to create a modified copy |

---

## 3. Big Idea

A struct is a blueprint for data. An `impl` block is where you attach behavior to that data. Together they model the things your program works with — wallets, users, accounts.

**Memory hook:** The struct defines what it IS. The `impl` block defines what it DOES.

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| Struct definition vs instance | The struct is the blueprint; the instance is the actual data | `struct Wallet` vs `let wallet = Wallet { balance: 0 }` | One blueprint, many instances |
| `mut` applies to the whole instance | You cannot make one field mutable; all or nothing | `let mut wallet = Wallet { ... }` | If you need to change anything, the whole variable must be `mut` |
| `&self` vs `&mut self` vs `self` | Read / Modify / Consume | `&self` = read-only, `&mut self` = change fields, `self` = take it | Match the method to what it actually needs |
| Associated function | No `self` — called with `::` not `.` | `Wallet::new()` | The idiomatic Rust constructor pattern |
| Struct update syntax moves heap fields | `String` fields are moved, `u64` and `bool` are copied | After `..user1`, `user1.username` is gone if it is a `String` | `Copy` types are fine; heap types are moved |
| `#[derive(Debug)]` | Add to struct to enable `{:?}` printing | Useful for debugging without writing `println!` for every field | Just add it above the struct definition |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| A method that reads but does not change | `&self` | Read-only — does not need `mut` |
| A method that modifies the struct | `&mut self` | Write access — instance must be `mut` |
| A method that permanently takes the struct | `self` | Ownership transfers — original is gone |
| A constructor | Associated function `::new()` | No `self` — creates an instance from scratch |
| To group multiple values under one name | Struct | Models real things cleanly |
| To print the whole struct for debugging | `#[derive(Debug)]` + `{:?}` | Easier than printing field by field |
| To create a modified copy of an existing instance | Struct update syntax `..other` | Copies unchanged fields — watch out for `String` moves |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| Why do I get "cannot borrow as mutable"? | The instance variable is not declared `mut`. | Add `mut` to the `let` declaration: `let mut wallet = ...` |
| Why is `user1.username` invalid after struct update syntax? | `String` is a heap type. Update syntax moved it into `user2`. | `bool` and `u64` fields are not affected — they are copied |
| When do I use `::` vs `.`? | `.` for methods. `::` for associated functions. | `wallet.deposit(100)` vs `Wallet::new()` |
| Can I print a struct with `{}`? | No. You need `#[derive(Debug)]` and `{:?}`. | `println!("{:?}", wallet)` |
| Can a struct have more than one `impl` block? | Yes. Multiple `impl` blocks for the same type are valid. | All methods still belong to the same type |
| What is the difference between a method and an associated function? | A method takes `self`. An associated function does not. | `wallet.deposit()` is a method. `Wallet::new()` is an associated function. |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Forgetting `mut` on the instance | Cannot call `&mut self` methods | Add `mut` to `let mut wallet = ...` |
| Trying to make one field `mut` | Rust does not allow field-level mutability | The whole instance is `mut` or none of it is |
| Using `.` to call an associated function | `wallet.new()` does not work | Use `Wallet::new()` with `::` |
| String fields after struct update syntax | Original variable is partially moved | Check if you need both; clone `String` fields if so |
| Forgetting `#[derive(Debug)]` | `println!("{:?}", s)` does not compile | Add `#[derive(Debug)]` above the struct definition |
| Using `self` when you meant `&self` | The instance is consumed — original is gone | Only use `self` if you want to permanently take ownership |

---

## 8. Compiler Errors As Clues

| Error | What it means | What to check |
|---|---|---|
| `cannot borrow as mutable, as it is not declared as mutable` | Instance variable missing `mut` | Add `mut` to the `let` declaration |
| `use of moved value` | A `String` field was moved by struct update syntax | Clone the field or restructure |
| `no method named ... found` | Method does not exist on this type | Check the `impl` block — method may be misspelled |
| `doesn't implement Debug` | Struct does not have `#[derive(Debug)]` | Add `#[derive(Debug)]` above the struct |
| `expected &... found &mut ...` | Wrong reference type passed | Match the method signature — `&self` or `&mut self` |

---

## 9. Practice

### Practice 1 — Define And Use A Struct

**Prompt:** Define a `BankAccount` struct with `owner: String`, `balance: u64`, and `is_active: bool`. Create an instance.

**Check:**

```rust
#[derive(Debug)]
struct BankAccount {
    owner: String,
    balance: u64,
    is_active: bool,
}

let account = BankAccount {
    owner: String::from("Alice"),
    balance: 0,
    is_active: true,
};

println!("{:?}", account);
```

### Practice 2 — Add A Deposit Method

**Prompt:** Add an `impl` block with a `deposit(&mut self, amount: u64)` method.

**Check:**

```rust
impl BankAccount {
    fn deposit(&mut self, amount: u64) {
        self.balance += amount;
    }
}

let mut account = BankAccount { owner: String::from("Alice"), balance: 0, is_active: true };
account.deposit(500);
println!("{}", account.balance);  // 500
```

### Practice 3 — Add A Constructor

**Prompt:** Add a `new` associated function that creates a `BankAccount` with zero balance and `is_active: true`.

**Check:**

```rust
impl BankAccount {
    fn new(owner: String) -> BankAccount {
        BankAccount {
            owner,
            balance: 0,
            is_active: true,
        }
    }
}

let account = BankAccount::new(String::from("Bob"));
```

### Practice 4 — Add A Withdraw Method

**Prompt:** Add a `withdraw(&mut self, amount: u64) -> Result<(), String>` method that returns an error if the amount exceeds the balance.

**Check:**

```rust
fn withdraw(&mut self, amount: u64) -> Result<(), String> {
    if amount > self.balance {
        Err(String::from("Insufficient funds"))
    } else {
        self.balance -= amount;
        Ok(())
    }
}

match account.withdraw(1000) {
    Ok(()) => println!("Withdraw successful"),
    Err(reason) => println!("Failed: {}", reason),
}
```

---

## 10. Tiny Drills

Short repetitions. These should take 1-3 minutes each.

- Say the difference between a struct definition and an instance
- Say what dot notation is used for
- Name the three ways to take `self` in a method and what each one does
- Say when to use `::` vs `.`
- Explain what `#[derive(Debug)]` does in one sentence
- Say what happens to a `String` field after struct update syntax

---

## 11. Active Recall

Answer without looking:

1. What is a struct?
2. What is the difference between a struct definition and an instance?
3. How do you access a field on an instance?
4. Why does the whole instance need to be `mut` to change a field?
5. What does an `impl` block do?
6. What is the difference between `&self`, `&mut self`, and `self`?
7. What is an associated function and how do you call it?
8. What does `#[derive(Debug)]` do?
9. What happens to a `String` field after struct update syntax?
10. How does a Rust struct connect to a Solana account?

---

## 12. Relevance To Solana & Blockchain

| Lesson idea | Where it appears in Solana/blockchain | Why it matters |
|---|---|---|
| Struct definition | Every Anchor account struct | Fields in the struct are stored on-chain as bytes |
| `#[account]` attribute | Anchor's version of `impl` for accounts | Marks the struct as an on-chain account |
| `&mut self` methods | Writable account patterns in Anchor | Anchor passes mutable accounts just like `&mut self` |
| Associated functions | Program instruction constructors | `Wallet::new()` maps to account initialization |
| Field types | On-chain data layout | `Pubkey`, `u64`, `bool` are the common Solana field types |
| `impl` block | Instruction logic | The logic that changes account state lives in `impl`-like handlers |

**Memory hook:** A Solana account is a struct. An Anchor instruction is an `impl` method. The `&mut self` rules are the account mutability rules.

```rust
// A Solana account in Anchor — this is a Rust struct
#[account]
pub struct PaymentAccount {
    pub owner: Pubkey,
    pub balance: u64,
    pub is_active: bool,
}
```

---

## 13. Relevance To AI Agents

| Lesson idea | Where it appears in AI agents | Why it matters |
|---|---|---|
| Struct | Agent state model | An agent's memory, tools, and config can be grouped in a struct |
| `impl` block | Agent methods | Agent behaviors (run, pause, pay) are methods on the struct |
| `&self` | Read-only agent queries | Check current credit balance without modifying it |
| `&mut self` | Updating agent state | Deduct credits, update task status, record a payment |
| Associated function | Agent constructor | `Agent::new(config)` creates an agent with initial state |
| `#[derive(Debug)]` | Logging agent state | Print the full agent state for debugging |

**Memory hook:** An on-chain AI agent is a struct with an `impl` block — its state is stored on-chain, and its behaviors are the methods.

---

## 14. Transcript Coverage Check

| Transcript topic | Covered? | Where |
|---|---|---|
| Struct definition and syntax | Yes | Recap, terms, practice 1 |
| Instance creation | Yes | Terms, practice 1 |
| Dot notation | Yes | Terms, key concepts |
| `mut` on the whole instance | Yes | Recap, confusions, traps |
| Field init shorthand | Yes | Recap, terms |
| Struct update syntax | Yes | Recap, key concepts, confusions |
| String fields moved by update syntax | Yes | Key concepts, confusions, traps |
| `impl` block | Yes | Recap, terms, practice 2 |
| `&self` | Yes | Terms, key concepts, drills |
| `&mut self` | Yes | Terms, practice 2 |
| `self` (consuming) | Yes | Terms, key concepts |
| Associated function | Yes | Recap, terms, practice 3 |
| `::` syntax | Yes | Terms, confusions, drills |
| `#[derive(Debug)]` and `{:?}` | Yes | Recap, terms, practice 1 |
| Wallet example from class | Yes | Practice 2, Solana relevance |
| Solana account = Rust struct | Yes | Solana relevance |

---

## 15. Final Study Checklist

- [ ] I can define a struct with named fields and correct types
- [ ] I can create an instance and access fields with dot notation
- [ ] I know why the whole instance must be `mut` to change a field
- [ ] I can write an `impl` block with `&self` and `&mut self` methods
- [ ] I can write an associated function and call it with `::`
- [ ] I know what `#[derive(Debug)]` does and how to use `{:?}`
- [ ] I completed all four practice exercises
- [ ] I can explain the Solana/blockchain relevance
- [ ] I can explain the AI agent relevance

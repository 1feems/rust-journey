---
title: "Session 6 — Study Guide: Error Handling"
description: "Study-back guide for error handling — panic, Result, the ? operator, and custom error types."
---

# Study Guide — Session 6: Error Handling

> Use this after watching the lesson, reading the synthesis, and asking follow-up questions.
> Goal: remember the lesson, understand confusing parts, and practice the core skills.

---

## 0. Inputs Check

**Did the student do a separate Q&A with ChatGPT after the lesson?**

- **No** — using in-class Q&A from the transcript plus the synthesis as the source.

| Source | Used? |
|---|---|
| Transcript | Yes — `pipeline/transcripts/rust-school/cleaned/lesson-6-error-handling.txt` |
| Synthesis | Yes — `02-synthesis.md` |
| Post-session Q&A | No |

---

## 1. Lesson Recap

- Good software is not software that never fails — it is software that **fails safely and predictably**
- Rust has two categories of error: **unrecoverable** (use `panic!`) and **recoverable** (use `Result`)
- `panic!` stops the program immediately — use it only for genuine bugs and critical security violations, not for normal user mistakes
- `Result<T, E>` returns either `Ok(value)` on success or `Err(reason)` on failure — the program keeps running and you handle both outcomes
- The `?` operator is shorthand: add `?` after any `Result`-returning call and it automatically propagates the error to the caller if there is one — no `match` needed
- Custom error enums replace `String` errors — they enforce a shared vocabulary across a team and let the compiler catch typos
- The `thiserror` crate is the production standard for defining custom error types — optional, but widely used

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| `panic!` | Stops the program completely right now | A bug made something impossible happen, or a critical rule was violated | Program cannot safely continue — everything stops |
| Unrecoverable error | An error the program cannot come back from | A programmer mistake, corrupted state, malicious access attempt | Use `panic!` — the only safe response is to stop |
| Recoverable error | An expected failure the program can handle and keep going | User overdrafts, missing file, bad input | Use `Result` — the program keeps running |
| `Result<T, E>` | A type that holds either success or failure | Any function that can fail | Forces you to handle both outcomes — you cannot ignore it |
| `Ok(value)` | The success branch of `Result` | When the operation worked | Unwraps the value and continues |
| `Err(reason)` | The failure branch of `Result` | When the operation failed | Carries the reason so the caller can decide what to do |
| `?` operator | Propagates an error to the caller automatically | When you want the error to travel up, not be handled here | Eliminates repetitive `match` blocks |
| Error propagation | Passing an error up to the calling function | When you are not the right place to handle it | Keeps error handling in one place |
| Custom error enum | An enum whose variants are specific error types | Instead of `String` errors | Consistent, typed, and compiler-checked |
| `thiserror` crate | A Rust package for defining custom error types cleanly | Production Rust projects | Standardizes error format — widely used in open source and enterprise code |

---

## 3. Big Idea

Every function that can fail must say so. Rust makes you handle `Result` — you cannot pretend a failure did not happen. This is by design: it forces you to build software that fails safely.

**Memory hook:** In Rust, ignoring an error is not an option — the compiler will not allow it.

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| `panic!` | Crashes the whole program — no recovery | `panic!("critical failure")` | For bugs, not user mistakes |
| `Result<T, E>` | Either it worked (`Ok`) or it did not (`Err`) | `-> Result<u64, String>` | The program keeps running either way |
| `Ok(value)` | Success — holds the result you wanted | `Ok(balance - amount)` | Unwrapped when you match on it |
| `Err(reason)` | Failure — holds the explanation | `Err(String::from("Insufficient balance"))` | Caller must decide what to do with it |
| `match` on `Result` | Forces you to handle both `Ok` and `Err` | `match withdraw(...) { Ok(b) => ..., Err(e) => ... }` | No silent failures |
| `?` operator | Propagate or continue — one character | `let balance = withdraw(100, 50)?;` | Returns `Err` early if it fails |
| Custom error enum | Named variants instead of strings | `BankError::InsufficientBalance` | Consistent across the whole team |
| `thiserror` | Derive macro that adds a display message to each enum variant | `#[error("Insufficient balance")]` | Lets you `println!("{}", e)` cleanly |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| The program to stop immediately — no recovery | `panic!` | For bugs and impossible states only |
| A function to signal failure but keep the program alive | `Result<T, E>` | The caller decides what to do next |
| To handle the error right here in this function | `match` on `Result` | You want to react now, not pass it up |
| To pass the error up to whoever called your function | `?` operator | Cleaner than writing `match` every time |
| Clear, typed error messages instead of free-form strings | Custom error enum | Consistent across the team, compiler-checked |
| Production-grade error display messages | `thiserror` | Adds `println!("{}", e)` support with one line |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| When do I use `panic!` vs `Result`? | `panic!` for bugs that should never happen. `Result` for anything a normal user might trigger. | A user overdrafting is `Result`. A bug causing negative balance is `panic!` |
| What does `?` actually do? | If the `Result` is `Err`, it immediately returns that `Err` from the current function. If it is `Ok`, it unwraps the value and continues. | `let b = withdraw(100, 50)?;` — if withdraw fails, the whole function returns that `Err` |
| Can I use `?` in main? | Yes — change `fn main()` to `fn main() -> Result<(), Box<dyn std::error::Error>>` | Then `?` works in `main` too |
| Why not just use String for errors? | Strings are inconsistent. One dev writes `"low funds"`, another writes `"Insufficient Balance"`. An enum forces everyone to use the same variants. | `BankError::InsufficientBalance` — same every time |
| Is `thiserror` required? | No. A plain enum works fine. `thiserror` just makes the format cleaner and is the convention in larger projects. | Both approaches produce the same behaviour |
| Does `?` work with any error type? | Only if the error types match. If `withdraw` returns `Err(String)` and your function also returns `Result<_, String>`, `?` works fine. If types differ, you need a conversion. | Use the same error type across functions using `?` together |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Using `panic!` for normal user errors | Program crashes when a user types the wrong thing | Ask: could a normal user cause this? If yes, use `Result` |
| Forgetting to `match` on a `Result` | The compiler warns you — and the warning is actually an error in some contexts | Always match on `Ok` and `Err`, or use `?` to propagate |
| Using `?` in a function that does not return `Result` | Compiler error: `?` can only be used in a function that returns `Result` or `Option` | Make the outer function return `Result` before adding `?` |
| Returning inconsistent `String` errors | Different messages for the same failure — hard to test and maintain | Define a custom error enum once and use its variants everywhere |
| Printing a custom error enum with `{:?}` instead of `{}` | Shows `InsufficientBalance` in debug format, not a human message | Add `#[derive(Debug)]` and use `thiserror` to get `{}` printing |
| Thinking `Ok` means the operation was perfect | `Ok` just means no error. You still need to check the value inside. | Inspect what `Ok(value)` contains before trusting it |

---

## 8. Compiler Errors As Clues

| Error or warning | What it means | What to check |
|---|---|---|
| `unused Result that must be used` | You called a function returning `Result` and ignored the outcome | Add a `match`, use `?`, or call `.unwrap()` to acknowledge it |
| `the ? operator can only be used in a function that returns Result or Option` | `?` is inside a function that does not return `Result` | Change the outer function's return type to `Result<_, E>` |
| `mismatched types: expected Result<_, String>, found Result<_, WalletError>` | Error types do not match — `?` cannot propagate across them | Make both functions use the same error type, or implement `From` |
| `error[E0277]: the trait Display is not implemented for WalletError` | Trying to print the error with `{}` but the type has no display message | Add `thiserror` with `#[error("...")]` or implement `fmt::Display` manually |
| `error[E0004]: non-exhaustive patterns: Err not covered` | `match` only handles `Ok` — `Err` is missing | Add an `Err(e) => ...` arm to your match |

---

## 9. Practice

### Practice 1 — Fix the code

**Prompt:** This code tries to use `?` but the compiler rejects it. Why? Fix it.

```rust
fn get_balance(id: u64) -> u64 {
    let balance = fetch_from_db(id)?;
    balance
}

fn fetch_from_db(id: u64) -> Result<u64, String> {
    Ok(id * 100)
}
```

**Check:** `get_balance` must return `Result<u64, String>` for `?` to work. Fix: `fn get_balance(id: u64) -> Result<u64, String>`.

---

### Practice 2 — Choose the right tool

**Prompt:** For each situation, decide: `panic!` or `Result`?

1. A user tries to send more tokens than they have
2. An internal counter wraps around below zero due to a bug
3. A file the program needs does not exist at startup
4. A student enters an empty name in a form

**Check:**
1. `Result` — expected user error
2. `panic!` — programming bug that should never happen
3. `Result` — expected failure the program can handle
4. `Result` — normal user mistake

---

### Practice 3 — Predict the output

**Prompt:** What does this print and why?

```rust
fn withdraw(balance: u64, amount: u64) -> Result<u64, String> {
    if amount > balance {
        Err(String::from("Insufficient balance"))
    } else {
        Ok(balance - amount)
    }
}

fn main() {
    match withdraw(50, 100) {
        Ok(b)  => println!("New balance: {}", b),
        Err(e) => println!("Failed: {}", e),
    }
}
```

**Check:** Prints `Failed: Insufficient balance` — amount (100) is greater than balance (50) so `Err` is returned and the `Err` arm of match runs.

---

### Practice 4 — Write it yourself

**Prompt:** Write a function called `process_payment` that calls `withdraw` (from Practice 3) and uses `?` to propagate any error. `process_payment` should print "Payment processed" on success and return the new balance.

**Check:**
```rust
fn process_payment(balance: u64, amount: u64) -> Result<u64, String> {
    let new_balance = withdraw(balance, amount)?;
    println!("Payment processed");
    Ok(new_balance)
}
```

---

## 10. Tiny Drills

Short questions — 1-2 minutes each. Try to answer without looking.

- **Same or different?** `panic!` vs `Err` — what happens to the program in each case?
- **What prints?** `match Ok(42) { Ok(n) => println!("{}", n), Err(_) => println!("fail") }` — what output do you see?
- **Fill in the blank:** `fn divide(a: u64, b: u64) -> Result<u64, ___>` — what type should the blank be if you want to return an error string?
- **True or false:** You can use `?` in a function that returns `()`.
- **Name the variant:** If a `WalletError` enum has `InsufficientBalance` and `InvalidAmount`, what do you write to return the second one?
- **One word:** What Rust keyword/operator propagates a `Result` error automatically without writing a full `match`?

---

## 11. Active Recall

Answer without looking:

1. What is the difference between a recoverable and an unrecoverable error in Rust?
2. What does `Result<T, E>` contain when it is successful? When it fails?
3. What does `?` do if the `Result` is `Err`?
4. Why is a custom error enum better than returning a `String`?
5. What does `thiserror` add that a plain enum does not have?
6. In which situations should you use `panic!` instead of `Result`?

---

## 12. Relevance To Solana & Blockchain

| Lesson idea | Where it appears in Solana/blockchain | Why it matters |
|---|---|---|
| `Result<T, E>` | Every Anchor instruction handler returns `Result<(), ProgramError>` | If it returns `Err`, the entire transaction is reverted |
| `?` operator | Used in virtually every Anchor program to propagate errors from account checks, arithmetic, CPI calls | Keeps instruction handlers readable without nested matches |
| Custom error enum | `#[error_code]` in Anchor defines program-specific errors that appear in transaction logs | Lets developers and users debug exactly what went wrong |
| `panic!` avoided | Anchor programs never panic intentionally — they return `Err` | A panic in a Solana program means the transaction silently fails — returning `Err` is the correct path |

**Memory hook:** In Solana, returning `Err` is safe — the transaction reverts. Panicking is not — always use `Result`.

---

## 13. Relevance To AI Agents

| Lesson idea | Where it appears in AI agents | Why it matters |
|---|---|---|
| `Result<T, E>` | Agent tool calls can fail — an LLM call, a wallet transaction, a database write all return success or failure | Lets the agent decide: retry, fall back, or report the error |
| `?` operator | An agent processing a multi-step task uses `?` to propagate a failure from step 2 up to the top-level handler | Keeps agent logic flat and readable instead of deeply nested |
| Custom error enum | An on-chain payment agent defines errors like `InsufficientCredits`, `TransactionTimeout`, `InvalidRecipient` | Structured errors let the agent log and respond differently to each failure type |
| `panic!` avoided | An agent that panics crashes the whole process — a well-built agent returns `Err` and lets the caller decide | Agents should degrade gracefully, not crash |

**Memory hook:** An agent that returns `Err` can be retried. An agent that panics is dead.

---

## 14. Transcript Coverage Check

| Transcript topic | Covered? | Where |
|---|---|---|
| What good software means — fails safely, not never | Yes | Section 1 (Lesson Recap), Section 3 (Big Idea) |
| `panic!` for unrecoverable errors | Yes | Sections 4, 5, 7, 8 |
| `Result<T, E>` with `Ok` and `Err` | Yes | Sections 2, 4, 9 |
| `match` to handle both `Result` branches | Yes | Sections 4, 9 |
| The `?` operator and what it does | Yes | Sections 2, 4, 5, 6, 8, 9 |
| Error propagation concept | Yes | Sections 2, 4, 6 |
| Custom error enums replacing `String` | Yes | Sections 2, 4, 5, 6, 7 |
| `thiserror` crate and `#[error("...")]` | Yes | Sections 2, 4, 6, 7, 8 |
| In-class Q&A: "why not just pass the enum directly?" | Yes | Section 6 (Common Confusions) |
| Solana connection — Anchor, `ProgramError`, `#[error_code]` | Yes | Section 12 |
| Homework assigned — IronCloud codebase exploration | Noted | `05-homework.md` |

---

## 15. Final Study Checklist

- [ ] I can explain the difference between `panic!` and `Result`
- [ ] I know when to use `panic!` and when to use `Result`
- [ ] I can write a function that returns `Result<T, E>`
- [ ] I can use `match` to handle both `Ok` and `Err`
- [ ] I know what `?` does and where it can be used
- [ ] I can define a custom error enum with at least two variants
- [ ] I understand why enums are better than strings for errors
- [ ] I know what `thiserror` adds and that it is optional
- [ ] I can explain the Solana relevance — Anchor, `Result`, `?`, `#[error_code]`
- [ ] I can explain the AI agent relevance — tool call failures, structured errors

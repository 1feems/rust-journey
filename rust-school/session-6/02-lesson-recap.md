# Lesson Recap #6: Error Handling

Good software doesn't never fail — it fails safely and predictably. Rust forces you to handle errors explicitly so nothing slips through silently.

---

## Key Takeaways

- `panic!` stops the program completely — for bugs and impossible states only, not expected user errors
- `Result<T, E>` handles recoverable errors — `Ok(value)` on success, `Err(reason)` on failure
- The `?` operator propagates errors upward automatically — cleaner than writing `match` every time
- Custom error enums replace inconsistent strings — everyone uses the same named variants
- Every Solana instruction handler returns `Result` — this session is direct preparation for Anchor programs

---

## Why This Matters

Every program you build will eventually encounter something going wrong. How your program responds determines whether it is reliable or fragile.

Rust does not let you ignore errors. If a function can fail, it must return a `Result`. If you receive a `Result`, you must handle both outcomes. There is no silent failure — which is exactly why Solana uses Rust.

---

## Important Rules

- Use `panic!` for programming bugs and impossible states — not for expected failures
- Use `Result` for anything a normal user might trigger — wrong input, insufficient balance, missing account
- The `?` operator only works inside a function that returns `Result`
- Custom error enums are preferred over `String` for team projects — consistent, matchable, compiler-checked

---

## Key Terms

| Term | Meaning |
|---|---|
| `panic!` | Stops the program immediately. For critical, unrecoverable situations only |
| Unrecoverable error | An error the program cannot continue from. Use `panic!` |
| Recoverable error | An expected failure the program can handle and keep running |
| `Result<T, E>` | Either `Ok(value)` on success or `Err(reason)` on failure |
| `Ok` | The success variant of `Result`. Contains the value |
| `Err` | The failure variant of `Result`. Contains the reason |
| `?` operator | Propagates an error upward automatically. Replaces repetitive `match` |
| Error propagation | Passing an error up to the calling function instead of handling it immediately |
| Custom error enum | An enum whose variants are specific error types, replacing string messages |
| `thiserror` crate | A Rust package for defining custom error types in a standard, clean way |

---

## Key Concepts

**`panic!` — Unrecoverable Errors:**
Use panic for bugs and situations that should never happen — not for normal user failures.

```rust
fn withdraw(balance: u64, amount: u64) {
    if amount > balance {
        panic!("Insufficient balance — program stopping");
    }
}
```

---

**`Result<T, E>` — Recoverable Errors:**
Return `Ok` on success, `Err` with a reason on failure. The caller must handle both.

```rust
fn withdraw(balance: u64, amount: u64) -> Result<u64, String> {
    if amount > balance {
        Err(String::from("Insufficient balance"))
    } else {
        Ok(balance - amount)
    }
}
```

---

**The `?` Operator:**
Instead of writing a `match` every time you call a `Result`-returning function, use `?` to propagate the error automatically.

```rust
fn process_transaction(balance: u64, amount: u64) -> Result<u64, String> {
    let new_balance = withdraw(balance, amount)?;   // returns Err immediately if it fails
    println!("Transaction complete. Balance: {}", new_balance);
    Ok(new_balance)
}
```

---

**Custom Error Enums:**
Replace inconsistent strings with named variants everyone on the team shares.

```rust
enum BankError {
    InsufficientBalance,
    AccountNotFound,
}
```

---

## Step-by-Step Walkthrough

Tracing what happens when `process_transaction(100, 150)` is called:

```rust
let new_balance = withdraw(balance, amount)?;
```

1. `withdraw(100, 150)` is called — amount (150) exceeds balance (100)
2. `withdraw` returns `Err(String::from("Insufficient balance"))`
3. The `?` operator sees an `Err` — it immediately returns that error from `process_transaction`
4. `process_transaction` exits early — the `println!` line is never reached
5. The caller receives `Err("Insufficient balance")`

Final state:

```rust
Err("Insufficient balance")
```

---

## Common Confusions

* "When do I use `panic!` vs `Result`?"
  `panic!` is for bugs — things that should never happen. `Result` is for expected failures — things a normal user might trigger like an empty field or insufficient balance.

* "What does `?` actually do?"
  If the `Result` is `Ok`, it unwraps the value and continues. If it's `Err`, it returns the error from the current function immediately. It is shorthand for a `match` that either continues or returns early.

* "Why use an enum instead of a String for errors?"
  Strings are inconsistent — different developers write different messages for the same error. An enum gives everyone the same named variants. It is also matchable and compiler-checked.

* "Can I use `?` anywhere?"
  Only inside a function that returns `Result` (or `Option`). Using it in `main` requires `main` to return `Result`.

---

## Common Mistakes

- Using `panic!` for expected user errors — use `Result` instead
- Forgetting that `?` only works inside a `Result`-returning function
- Returning a reference to a local variable — return the value itself instead
- Using `String` for error messages in team code — use a custom error enum

These usually happen because error handling in Rust is more explicit than most other languages — but that explicitness is what makes Rust programs reliable in production.

---

# Source

- Recording: (check Telegram group for session resources)
- Transcript: `pipeline/transcripts/rust-school/cleaned/lesson-6-error-handling.txt`
- Course: Rust School
- Session: 6

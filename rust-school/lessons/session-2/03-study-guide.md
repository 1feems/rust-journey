# Study Guide — Rust School Session 2: Rust Basics

> Use this after watching the class recording and reading the synthesis.
> No separate post-session Q&A recap was provided, so this guide uses the in-class transcript questions and the synthesis.

---

## 1. Lesson Recap

- Variables are containers for values. The instructor used the glass-and-water analogy: the glass is the variable, the water is the value.
- Rust variables are immutable by default. Use `mut` when a value needs to change.
- `const` is for values that never change. Constants need a type and are usually written in uppercase.
- Shadowing means creating a new variable with the same name, hiding the old value.
- Rust data types describe the nature of a value: integer, float, boolean, character, string, and more.
- Functions are reusable blocks of code. They can take inputs called parameters and return typed values.
- Control flow decides what code runs: `if`, `else`, and `else if`.
- Loops repeat code: `for` for ranges, `while` for conditions, and `loop` for infinite repetition until `break`.

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| Variable | A container for a value | `let a = 10;` | Lets you store and reuse data |
| `let` | Creates a variable | When declaring a new value | Main way to bind values in Rust |
| Immutable | Cannot be changed after creation | Default `let` behavior | Prevents unexpected changes |
| `mut` | Allows a variable to change | `let mut count = 1;` | Needed for counters, loops, changing state |
| `const` | A value that never changes | `const MAX_SUPPLY: u64 = 1_000_000;` | Stores fixed values clearly |
| Type annotation | Writing the type explicitly | `let a: i32 = 10;` | Makes intent clear |
| Shadowing | Reusing a variable name with a new binding | `let a = 10; let a = 12;` | Lets you transform a value without mutation |
| `i32` | Signed 32-bit integer | General whole numbers | Can hold positive and negative values |
| `i64` | Signed 64-bit integer | Larger whole numbers | More range, more memory |
| `u64` | Unsigned 64-bit integer | Lamports, balances, counts | Cannot go negative |
| `f64` | 64-bit floating point | Decimal values | Represents numbers with decimals |
| `bool` | `true` or `false` | Conditions and flags | Required for `if` conditions |
| `char` | One character | Single letters/symbols | Different from text strings |
| `String` | Owned, growable text | Text built or changed at runtime | Useful for dynamic text |
| `&str` | Borrowed string slice | Fixed text/literals | Common for parameters and labels |
| Function | Reusable block of code | `fn greet() { ... }` | Avoids repeating code |
| Parameter | Input a function accepts | `fn greet(name: &str)` | Makes functions dynamic |
| Argument | Actual value passed into a function | `greet("Shiv")` | Supplies data to parameters |
| Return type | Type a function gives back | `fn add(...) -> i32` | Tells Rust what output to expect |
| Control flow | Choosing which code runs | `if`, `else if`, `else` | Adds decision-making |
| `for` loop | Repeats over a range/collection | `for i in 1..=5` | Best when count/range is known |
| `while` loop | Repeats while a condition is true | `while count <= 5` | Best when stopping depends on a condition |
| `loop` | Infinite loop | `loop { ... break; }` | Runs until manually stopped |
| `break` | Stops a loop | Inside `loop` or other loops | Prevents infinite running |

---

## 3. Big Idea

Rust makes you be explicit about change, types, function inputs, function outputs, and control flow.

**Memory hook:** Rust wants clear rules before it runs your code.

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| Variables | Named containers for values | `let a = 10;` | Glass = variable, water = value |
| Mutability | Values are locked unless marked `mut` | `let mut a = 10; a = 12;` | No `mut`, no change |
| Constants | Fixed values that never change | `const VALUE: i32 = 10;` | Constants need a type |
| Shadowing | New binding with the same name | `let a = 10; let a = 12;` | Same name, new value |
| Data types | The nature of a value | `i32`, `bool`, `String` | Types help Rust catch mistakes |
| Functions | Reusable code blocks | `fn greet() { ... }` | A function is like a machine |
| Parameters | Named inputs to a function | `fn greet(name: &str)` | Parameter receives the argument |
| Return values | Output from a function | `fn add(...) -> i32` | `->` tells the return type |
| Control flow | Code chooses a path | `if age >= 18 { ... }` | Conditions must be `bool` |
| Loops | Repeat code | `for`, `while`, `loop` | Pick the loop based on the stop rule |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| A normal value that should not change | `let` | Immutable by default |
| A value that should change | `let mut` | Makes change explicit |
| A value that never changes anywhere | `const` | Clear fixed value |
| To reuse a name with a transformed value | Shadowing | New binding, not mutation |
| A number that can be negative | `i32` or `i64` | Signed integers allow negatives |
| A balance or count that cannot be negative | `u64` | Unsigned integer prevents negatives |
| A true/false decision | `bool` | Required for conditions |
| A reusable action | Function | Avoid repeating code |
| A function needs input | Parameter | Makes the function flexible |
| A function gives output | Return type with `->` | Tells Rust what comes back |
| A known range/count | `for` loop | Cleanest for ranges |
| Repeat while a condition is true | `while` loop | Stops when condition fails |
| Run until manually stopped | `loop` + `break` | Infinite until broken |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| Are variables immutable by default? | Yes. Add `mut` if the value should change. | `let mut count = 1;` |
| Do we always specify the type? | No. Rust can infer often, but explicit types help clarity. | `let a: i32 = 10;` |
| Should constants be uppercase? | Not required, but Rust convention uses uppercase. | `const MAX_SUPPLY: u64 = 1_000_000;` |
| Are constants mutable? | No. A constant can never be `mut`. | `const VALUE: i32 = 10;` |
| What is shadowing? | Creating a new variable with the same name. | `let a = 10; let a = 12;` |
| Is shadowing the same as mutation? | No. Mutation changes a value; shadowing creates a new binding. | `let a = "10"; let a = 10;` |
| What is a parameter? | A named input inside a function. | `name` in `fn greet(name: &str)` |
| What is an argument? | The actual value passed into the function. | `"Shiv"` in `greet("Shiv")` |
| What does `-> i32` mean? | The function returns an `i32`. | `fn add(a: i32, b: i32) -> i32` |
| What is `1..5` vs `1..=5`? | `1..5` stops before 5. `1..=5` includes 5. | `1,2,3,4` vs `1,2,3,4,5` |
| Why use `loop`? | Mostly for things that run until a specific event. Beginners usually use `for` or `while`. | `loop { if done { break; } }` |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Forgetting `mut` | Rust blocks reassignment | Use `let mut` for changing values |
| Thinking `const` and `let` are the same | `const` is always fixed and needs a type | Use `const` only for permanent values |
| Confusing shadowing with mutation | You think the same variable changed | Remember shadowing creates a new binding |
| Using `i32` for balances | Negative balances become possible | Use `u64` for lamports/balances |
| Forgetting function return type | Function does not declare output | Add `-> Type` |
| Adding semicolon to final return expression | Function returns `()` instead of the value | No semicolon on final expression |
| Using non-bool in `if` | Rust rejects condition | Conditions must evaluate to `bool` |
| Misreading ranges | Loop prints one too few/many | Use `..=` when including the end |
| Infinite `loop` without `break` | Program never stops | Add a break condition |

---

## 8. Compiler Errors As Clues

| Error or warning | What it means | What to check |
|---|---|---|
| `cannot assign twice to immutable variable` | You changed a locked variable | Add `mut` or use shadowing |
| `mismatched types` | Rust expected one type and got another | Check annotations and function return type |
| `expected i32, found ()` | Function returned nothing instead of `i32` | Remove semicolon from final expression |
| `missing type for const item` | Constants need explicit types | Add `: Type` after the constant name |
| `unused variable` | You created a value but never used it | Print it, use it, or prefix with `_` |
| Loop never ends | No compiler error, but program keeps running | Add `break` or change loop type |

---

## 9. Practice

### Practice 1 — Make It Mutable

**Prompt:** Fix the error.

```rust
let balance = 1000;
balance = 1500;
```

**Check:**

```rust
let mut balance = 1000;
balance = 1500;
```

### Practice 2 — Const Or Let?

**Prompt:** Which should be `const` and which should be `let mut`?

- Maximum token supply
- A counter that increases in a loop

**Check:**

```rust
const MAX_SUPPLY: u64 = 1_000_000;
let mut count = 1;
```

### Practice 3 — Function Return

**Prompt:** Fix the function.

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b;
}
```

**Check:**

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

### Practice 4 — Range Check

**Prompt:** Which loop prints `1, 2, 3, 4, 5`?

```rust
for i in 1..5 {
    println!("{}", i);
}

for i in 1..=5 {
    println!("{}", i);
}
```

**Check:** `1..=5` includes 5.

---

## 10. Tiny Drills

Short repetitions. These should take 1-3 minutes each.

- Say what changes: `let mut count = 1; count += 1;`
- Say what stays fixed: `const VALUE: i32 = 10;`
- Explain the difference: `let a = 10; let a = 12;`
- Name the parameter: `fn greet(name: &str) { }`
- Name the argument: `greet("Shiv");`
- Say what returns: `fn add(a: i32, b: i32) -> i32 { a + b }`
- Predict the output: `for i in 1..=3 { println!("{}", i); }`

---

## 11. Active Recall

Answer without looking:

1. What is a variable?
2. What does `mut` do?
3. What is the difference between `let` and `const`?
4. Why does `const` need a type?
5. What is shadowing?
6. What is the difference between a parameter and an argument?
7. What does `-> i32` mean?
8. Why does the final expression in a function usually not have a semicolon?
9. What is the difference between `1..5` and `1..=5`?
10. When would you use `for`, `while`, or `loop`?

---

## 12. Relevance To Solana & Blockchain

| Lesson idea | Where it appears in Solana/blockchain | Why it matters |
|---|---|---|
| `u64` | Lamports and token balances | Balances should not go negative |
| `mut` | Writable account state | State changes must be explicit |
| `const` | Fixed limits or config values | Helps represent rules that should not change |
| Functions | Instruction handlers | Solana programs are built from functions |
| Return types | Result/error handling | Handlers must return the expected type |
| Control flow | Validating accounts and permissions | Programs branch based on conditions |
| Loops | Iterating accounts or data lists | Many programs inspect collections of accounts |

**Memory hook:** Solana programs are functions that safely change typed account state.

---

## 13. Relevance To AI Agents

| Lesson idea | Where it appears in AI agents | Why it matters |
|---|---|---|
| Variables | Store task data, status, counters | Agents need state |
| `mut` | Updating memory, retries, credits | Change should be intentional |
| `const` | Fixed limits, model names, thresholds | Keeps configuration stable |
| Functions | Tools and reusable actions | Agents call tools like functions |
| Parameters | Tool inputs | Tools need typed inputs |
| Return values | Tool outputs | Agents need structured results |
| Control flow | Decide next action | Agents branch based on conditions |
| Loops | Polling, retries, repeated checks | Agents often repeat until done |

**Memory hook:** An agent is a loop of typed actions, state checks, and tool calls.

---

## 14. Transcript Coverage Check

| Transcript topic | Covered? | Where |
|---|---|---|
| Variables as containers | Yes | Recap, terms, key concepts |
| `let` | Yes | Terms, key concepts |
| `mut` and immutability | Yes | Terms, confusions, practice |
| `const` | Yes | Terms, confusions, practice |
| Uppercase const convention | Yes | Common confusions |
| Type annotations | Yes | Terms, errors |
| Shadowing | Yes | Terms, confusions, drills |
| Data types | Yes | Recap, key concepts |
| `i32`, `i64`, `u64` | Yes | Terms, how to choose |
| `bool` | Yes | Terms |
| `char`, `String`, `&str` | Yes | Terms |
| Functions | Yes | Terms, concepts |
| Parameters and arguments | Yes | Terms, confusions, drills |
| Return type `->` | Yes | Terms, confusions, practice |
| Final expression / semicolon rule | Yes | Traps, errors, practice |
| `if`, `else`, `else if` | Yes | Terms, relevance |
| `for` loop | Yes | Terms, practice |
| `1..5` vs `1..=5` | Yes | Confusions, practice |
| `while` loop | Yes | Terms, active recall |
| `loop` and `break` | Yes | Terms, confusions |
| Nested loop/star pattern homework | Yes | What to practice next |

---

## 15. Final Study Checklist

- [ ] I know the main idea
- [ ] I know the key terms
- [ ] I can choose between `let`, `mut`, `const`, and shadowing
- [ ] I can explain function parameters and return values
- [ ] I can explain `for`, `while`, and `loop`
- [ ] I can avoid the beginner traps
- [ ] I completed the practice
- [ ] I can explain the Solana/blockchain relevance
- [ ] I can explain the AI agent relevance

# Study Guide — Rust School Session 1: Introduction to Rust

> Use this after watching the class recording and reading the synthesis.
> Session 1 is entirely conceptual — no code examples were shown. Use this guide to lock in the *why* before moving to syntax in Session 2.

---

## 1. Lesson Recap

- Rust is a programming language designed to be fast and safe at the same time — most languages force you to choose one
- The compiler is Rust's main safety tool. It checks your code before it ever runs and refuses to compile anything it finds dangerous
- Memory bugs are when a program reads or writes to memory it should not — a common source of crashes in older languages
- Concurrency bugs happen when two parts of a program try to change the same thing at the same time, causing data corruption
- Languages like Python and JavaScript use a garbage collector — a background process that cleans up unused memory. Rust does not have one, and does not need one
- Solana chose Rust because smart contracts handle real money and bugs in deployed contracts can be irreversible

---

## 2. Terms To Know

| Term | What it means | When you would use it | Why it matters |
|---|---|---|---|
| Compiler | The tool that checks your code before it runs | Every time you build a Rust program | Catches bugs before users see them |
| Memory bug | When a program reads or writes memory it should not | C/C++ programs without safety rules | Can crash programs or create security holes |
| Concurrency | Multiple parts of a program running at the same time | Web servers, games, anything with parallel tasks | Shared state between concurrent parts causes subtle bugs |
| Garbage collector | A background process that cleans up unused memory | Python, JavaScript, Java | Trades performance for convenience |
| Production | When software is live and real users are using it | After deployment | Bugs that reach production affect real people |
| Ownership | Rust's system for tracking who is responsible for each piece of memory | Every Rust program | Covered in depth in Session 3 |
| Smart contract | A program deployed on a blockchain that handles transactions | Solana programs | Cannot be easily patched after deployment |

---

## 3. Big Idea

Rust moves error detection from runtime (when users are affected) to compile time (before the program ever runs).

**Memory hook:** The compiler is not blocking you. It is catching real problems before they can do damage.

---

## 4. Key Concepts

| Concept | Plain-English explanation | Example | Remember |
|---|---|---|---|
| Why Rust exists | Most languages pick speed or safety — Rust picks both | C is fast but unsafe; Python is safe but slow; Rust is both | No trade-off |
| The compiler as safety net | Rust rejects code that breaks its rules before it runs | Unsafe memory access = compile error | Error at compile time beats error in production |
| Concurrency safety | Two things cannot accidentally corrupt shared data | Two threads writing the same account at once | Ownership makes this impossible |
| No garbage collector | Memory is managed by ownership rules, not a background process | No GC = no performance cost | Faster than managed languages |
| Why Solana chose Rust | Smart contracts cannot be easily patched after deployment | A bug in a live contract costs real money | Compile-time safety matters most for financial programs |

---

## 5. How To Choose

| If you need... | Use... | Why |
|---|---|---|
| Speed and safety together | Rust | No other widely-used language delivers both |
| Safe memory without manual management | Rust's ownership system | Replaces the garbage collector with compile-time rules |
| Financial programs that cannot have bugs | Rust | Bugs in deployed contracts can be permanent |
| A beginner-friendly scripting language | Python or JavaScript | Not Rust — Rust has a steep learning curve |
| Systems-level control (OS, firmware, drivers) | Rust or C/C++ | Rust gives C-level control with safety |

---

## 6. Common Confusions

| Confusion | Clear answer | Quick example |
|---|---|---|
| Is the compiler being annoying when it rejects my code? | No. It found a real problem. | Read the error message — it tells you exactly what is wrong |
| Do other languages have these bugs too? | Yes. They just let them reach runtime instead. | Python crashes at runtime; Rust refuses to compile |
| Does Rust have a garbage collector? | No. Ownership replaces it. | No background process, no performance cost |
| Is Rust the only safe language? | No, but it is the only one with C-level speed and no GC. | Go has a GC. Java has a GC. Rust does not. |
| Do I need to understand ownership now? | No. Session 3 covers it fully. | Session 1 is motivation only |
| Why would a blockchain use Rust specifically? | Bugs in smart contracts handle real money and are hard to reverse. | Rust's compile-time guarantees prevent a whole category of bugs |

---

## 7. Beginner Traps

| Trap | What goes wrong | How to avoid it |
|---|---|---|
| Fighting the compiler | Feeling blocked when the compiler rejects code | Shift mindset: the compiler found a bug for you |
| Thinking Rust is just stricter Java or Python | Rust's rules are structurally different, not just stricter | Ownership is a new concept — not found in most languages |
| Skipping the "why" | Jumping to syntax without understanding the motivation | Session 1 exists for a reason — the context helps the rules stick |
| Assuming errors are broken setups | Rust errors are usually rule violations, not environment issues | Read the error message before checking your setup |

---

## 8. Compiler Errors As Clues

> Session 1 is entirely conceptual — no code was written, so no compiler errors apply yet. This section previews what is coming.

| Future error | What it will mean | When you will see it |
|---|---|---|
| `cannot assign twice to immutable variable` | You tried to change a variable that is locked | Session 2 — variables |
| `value borrowed here after moved` | Ownership was transferred and you used the old variable | Session 3 — ownership |
| `cannot borrow as mutable` | You tried to change something without declaring it `mut` | Session 5 — structs |

---

## 9. Practice

> Session 1 has no code exercises. These are verbal exercises to cement the concepts.

### Practice 1 — In Your Own Words

**Prompt:** Explain what problem Rust solves that Python cannot.

**Check:** Python uses a garbage collector and catches memory errors at runtime. Rust catches them at compile time and does not need a GC. The result is both safety and C-level performance.

### Practice 2 — Motivation Check

**Prompt:** Why would a blockchain project choose Rust over a simpler language like JavaScript?

**Check:** Smart contracts handle real money. A bug in a deployed contract can be permanent and costly. Rust's compiler catches dangerous bugs before deployment — something JavaScript cannot guarantee.

### Practice 3 — Mindset Shift

**Prompt:** Describe the instructor's shift in mindset about the compiler.

**Check:** Stop seeing the compiler as an obstacle. Start seeing it as a teammate that finds real bugs before users do. When it rejects your code, it caught something.

---

## 10. Tiny Drills

Short repetitions. These should take 1-3 minutes each.

- Say the two problems Rust solves that most languages cannot
- Define memory bug in one sentence without looking
- Define concurrency in one sentence without looking
- Explain why Rust does not need a garbage collector
- Name the main reason Solana chose Rust
- Say what happens when the Rust compiler finds a problem

---

## 11. Active Recall

Answer without looking:

1. What two problems is Rust specifically designed to solve?
2. What does the Rust compiler do that most other language compilers do not?
3. What is a garbage collector, and why does Rust not need one?
4. What is a memory bug?
5. What is concurrency, and why is it dangerous?
6. Why would Solana choose Rust over a simpler language?
7. What is the instructor's main shift in mindset about the Rust compiler?
8. What is the difference between finding a bug at compile time and finding it in production?

---

## 12. Relevance To Solana & Blockchain

| Lesson idea | Where it appears in Solana/blockchain | Why it matters |
|---|---|---|
| Compiler safety | Anchor program compilation | Bugs are caught before deployment, not after |
| No garbage collector | Transaction throughput | Solana processes thousands of transactions per second — GC pauses would hurt |
| Concurrency safety | Parallel transaction execution | Solana runs many transactions at once safely |
| Ownership rules | Account access in programs | Only one writer to an account at a time |
| Smart contract bugs are hard to reverse | Deployed programs | Rust's compile-time checks reduce the chance of a critical bug reaching mainnet |

**Memory hook:** Rust's strictness is why Solana can be fast and safe at the same time.

---

## 13. Relevance To AI Agents

| Lesson idea | Where it appears in AI agents | Why it matters |
|---|---|---|
| Compiler safety | Agent program builds | Catch bugs in agent logic before running on-chain |
| No garbage collector | Agent latency | Agents that process payments need low latency |
| Concurrency safety | Agents running multiple tasks | Two agent tasks should not corrupt shared state |
| Ownership rules | Who controls agent memory | Clear rules about which part of the agent controls which data |
| Bugs hard to reverse | On-chain agent actions | An agent that sends a payment cannot unsend it |

**Memory hook:** An on-chain agent is a Rust program — its safety guarantees come from the same place Rust's do.

---

## 14. Transcript Coverage Check

| Transcript topic | Covered? | Where |
|---|---|---|
| Why Rust exists | Yes | Recap, key concepts |
| Compiler as safety net | Yes | Key concepts, confusions, practice |
| Memory bugs | Yes | Terms, recap |
| Concurrency bugs | Yes | Terms, recap |
| Garbage collector | Yes | Terms, confusions |
| Solana chose Rust | Yes | Recap, relevance sections |
| Session 1 is motivation only | Yes | Big idea, practice intro |
| Session 2 preview | Yes | Compiler errors as clues |

---

## 15. Final Study Checklist

- [ ] I can explain what problem Rust solves in plain English
- [ ] I know what the compiler does and why its errors are helpful
- [ ] I know what a memory bug is
- [ ] I know what concurrency is and why it causes bugs
- [ ] I know why Rust does not need a garbage collector
- [ ] I can explain why Solana chose Rust
- [ ] I completed the verbal practice exercises
- [ ] I can explain the Solana/blockchain relevance
- [ ] I can explain the AI agent relevance

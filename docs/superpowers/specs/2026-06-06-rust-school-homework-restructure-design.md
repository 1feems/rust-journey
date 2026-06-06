---
title: "Rust School — Homework Restructure & Overview Link Fix"
date: 2026-06-06
status: approved
---

# Rust School — Homework Restructure & Overview Link Fix

## Problem

The `01-overview.md` Pages section in every session contains broken links pointing to `./lesson` and `./exercise` — paths that don't exist. The actual files are `02-lesson-recap.md` and `03-study-guide.md`. Link labels are also wrong ("Lesson notes", "Exercises").

Homework files live in a separate top-level `homework/session-N/` folder, disconnected from the session they belong to.

All homework files contain a "How This Connects to Solana" section that doesn't belong in Rust School content.

## Scope

Affects: `rust-journey` repo, `rust-school/` directory.
Sessions: 1–6 and 8. (Session 7 not yet built.)

## Changes

### 1. Move Homework Into Session Folders

Move each homework file from `rust-school/homework/session-N/homework.md` to `rust-school/lessons/session-N/04-homework.md`.

Sessions with homework: 2, 3, 4, 5, 6, 8. Session 1 has no homework.

Delete the `rust-school/homework/` folder after all files are moved.

### 2. Fix All Overview Pages Sections

Replace the Pages section in every `01-overview.md` with correct links and labels.

**Sessions 2–6, 8 (with homework):**
```markdown
## Pages

- [Lesson Recap →](./02-lesson-recap.md)
- [Study Guide →](./03-study-guide.md)
- [Homework →](./04-homework.md)
```

**Session 1 (no homework):**
```markdown
## Pages

- [Lesson Recap →](./02-lesson-recap.md)
- [Study Guide →](./03-study-guide.md)
```

### 3. Remove "How This Connects to Solana" From Homework Files

Remove the `## How This Connects to Solana` section and its body from all 6 homework files (sessions 2–6, 8) after moving them.

### 4. Update README.md

The README links sessions to `01-overview.md` files — these paths are correct and do not change. No README update needed unless homework was linked directly (confirm during build).

## Out of Scope

- Content edits to lesson recap or study guide files
- Session 7 (no transcript yet)
- rust-for-solana lessons

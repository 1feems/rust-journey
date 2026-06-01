# HANDOFF — rust-journey

> Read this before any session. Source of truth for this repo.

---

## What This Is

`rust-journey` is a public GitHub repo (`1feems/rust-journey`) — Feems' personal Rust learning portfolio. It is separate from `buddy-tech`. Content is written for a public audience of beginner learners.

Local path: `/Users/feems/Desktop/rust-journey`

---

## Current State

**Live on GitHub.** All sessions 1–6 complete. Pushed 2026-06-01.

---

## Folder Structure

```
rust-journey/
├── README.md                                    ✅
├── rust-school/
│   ├── README.md                                ✅ Landing page with lesson links
│   ├── lessons/
│   │   ├── session-1/
│   │   │   ├── 01-overview.md                   ✅
│   │   │   ├── 02-lesson-recap.md               ✅
│   │   │   └── 03-study-guide.md                ✅
│   │   ├── session-2/ ... session-6/            ✅ All complete
│   └── homework/
│       ├── session-2/homework.md                ✅
│       ├── session-3/homework.md                ✅
│       ├── session-4/homework.md                ✅
│       ├── session-5/homework.md                ✅
│       └── session-6/homework.md                ✅
├── vocabulary/
│   └── glossary.md                              ✅ Sessions 1–6 populated
├── exercises/
│   └── README.md                                ⏳ Placeholder
└── learning-notes/
    └── README.md                                ⏳ Placeholder
```

---

## Pipeline — How New Sessions Are Added

When the user drops a new Rust School transcript, build these 4 pages and push to this repo:

| # | File | Template (in buddy-tech) |
|---|---|---|
| 1 | `rust-school/lessons/session-N/01-overview.md` | `templates/OVERVIEW-TEMPLATE.md` |
| 2 | `rust-school/lessons/session-N/02-lesson-recap.md` | `templates/LESSON-RECAP-TEMPLATE.md` |
| 3 | `rust-school/lessons/session-N/03-study-guide.md` | `templates/RUST-SCHOOL-STUDY-GUIDE-TEMPLATE.md` |
| 4 | `rust-school/homework/session-N/homework.md` | `templates/HOMEWORK-TEMPLATE.md` |

Then update `rust-school/README.md` to add the new session link.

---

## What Still Needs Doing

| Task | Notes |
|---|---|
| Add homework answers | User adds their own answers directly to homework files as they complete them |
| Add exercises content | User adds practice exercises to `exercises/` as they create them |
| Add learning notes | User adds walkthrough notes to `learning-notes/` as they record them |

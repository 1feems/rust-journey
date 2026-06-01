# HANDOFF — rust-journey

> Read this before any session. Source of truth for this repo.

---

## What This Is

`rust-journey` is a public GitHub repo (`1feems/rust-journey`) — Feems' personal Rust learning portfolio. It is separate from `buddy-tech`. Content is written for a public audience of beginner learners.

---

## Current State

**Not yet pushed to GitHub.** Folder exists locally at `/Users/feems/Desktop/rust-journey/`.

---

## Folder Structure

```
rust-journey/
├── README.md                          ✅ Done
├── rust-school/
│   ├── session-1/
│   │   ├── 01-overview.md             ✅ Copied from buddy-tech
│   │   ├── 02-lesson-recap.md         ⏳ Placeholder — needs building from transcript
│   │   └── 03-study-guide.md          ✅ Copied from buddy-tech
│   ├── session-2/
│   │   ├── 01-overview.md             ✅
│   │   ├── 02-lesson-recap.md         ⏳ Placeholder — needs building from transcript
│   │   └── 03-study-guide.md          ✅
│   ├── session-3/
│   │   ├── 01-overview.md             ✅
│   │   ├── 02-lesson-recap.md         ⏳ Placeholder — needs building from transcript
│   │   └── 03-study-guide.md          ✅
│   ├── session-4/
│   │   ├── 01-overview.md             ✅
│   │   ├── 02-lesson-recap.md         ⏳ Placeholder — needs building from transcript
│   │   └── 03-study-guide.md          ✅
│   ├── session-5/
│   │   ├── 01-overview.md             ✅
│   │   ├── 02-lesson-recap.md         ✅ Done (fetched from GitHub)
│   │   └── 03-study-guide.md          ✅
│   └── session-6/
│       ├── 01-overview.md             ✅
│       ├── 02-lesson-recap.md         ⏳ Placeholder — needs building from transcript
│       └── 03-study-guide.md          ✅
├── homework/
│   ├── session-2/homework.md          ✅ Copied from buddy-tech
│   ├── session-3/homework.md          ✅
│   ├── session-4/homework.md          ✅
│   ├── session-5/homework.md          ✅
│   └── session-6/homework.md          ✅
├── vocabulary/
│   └── glossary.md                    ✅ Done — all 6 sessions populated
├── exercises/
│   └── README.md                      ⏳ Placeholder
└── learning-notes/
    └── README.md                      ⏳ Placeholder
```

---

## What Still Needs Doing

| Task | Notes |
|---|---|
| Build lesson recaps for sessions 1–4 and 6 | Use `templates/LESSON-RECAP-TEMPLATE.md` in buddy-tech. Needs transcript for each session. |
| Add user's homework answers | User fills these in as they complete homework |
| Push to GitHub as public repo | Run: `gh repo create 1feems/rust-journey --public` then push |
| Add homework answers section to each homework file | User adds their own answers directly |

---

## Templates (in buddy-tech)

| Template | File | Used for |
|---|---|---|
| Lesson Recap | `templates/LESSON-RECAP-TEMPLATE.md` | Building session lesson recaps |
| Rust School Study Guide | `templates/RUST-SCHOOL-STUDY-GUIDE-TEMPLATE.md` | Building study guides |
| Homework reference | `lessons/rust-school/session-5/05-homework.md` | Homework format |

---

## Next Session — Start Here

1. Build lesson recaps for sessions 1–4 and 6 (user to provide transcripts)
2. Push repo to GitHub: `cd /Users/feems/Desktop/rust-journey && git init && gh repo create 1feems/rust-journey --public --source=. --push`

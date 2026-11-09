# CLAUDE.md

Permanent working rules for this project. Read this first, every session.

---

## Project Overview

**What it is.** A public website showing exam proctoring duties for staff at
the School of Engineering & Technology, Al Hussein Technical University (HTU).
**Since November 2026 there is no Excel at all**: the page is published by the
Exam Committee program (OneDrive "Exam Committee Program", `server/staff_site.py`,
Exams page "Publish to staff"), which writes ONLY `index.html` here (schedule
locked inside) from its own database, commits and pushes. The old workbook
build (`build.py`, `sync.cmd`, `vba/`) was removed on 2026-11-09; it is in git
history only.

**Purpose.** A proctor opens the site on a phone and sees where they must be
and when. A course coordinator sees who is covering their exam.

**Platform / environment.**

| | |
|---|---|
| OS | Windows 11 |
| Data | the Exam Committee program (FastAPI + SQLite), not this repo |
| Hosting | GitHub Pages from `SETcommittee/Fall2026-2027proctors` (public repo; renamed from `SET-Exam-Proctors` on 2026-11-09) |
| Live site | https://setcommittee.github.io/Fall2026-2027proctors/ (the old `/SET-Exam-Proctors/` link no longer works) |

**Constraints that shape everything:**

- The data lives in the program, **outside this repository**, so publishing
  only runs on the committee's machine. Nothing rebuilds in CI.
- The repo and GitHub Pages are **public**, so the schedule is **locked**
  (2026-11-09): AES-256-GCM in `index.html` (`var SEALED`), key only in the
  link's fragment `#k=<base64url 32 bytes>` (`&p=<name>` for personal links),
  unlocked in the browser with WebCrypto (`boot()`/`unlock()` in the template).
  Never write a readable copy (no data.json / artifact.html) and never put the
  key in the repo. Key = program setting `staff_site_key`; "Make a new link"
  rotates it. `robots.txt` + `noindex` too. No emails, no exam notes.
- **HTU look** (2026-11-09), matching the program: HTU red #E8343F, ink
  #161613, ground #F9F9F9, Source Sans 3, square corners, red top strip, HTU
  logo. **Light-only by explicit request.** Do not reintroduce dark mode.
- This repo is deployed to real staff mid-exam-period. A broken push is visible
  to ~21 people within a minute.

---

## Source of Truth

In order of authority:

1. **Actual project files** — authoritative for how things are implemented.
2. **The Exam Committee program's database** — authoritative for exam,
   proctor and room **data**. It is not in this repo.
3. `PROJECT/REQUIREMENTS.md` — agreed requirements.
4. `PROJECT/ARCHITECTURE.md` — current architecture and the file map.
5. `PROJECT/DECISIONS.md` — permanent decisions and why.
6. `PROJECT/PROJECT_STATUS.md` — current state.
7. `PROJECT/TODO.md` — unfinished work.
8. `CHANGELOG/CHANGELOG.md` — history.

If a document disagrees with the files or the program, **investigate the
conflict**. Do not assume the document is right, and do not assume it is wrong.

---

## Context Loading Rules

**Do not reread the whole project for every task.**

On starting a session:

1. Read `CLAUDE.md` (this file).
2. Read `PROJECT/PROJECT_STATUS.md`.
3. Read `PROJECT/TODO.md`.
4. Read the most recent `CHANGELOG/CHANGELOG.md` entries.
5. Work out which requirements and decisions bear on the request.
6. Use the **Relevant File Map** in `ARCHITECTURE.md` to find the files that
   matter.
7. Inspect only those files.

Open unrelated or finished areas only when there is a specific reason to.

---

## Editing Rules

- Make the smallest change that correctly solves the task.
- Do not modify unrelated files.
- Preserve existing behaviour unless a change was asked for.
- Do not redesign the architecture without being asked.
- Do not add a second way to do something that already exists.
- Do not assume a change works. Verify it.
- Do not invent requirements.
- Ask when a decision is genuinely the user's to make.
- Match the surrounding code and prose style.
- Check who depends on a shared component before changing it.

**Specific to this project:**

- **Never edit `index.html` by hand.** It is
  written by the program. Edit `template.html` (the page) or the program's
  `server/staff_site.py` (the data), then publish from the program.
- Treat the program's data as the committee's live data. Never publish
  without the committee pressing Publish.

---

## Documentation Rules

After meaningful work:

- Update `PROJECT/PROJECT_STATUS.md`.
- Update `PROJECT/TODO.md` if the task list moved.
- Add a `CHANGELOG/CHANGELOG.md` entry.
- Update `PROJECT/ARCHITECTURE.md` if the architecture actually changed.
- Update `PROJECT/DECISIONS.md` when a lasting decision is taken.

Skip all of this for trivial changes that would add no useful information.
A routine data publish is not a documentation event.

---

## Resume Protocol

When the user says "continue the project", or returns after a gap:

1. Load context in the order under **Context Loading Rules**.
2. Read the current status.
3. Review unfinished tasks.
4. Review recent changes.
5. Pick the most relevant next task, or ask if it is ambiguous.
6. Inspect only the files that task needs.
7. Continue from where the project actually is.

Do not re-analyse the project from scratch.

---

## Verification Expectations

Say plainly which of these is true:

- "I implemented it" — the change is written.
- "I implemented and verified it" — the change is written **and** checked.

For this project, verification usually means one of:

- The program's preview (`/api/staff-site/preview`) shows the expected exams
  and seat counts.
- The page loads in a browser with **no console errors** and the three tabs work.
- The deployed site returns 200 and contains the expected change.

Never report work as finished on the strength of the code looking right.

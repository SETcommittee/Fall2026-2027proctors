# Exam Proctor Duty — SET

A one-page site so staff can look up their own proctoring duties, and course
coordinators can see who is covering their exams. It is published by the
**Exam Committee program** (Exams page, *Publish to staff*) from the
committee's own data. No Excel workbook is involved.

## For staff

Open the site and pick a tab:

- **My duties** — start typing your name (or hit *Browse all*) and pick yourself
  from the list. You get your duty count, hours, next duty, a day strip with
  your days highlighted, and an "Add to calendar" link per duty. Once you are
  selected the address bar holds your own link (`…/#k=…&p=Your%20Name`) — copy it
  and send it to that person so it opens straight to their duties.
- **By course** — coordinators type a course name (or their own name) and see
  who is proctoring each sitting, the rooms, and the student count.
- **Full schedule** — everything. Tap a day in the strip to jump to it, or
  search by course, coordinator, room or proctor name, filtered by department.

Names and course lists stay hidden until someone searches or asks to browse, so
the page does not open onto a wall of chips. Unfilled proctor places show as
**Not assigned yet**, and a banner at the top says whether every exam is fully
covered.

## Publishing

In the Exam Committee program, open **Exams** and press **Publish to staff**.
It shows what will go out (with a link to see the page first), then writes the
semester in view into this folder, commits and pushes. GitHub Pages redeploys
in about a minute. A publish with nothing new in the schedule makes no commit.

Not published: email addresses and the committee's notes on exams.

## Files

| File | What it is |
|---|---|
| `template.html` | The page: markup, styles and behaviour, with a `/*__DATA__*/ null` slot |
| `index.html` | Written by the program: the page with the schedule locked inside |
| `robots.txt` | Keeps search engines out (the page also carries `noindex`) |
| `.github/workflows/pages.yml` | Deploys to GitHub Pages on every push to `main` |

Never edit `index.html` by hand: change
`template.html` (the page) or the program (the data), then publish again.

## Privacy: the page is locked

The repository is public, so the schedule is published **encrypted**
(AES-256-GCM): `index.html` holds only scrambled text, and no readable copy
(`data.json`, `artifact.html`) is ever written. The key is in the link the
committee sends to staff, after `#k=`; browsers never send that part to the
server, so GitHub never sees it. The page unlocks the schedule in the browser.
Without the full link it says "This page is locked".

- Personal links: `.../#k=<key>&p=<name>`.
- If a link reaches the wrong people: the program's *Make a new link*, then
  Publish; every old link stops working.
- Search engines are kept out too (`robots.txt`, `noindex`).

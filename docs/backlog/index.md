# Backlog — Tiny Page

Planned work, in order. Each task has its own file with the goal, scope, acceptance criteria and
evidence.

**States:** `Not started` → `In progress` → `Done` (only with evidence in the task file).

| Order | Task | State | Depends on | PR |
| ----- | ---- | ----- | ---------- | -- |
| 1 | [t001 — Add the main page](t001.md) | Not started | — | — |
| 2 | [t002 — Add automatic tests and CI](t002.md) | Not started | t001 | — |
| 3 | [t003 — Add a click counter button](t003.md) | Not started | t002 | — |
| 4 | [t004 — Add a reset button and rename the counter button](t004.md) | Not started | t003 | — |
| 5 | [t005 — Change the subtitle to "A tiny page for learning GitHub."](t005.md) | Not started | t004 | — |
| 6 | [t007 — Change the subtitle to "Made one small step at a time."](t007.md) | Not started | t004 | — |
| 7 | [t006 — Improve the page style](t006.md) | Not started | t005 | — |

## How to add a task

1. Copy an existing task file and give it the next free id (`t008.md`, …).
2. Fill in the goal, scope, out of scope and acceptance criteria **before** any code.
3. Add a row to the table above.

---

## 🎓 Learning purpose

Tiny Page is tiny on purpose: the app is not the goal. Each task practises one GitHub skill.

| Task | What Matan learns | Who drives |
| ---- | ----------------- | ---------- |
| t001 | The whole task loop for the first time, slowly | Claude (Matan watches) |
| t002 | What a test and CI are; turning on "checks must pass" after the merge | Claude (Matan watches) |
| t003 | A red check on purpose: see the merge blocked, then fix it with a new commit | Matan, with Claude's help |
| t004 | A real review round: comments, replies, fixes, resolved conversations | Matan, with Claude's help |
| t005 + t007 | A merge conflict, fixed by hand (t007 is started after t005 but merged before it) | Matan |
| t006 | The whole task loop alone | Matan alone (Claude only reviews) |

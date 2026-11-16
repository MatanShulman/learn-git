# Backlog — Tiny Page

Planned work, in order. Each task has its own file with the goal, scope, acceptance criteria and
evidence.

**States:** `Not started` → `In progress` → `Done` (only with evidence in the task file).

**Parent and child tasks:** a row marked ↳ is a child of the parent task above it. A parent
(Kind: Aggregate) has no work of its own and is `Done` only when all its children are `Done`.

| Order | Task | State | Depends on | PR |
| ----- | ---- | ----- | ---------- | -- |
| — | [t008 — Adopt the full team method](t008.md) (parent of t008-01 … t008-05) | Not started | — | — |
| 1 | ↳ [t008-01 — Fix the docs after setup](t008-01.md) | Not started | — | — |
| 2 | ↳ [t008-02 — Set the backlog conventions](t008-02.md) | Not started | t008-01 | — |
| 3 | ↳ [t008-03 — Organise the project docs](t008-03.md) | Not started | t008-02 | — |
| 4 | ↳ [t008-04 — Write down the full delivery loop](t008-04.md) | Not started | t008-03 | — |
| 5 | ↳ [t008-05 — Add agent role skills](t008-05.md) | Not started | t008-04 | — |
| 6 | [t001 — Add the main page](t001.md) | Not started | t008 | — |
| 7 | [t002 — Add automatic tests and CI](t002.md) | Not started | t001 | — |
| 8 | [t003 — Add a click counter button](t003.md) | Not started | t002 | — |
| 9 | [t004 — Add a reset button and rename the counter button](t004.md) | Not started | t003 | — |
| 10 | [t005 — Change the subtitle to "A tiny page for learning GitHub."](t005.md) | Not started | t004 | — |
| 11 | [t007 — Change the subtitle to "Made one small step at a time."](t007.md) | Not started | t004 | — |
| 12 | [t006 — Improve the page style](t006.md) | Not started | t005 | — |

## How to add a task

1. Copy an existing task file and give it the next free id (`t009.md`, …).
2. Fill in the goal, scope, out of scope and acceptance criteria **before** any code.
3. Add a row to the table above.

---

## 🎓 Learning purpose

Tiny Page is tiny on purpose: the app is not the goal. Each task practises one GitHub skill.

| Task | What Matan learns | Who drives |
| ---- | ----------------- | ---------- |
| t008 | Splitting big work into a parent and children; the full team method, set up before the app tasks | Claude writes, Matan types the commands and reviews |
| t001 | The whole task loop on app code for the first time, slowly | Claude (Matan watches) |
| t002 | What a test and CI are; turning on "checks must pass" after the merge | Claude (Matan watches) |
| t003 | A red check on purpose: see the merge blocked, then fix it with a new commit | Matan, with Claude's help |
| t004 | A real review round: comments, replies, fixes, resolved conversations | Matan, with Claude's help |
| t005 + t007 | A merge conflict, fixed by hand (t007 is started after t005 but merged before it) | Matan |
| t006 | The whole task loop alone | Matan alone (Claude only reviews) |

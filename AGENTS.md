# AGENTS.md — rules for AI agents in this repo

Read this whole file before you do anything in this repo.

## What this repo is

- **Tiny Page**: a tiny web page (a title, a subtitle and a click counter) made with plain HTML,
  CSS and JavaScript.
- **The app is not the goal.** This is a learning repo. Matan (the owner) is learning a professional
  GitHub team workflow, "the team method" below, by doing it task by task.
- The repo itself must show the workflow: task files, pull requests (PRs), reviews, checks and the
  learning log. Someone will check the repo later to see if the workflow was followed.

## The team method

The big idea: **`main` always works and is protected. Nobody pushes to `main` directly.**
Every change goes through:

```text
task file → branch → small commits → push → pull request → automatic checks → review → fixes
→ squash merge → sync → check main is healthy
```

The step-by-step task loop, with commands, is in [`docs/workflow.md`](docs/workflow.md).

### The rules

1. **The repo is the source of truth.** Rules for agents live in this file. Planned work lives in
   `docs/backlog/` as one Markdown file per task (`t001.md`, `t002.md`, …) with a goal, scope,
   out of scope, acceptance criteria and a state.
2. **One task = one branch = one small PR.** No unrelated cleanups or "while I'm here" changes.
3. **Write the acceptance criteria before the code.** Each one is a small, checkable "it works
   when…" sentence, including at least one "it refuses / doesn't break when…" case.
4. **`main` is protected:** changes only come in through PRs, merges are **squash only**, branches
   are **deleted automatically after merge**, force-push and deleting `main` are blocked.
5. **Keep all history.** Every PR stays on GitHub forever as the record of *why* a change was made.
   Never force-push, never rewrite commits that are already pushed, never delete PRs. Fixes after
   review are **new commits** on the same branch.
6. **Check locally before pushing:** `npm test`, open the page, read your own `git diff`.
7. **CI (GitHub Actions) runs the same checks on GitHub** for every PR and every update of `main`.
8. **The reviewer must not be the author.** The reviewer can be Matan (on the GitHub web page),
   Matan's teacher, or a *fresh* agent session that only reviews. If the author checks their own
   work, call it **"self-review"**, not "review".
9. **Answer every review comment in its thread:** "Fixed in `<commit>`" / a disagreement with the
   reason / "partly fixed, this remains". Resolve a conversation only after the fix is actually
   pushed to the PR.
10. **Merge only when** the acceptance criteria are met, the checks are green **on the latest
    commit**, all conversations are resolved, and the task file is updated to `Done` with its
    evidence. And only with Matan's OK.
11. **After merge:** update local `main`, delete the local branch, and check that CI on `main` is
    green ("main health") before starting the next task. If `main` is red, stop and fix `main`
    first, in its own small PR.
12. **Agent attribution.** Commits written by an agent start the subject with the agent's name,
    e.g. `[Claude] Add a click counter button` (or `[Codex] …`). Any PR description or comment
    an agent posts through Matan's account starts with
    `> Agent notice: <agent name>, acting on behalf of the user.` (use your real agent name).
    Matan's own commits and comments don't need it.
13. **Task states:** `Not started` → `In progress` → `Done`. A task is `Done` only with recorded
    evidence: what was tested, the result (passed / failed / not run) and the PR.

The only commit that ever went straight to `main` is the first setup commit.

## Names and formats

- **Branch:** task id + short name, e.g. `t003-click-counter`.
- **Commit subject:** short, says what the commit does, e.g. `[Claude] Add the main page`.
- **PR title:** task id + task title, e.g. `t003 — Add a click counter button`.
- **PR description:** follow [`.github/pull_request_template.md`](.github/pull_request_template.md)
  (added in t002).

## Task files

Each task file in `docs/backlog/` reads like a real team's task:

- **On top, the real work:** title, state, depends on, goal, scope, out of scope, acceptance
  criteria, how to verify, evidence.
- **At the very end,** after a `---` line, a `## 🎓 Learning purpose` section: who drives, which
  GitHub skill the task practises, and any special steps (like a red check on purpose).
- Keep learning notes out of the goal, scope and acceptance criteria. The backlog index
  (`docs/backlog/index.md`) follows the same pattern.

## Project facts

- **Stack:** plain HTML, CSS and JavaScript. No framework, no build step, no npm dependencies.
- **Open the page:** double-click `index.html`. It must work from `file://`, so **don't use ES
  modules** (`<script type="module">`, `import`, `export`) in browser code: browsers block them on
  `file://`.
- **Testable logic lives in `logic.js`:** pure functions only (no DOM, no `window`, no
  `localStorage`). The page loads it as a normal `<script>` before `app.js`. Its last line exports
  the functions so Node's tests can load them:
  `if (typeof module !== "undefined") module.exports = { ... };`
- **`app.js`** connects `logic.js` to the page (DOM, clicks).
- **Tests:** `tests/logic.test.js` with Node's built-in test runner (`node:test`,
  `node:assert/strict`). Run them with `npm test`. Node 22.
- **CI:** `.github/workflows/checks.yml` runs `npm test` on every pull request and every push to
  `main`.
- **Line endings:** LF, set by `.gitattributes`.

### Files

Some files don't exist yet. The task in brackets adds them.

```text
AGENTS.md                         rules for agents (this file)
CLAUDE.md                         one line: @AGENTS.md (so Claude Code reads this file too)
README.md                         what it is, how to open it, how to run the checks
index.html  styles.css            the page (t001)
logic.js                          pure functions, no DOM (t002)
app.js                            connects logic.js to the page (t003)
tests/logic.test.js               tests for logic.js (t002)
package.json                      no dependencies; "npm test" runs node --test (t002)
.gitignore  .gitattributes        files Git ignores / line endings
.github/workflows/checks.yml      CI (t002)
.github/pull_request_template.md  PR description template (t002)
docs/workflow.md                  Matan's cheat sheet: every step and command
docs/learning-log.md              one or two lines per PR: what Matan learned / what went wrong
docs/backlog/index.md             ordered list of tasks with links and states
docs/backlog/t001.md …            one file per task
```

## Working with Matan

- Matan is new to GitHub team work. Use **simple, beginner-friendly words**. Matan writes casually
  and with typos; that's fine.
- **One major step at a time.** Before each major step (commit, push, opening a PR, merge, changing
  GitHub settings, installing a tool), explain in 1–3 simple sentences what you'll do and why, show
  the exact command, and **wait for Matan's OK**. An OK for one step is not an OK for the next one.
  Read-only checks (`git status`, `git log`, reading files) don't need an OK.
- **Teach, don't just do.** The first time a new idea comes up, explain it in one short paragraph,
  with an everyday comparison if it helps.
- **Hand over step by step.** Each task file's 🎓 Learning purpose section says who drives: the
  agent, Matan with guidance, or Matan alone.
- **Never fake results.** Always say whether a check **passed**, **failed** or was **not run**.
  Never say "CI passed" unless you saw it pass on the latest commit.
- **Check before you use a tool.** Make sure a command exists on Matan's machine (Windows 10,
  VS Code, Git Bash). Don't invent commands, settings or results.
- **Start of a session:** the repo is switched to public (it's private between sessions), then do
  "main health" (step 0 of the task loop).
- **End of a session:** give a short numbered summary (what we did, what comes next), then the
  repo is switched to private. Switching visibility is a settings change, so it needs Matan's OK.
- **Privacy:** don't write real names of other people in the repo (write "Matan's teacher").
  Commits use Matan's GitHub noreply email, set in this repo's local Git config.

## Safety rules for agents

- **Never commit or push directly to `main`.**
- **Never force-push**, never rewrite pushed history, never delete the repo, PRs or other people's
  branches.
- **Never merge, change repo settings, or post on GitHub without Matan's OK** in that moment.
- Don't commit secrets, tokens or files from Matan's machine that don't belong to the project.
- If something fails, show the real error, explain it simply and suggest a fix. Don't hide it.
- While the repo is private, GitHub's free plan doesn't enforce the protection on `main`. Follow
  the rules anyway.

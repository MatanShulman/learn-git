# Workflow — how we work

Every change goes through the same steps. `main` always works, and nobody pushes to it directly.

```text
task file → branch → small commits → push → pull request → automatic checks → review → fixes
→ squash merge → sync → check main is healthy
```

The rules behind these steps are in [`AGENTS.md`](../AGENTS.md).

## Start of a session

1. Make the repo public (it's private between sessions):

   ```bash
   gh repo edit --visibility public --accept-visibility-change-consequences
   ```

   Or on GitHub: **Settings → General → Danger Zone → Change visibility**.
2. Do step 0 of the task loop (main health).

## The task loop

### 0. Main health

```bash
git switch main
git pull
gh run list --branch main --limit 3
```

- The latest run must be ✅ (`completed`, `success`).
- `in_progress` means it's still running: wait and check again.
- ❌ means stop: fix `main` first, in its own small PR.

Before t002 there is no CI, so the list is empty. That's expected.

### 1. Readiness

Open the task file in `docs/backlog/` and check:

- The goal, scope and acceptance criteria are clear.
- The tasks it depends on are `Done`.

If not, fix the task file first (that can be its own small PR).

### 2. Branch

```bash
git switch -c t003-click-counter
```

Name it: task id + short name. First commit on the branch: set the task's state to `In progress`
(in the task file and in `docs/backlog/index.md`).

### 3. Implement in small commits

```bash
git status                  # what changed?
git add index.html          # pick the files for the next commit
git diff --staged           # read exactly what will be committed
git commit -m "Add the click counter button"
```

One commit = one small, clear step. Commits written by an agent start with `[Claude]` (or
`[Codex]`).

### 4. Local checks (before pushing)

```bash
npm test                    # all tests must pass (from t002 on)
git diff main...HEAD        # read the whole change, like a reviewer would
```

Then double-click `index.html` and try every acceptance criterion by hand.

### 5. Push

```bash
git push -u origin t003-click-counter
```

`-u` links your branch to the one on GitHub, so next time `git push` is enough.

### 6. Open the pull request

```bash
gh pr create --web
```

The browser opens with the PR template already filled in. Title: task id + task title, e.g.
`t003 — Add a click counter button`. Fill in every part of the template.

Not ready for review yet? Open it as a **draft** (`gh pr create --draft`), and later mark it
ready with `gh pr ready`.

### 7. Watch the checks

```bash
gh pr checks --watch
```

The same checks are on the PR page, at the bottom of the **Conversation** tab.

### 8. Review

Someone who is **not the author** reviews the PR:

```bash
gh pr view --web            # open the PR in the browser
```

On the PR page → **Files changed** → click the `+` next to a line → write a comment.
If the author checks their own work, it's called **self-review**, not review.

### 9. Answer the review

- Reply in **each** thread: "Fixed in `abc1234`" / a disagreement with the reason /
  "partly fixed, this remains".
- Fix with **new commits** on the same branch, then `git push`.
- Resolve a conversation only after its fix is pushed.
- Ask for another review.

### 10. Before merging

1. Task file: state `Done`, tick the acceptance criteria, fill in **Evidence** (commands run,
   results: passed / failed / not run, PR link).
2. `docs/backlog/index.md`: state `Done` and the PR link.
3. `docs/learning-log.md`: one or two lines about this PR.
4. Commit and push these in the same PR, then check the **latest** commit is green:

   ```bash
   gh pr checks
   ```

Merge only when: the acceptance criteria are met, the checks are green on the latest commit, all
conversations are resolved, and the task file is `Done`.

### 11. Merge

```bash
gh pr merge --squash
```

All the PR's commits become **one** commit on `main`. The PR keeps the detailed history.
GitHub deletes the branch on GitHub by itself.

### 12. After the merge

```bash
git switch main
git pull
git fetch --prune                   # forget branches that were deleted on GitHub
git branch -d t003-click-counter    # delete the local branch
```

If `git branch -d` says **"not fully merged"**: that's normal after a squash merge (see the
learning notes below). Check that the PR is really merged:

```bash
gh pr view t003-click-counter       # must say MERGED
git branch -D t003-click-counter    # only after you saw MERGED
```

Then go back to step 0 (main health).

## End of a session

1. A short numbered summary: what we did, what comes next.
2. Make the repo private:

   ```bash
   gh repo edit --visibility private --accept-visibility-change-consequences
   ```

## Repo settings

On the repo page → **Settings**:

| Setting | Where | Value |
| ------- | ----- | ----- |
| Squash merging only | General → Pull Requests | ✅ Allow squash merging. ❌ Merge commits. ❌ Rebase merging. |
| Delete branches after merge | General → Pull Requests | ✅ Automatically delete head branches |
| Protect `main` (ruleset) | Rules → Rulesets | Require a pull request (0 approvals), block force pushes, block deletion, require conversation resolution, require status checks (after t002) |
| Visibility | General → Danger Zone | Public while working, private between sessions |

## Starting a new project

1. Make the folder a repo: `git init -b main`
2. Keep your email private:
   - In this repo:
     `git config user.email "<id>+<username>@users.noreply.github.com"`
     (find the id with `gh api user --jq .id`).
   - Once per account: GitHub → profile picture → **Settings → Emails** → tick
     **Keep my email addresses private**.
3. Write the first files: `AGENTS.md`, `CLAUDE.md`, `README.md`, `.gitignore`, the backlog.
4. First commit. It's the only one that goes straight to `main`.
5. Create the GitHub repo and push: `gh repo create <name> --public --source . --push`
6. Set the repo settings (table above).

---

## 🎓 Learning notes

### Words

| Word | What it means | Like… |
| ---- | ------------- | ----- |
| Repo | A folder plus the full history of its files | A notebook that remembers every page you ever wrote |
| Commit | A saved snapshot of your changes, with a message | A save point in a game |
| Branch | A separate line of work | A draft copy you can change freely |
| `main` | The branch that always works | The printed book |
| Remote / `origin` | The copy of the repo on GitHub | The cloud backup |
| Push / pull | Send your commits to GitHub / get new commits from GitHub | Upload / download |
| Pull request (PR) | A request to merge a branch into `main`, with a page for discussion | "Please check my draft" |
| Review | Someone else checks the PR and comments | A second pair of eyes |
| CI | GitHub runs the tests by itself on every PR and on `main` | An inspector at the factory door |
| Squash merge | All the PR's commits become one commit on `main` | Stapling all the drafts into one final page |
| Conflict | Two branches changed the same line, so Git asks you to choose | Two people editing the same sentence |
| Ruleset | Rules on GitHub that protect `main` | A guard at the door |
| Force-push | Overwriting history on GitHub. We never do it | Tearing pages out of the notebook |

### Common surprises

- **`git branch -d` says "not fully merged" after a squash merge.** The squash made a *new*
  commit on `main`, so Git can't find your branch's own commits there and thinks they're not
  merged. If the PR says MERGED, the work is safe on `main` and `-D` is fine.
- **A red ❌ on the PR.** A check failed. Click **Details** to read the error, fix it with a new
  commit, and push.
- **Conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`). Git shows both versions in the file.
  Edit the file to the final text, delete the markers, then `git add` and `git commit`.
- **Warnings about LF / CRLF.** They're about line endings (how a line break is saved).
  `.gitattributes` keeps them the same everywhere. The warning is harmless.

# How to Use GitHub for Version Control

A short, shared playbook so the team works the same way. For project-specific
branch names and the capability workflow, see [`CONTRIBUTING.md`](../CONTRIBUTING.md).

## The golden rules

- **Never commit directly to `main`.** Always branch, then open a Pull Request.
- **One branch = one focused change.** Best to keep things small.
- **Pull before you branch**, and sync often, so you start from the latest code.
- **Never force-push a shared branch** (`main` or anything in an open PR).
- **Never commit secrets** (API keys, `.env`). If you do, rotate the key — deleting it later doesn't remove it from history.

## First-time setup

```bash
git clone git@github.com:WoRnVeLcRo/cyber-agent-project.git
cd cyber-agent-project
git config user.name  "Your Name"
git config user.email "you@example.com"
```

## The daily loop

```bash
# 1. Start from fresh main
git checkout main
git pull

# 2. Branch for your task  (naming: feat/<phase>-<tool>, see CONTRIBUTING.md)
git checkout -b feat/enum-nikto

# 3. Work, then stage + commit in small logical chunks
git add kali_agent/capabilities/scanning_enumeration/nikto.py
git commit -m "Add nikto web-scan capability"

# 4. Push your branch (first push sets the upstream)
git push -u origin feat/enum-nikto
# later pushes are just: git push

# 5. Open a Pull Request on GitHub  ->  review  ->  merge into main
```

## Writing good commits

- **Imperative mood, ~50 chars:** "Add redis exploit check", not "added stuff".
- **One concern per commit** — don't mix a bug fix with a refactor.
- Add a body (blank line, then details) when the *why* isn't obvious.

```
Add nikto web-scan capability

Wires nikto into the enumeration phase: capability module, Flask route,
policy allowlist, and controller tool schema.
```

## Pull Requests

- Title says what changes; description says **why** and **how to test**.
- Keep them small and self-contained; link the related `ROADMAP.md` item.
- Resolve review comments, then **Squash and merge** to keep `main` history clean.
- Delete the branch after merge (GitHub offers a button).

### What "Squash and merge" means

It flattens **all the commits on your branch into a single commit** on `main`, so
the messy work-in-progress history stays off the main timeline. Your branch's
commits:

```
feat/exploit-redis
●  Add redis capability skeleton
●  Wire route + policy allowlist
●  fix typo in port parsing
●  address review comments
```

...become one clean commit when merged:

```
main
●  Add redis exploit capability (#12)   <- all 4 changes, squashed into one
●  Add roadmap and contributing guide
```

So `git log --oneline` on `main` reads like a changelog (one line per feature)
instead of "wip / fix typo / address review comments". Pick it from the dropdown
on the green merge button.

## Staying in sync

If `main` moved while you worked, update your branch before merging:

```bash
git checkout feat/enum-nikto
git fetch origin
git merge origin/main          # resolve any conflicts, commit, push
```

> Prefer `merge` over `rebase` for shared branches — it's safer for the team and
> avoids rewriting history others may have pulled.

## Oops — quick fixes

| Situation | Command |
|-----------|---------|
| Unstage a file (keep edits) | `git restore --staged <file>` |
| Discard local edits to a file | `git restore <file>` |
| Fix the last commit's message | `git commit --amend` *(only if not pushed)* |
| See what changed | `git status` · `git diff` |
| Undo a pushed commit safely | `git revert <commit>` *(makes a new undo commit)* |

## Command cheat sheet

```bash
git status                 # what's changed / staged
git diff                   # unstaged changes
git log --oneline -10      # recent history
git branch -vv             # local branches + tracking
git checkout <branch>      # switch branches
git pull                   # fetch + merge current branch
```

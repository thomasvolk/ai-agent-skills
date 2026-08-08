---
name: commit-push
description: Runs the `commit` skill to create a commit as the current git user, then pushes the branch. Use when the user runs `/commit-push` or asks to commit and push in one step.
allowed-tools: Bash, Skill
---

# commit-push — commit as the current user, then push

You are running the `commit-push` skill. It delegates the commit itself to the [[commit]] skill, then pushes the resulting commit(s) to the remote.

## 1. Run the commit skill

Invoke the `commit` skill (via the `Skill` tool) to stage and commit the current changes. Follow its rules as written — plain commit as the current git user, no AI co-author trailer, no destructive operations, no `--no-verify`.

If `commit` reports there was nothing to commit, stop here and say so — do not push stale state as if something new happened.

## 2. Check what will be pushed

Before pushing, run:

- `git status` to confirm the branch is clean and the commit landed.
- `git log @{u}..HEAD --oneline` (or `git rev-parse --abbrev-ref --symbolic-full-name @{u}` first, if it's unclear whether an upstream is set) to see exactly which commits are about to be pushed — not just the one just created, since the branch may already have been ahead of the remote.

If the branch has no upstream configured, note that `-u` will be needed and confirm the target remote/branch with the user before proceeding, since that decision (which remote, which branch) shouldn't be assumed.

## 3. Push

- Push with `git push` (or `git push -u origin <branch>` if no upstream exists yet).
- Never use `--force` or `--force-with-lease` unless the user explicitly asks for it — if a normal push is rejected (e.g. non-fast-forward), stop and report it rather than forcing.
- Never push to `main`/`master` without the user's explicit go-ahead if that wasn't already clearly the intent of the request.

## 4. Report

State the commit(s) pushed and the remote/branch they went to. If the push failed, report the actual git error rather than retrying blindly.

## 5. Hard rules

- Do not skip Step 1 and push pre-existing local commits when the user's intent was to commit *and* push new work — but if `commit` finds nothing new to commit and there are already unpushed commits, ask before pushing those instead of assuming.
- Never rewrite history (`rebase`, `commit --amend`) as part of this skill.
- Never force-push, and never push to a remote/branch the user didn't ask for.

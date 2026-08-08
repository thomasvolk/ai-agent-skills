---
name: create-branch
description: Creates a git branch named from the given text, or derived from uncommitted changes if no text is given, asking the user if there's neither. Use when the user runs `/create-branch [description]` or asks to create/start a new branch.
allowed-tools: Bash, AskUserQuestion
---

# create-branch — name and create a branch from context

You are running the `create-branch` skill. It picks a branch name from, in order, the arguments text, the current uncommitted changes, or the user directly — then creates and switches to that branch.

## 1. Determine the name source

- **Arguments given** — use that text as the basis for the name; skip to Step 3.
- **No arguments** — run `git status` (never `-uall`) and `git diff HEAD`. If it shows any staged, unstaged, or untracked changes, read the diff and derive a short description of *why* the change was made (same judgment as drafting a commit message) — skip to Step 3.
- **No arguments and no changes** — ask the user what the branch is for via `AskUserQuestion`. Use their answer as the basis for the name.

## 2. Check for anything unexpected

If `git status` shows an in-progress merge/rebase, or other state that looks like someone else's unfinished work, stop and ask before touching it.

## 3. Slugify the name

Convert the basis text into `kebab-case`: lowercase, spaces and punctuation collapsed to single hyphens, no leading/trailing hyphens, no consecutive hyphens. Keep it short — aim for 3-6 words. Do not add a type prefix (`feature/`, `fix/`, …) unless the user's text already specifies one or this repo's `git branch -a` / `git log` history shows an established convention.

## 4. Check for collisions

Run `git branch --list <name>` (and `git branch -a --list <name>` for remotes). If it already exists, pick a more specific slug from the same basis text rather than silently appending a number — ask the user only if you can't disambiguate.

## 5. Create and switch

Run `git checkout -b <name>`. Uncommitted changes (if any) move with you onto the new branch — this is expected, not a conflict to resolve.

## 6. Report

State the new branch name and, if it was derived from uncommitted changes rather than explicit arguments, briefly note what in the diff drove the name so the user can correct it if it's off.

## 7. Hard rules

- Never commit, stage, or discard changes as part of this skill — it only creates and switches branches.
- Never delete or overwrite an existing branch to resolve a name collision.
- Never push the new branch — this skill is local-only.

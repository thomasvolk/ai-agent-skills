---
name: commit
description: Creates a git commit for the current changes, attributed solely to the current git user — no AI co-author trailer. Use when the user runs `/commit` or asks to commit changes as themselves, without Claude listed as a co-author.
allowed-tools: Bash
---

# commit — plain commit as the current user

You are running the `commit` skill. It stages and commits the current changes using the identity already configured in git (`user.name` / `user.email`) — the resulting commit reads as authored entirely by that person, with no `Co-Authored-By` trailer or other mention of AI involvement.

## 1. Gather context

Run in parallel:

- `git status` (never `-uall`) to see tracked/untracked files.
- `git diff` to see staged and unstaged changes.
- `git log -n 5 --oneline` to match this repo's commit message style.
- `git config user.name` and `git config user.email` to confirm whose identity the commit will carry.

## 2. Check for anything unexpected

If `git status` shows unfamiliar files, an in-progress merge/rebase, or other state that looks like someone else's unfinished work, stop and ask before touching it. Never run destructive commands (`reset --hard`, `checkout .`, `clean -f`) to clear the way.

## 3. Draft the message

Write a concise commit message (1–2 sentences, focused on *why* not *what*) that matches the repository's existing style from `git log`. Do not add a `Co-Authored-By` line or any other trailer naming an AI — this commit is meant to read as ordinary, user-authored work.

## 4. Stage and commit

- Stage specific files by name (never `git add -A` / `git add .`) so nothing accidental (secrets, build output) gets swept in.
- Before staging, skim for files that might contain secrets (`.env`, credentials) even if the name looks innocuous — check contents if unsure, and warn the user rather than committing them.
- Commit with the message via a heredoc so formatting survives:

```
git commit -m "$(cat <<'EOF'
<message>
EOF
)"
```

- Do not pass `--no-verify`, `--no-gpg-sign`, or `-c commit.gpgsign=false` unless the user explicitly asks for it.
- If a pre-commit hook fails, fix the underlying issue and create a **new** commit — never amend to paper over a hook failure.

## 5. Verify

Run `git log -1` and confirm the commit's author matches the `user.name`/`user.email` captured in Step 1, and that the message body carries no AI attribution.

## 6. Hard rules

- Never create commits the user didn't ask for, and never push — this skill only commits locally.
- Never amend an existing commit unless the user explicitly asks for it; create a new commit instead.
- If there is nothing staged and nothing to commit, say so plainly rather than inventing an empty commit.

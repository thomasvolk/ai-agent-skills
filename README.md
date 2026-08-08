# ai-agent-skills

A collection of [Claude Code](https://claude.com/claude-code) skills packaged as a plugin. Skills are reusable, agent-invocable workflows — this repo currently ships:

- **askme** (`skills/engineering/askme`) — interactively interrogates a statement, claim, idea, or plan via round-by-round clarifying questions until it's unambiguous, printing a confidence score after each round. Invoke it with `/askme <statement>`.
- **commit** (`skills/workflow/commit`) — creates a git commit for the current changes, attributed solely to the current git user with no AI co-author trailer. Invoke it with `/commit`.
- **commit-push** (`skills/workflow/commit-push`) — runs the `commit` skill, then pushes the resulting commit(s) to the remote. Invoke it with `/commit-push`.

The repository is also a Claude Code plugin marketplace (see `.claude-plugin/marketplace.json` and `.claude-plugin/plugin.json`), so it can be added straight to Claude Code without cloning it manually.

## Installation

### Option A: Install as a Claude Code plugin (recommended)

From inside Claude Code, add this repo as a plugin marketplace and install the plugin:

```
/plugin marketplace add thomasvolk/ai-agent-skills
/plugin install thomasvolk-skills@thomasvolk
```

This makes the `/askme` skill (and any skills added later) available in every session.

### Option B: Install via npx

Install the skills directly from GitHub using the [`skills`](https://www.npmjs.com/package/skills) CLI:

```
npx skills add thomasvolk/ai-agent-skills
```

## Repository layout

```
.claude-plugin/       Plugin and marketplace manifests
skills/                Skill definitions, grouped by category
agent.sh               Launches Claude Code with this repo as a local plugin dir
```

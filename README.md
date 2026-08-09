# ai-agent-skills

A collection of [Claude Code](https://claude.com/claude-code) skills packaged as a plugin. Skills are reusable, agent-invocable workflows — this repo currently ships:

- **askme** (`skills/engineering/askme`) — interactively interrogates a statement, claim, idea, or plan via round-by-round clarifying questions until it's unambiguous, printing a confidence score after each round. Invoke it with `/askme <statement>`.
- **document** (`skills/engineering/document`) — creates a user-facing documentation page for a given topic at `docs/<topic>.md`. Invoke it with `/document <topic>`.
- **implement-epic** (`skills/engineering/implement-epic`) — implements an epic from its technical specification using TDD, then delegates final QA to `verify-epic`. Invoke it with `/implement-epic <epic-id>`.
- **readme** (`skills/engineering/readme`) — creates or updates a project's README.md, covering what it does, dependencies, related projects, install/usage instructions, and a link to the license. Invoke it with `/readme`.
- **refine-epic** (`skills/engineering/refine-epic`) — iteratively refines an epic PRD with open Q&A until confidence reaches 90%, creating the PRD if it doesn't exist. Invoke it with `/refine-epic <epic-id>`.
- **refine-spec** (`skills/engineering/refine-spec`) — iteratively refines a technical specification with open Q&A until confidence reaches 90%, creating the spec from the PRD if it doesn't exist. Invoke it with `/refine-spec <epic-id>`.
- **roadmap** (`skills/engineering/roadmap`) — reads `specs/briefing.md` and produces a requirement-only epic breakdown with resolved dependencies and parallel work identified. Invoke it with `/roadmap [max-epics]`.
- **verify-epic** (`skills/engineering/verify-epic`) — runs an epic's tests and reports acceptance-criteria coverage. Invoke it with `/verify-epic <epic-id>`.
- **commit** (`skills/workflow/commit`) — creates a git commit for the current changes, attributed solely to the current git user with no AI co-author trailer. Invoke it with `/commit`.
- **commit-push** (`skills/workflow/commit-push`) — runs the `commit` skill, then pushes the resulting commit(s) to the remote. Invoke it with `/commit-push`.
- **create-branch** (`skills/workflow/create-branch`) — creates a git branch named from given text, uncommitted changes, or the user directly if neither is available. Invoke it with `/create-branch [description]`.

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

# CLAUDE.md

## Registering new skills

Whenever a new skill is created under `skills/<category>/<name>/SKILL.md` in this repo, as soon as `SKILL.md` is written:

- Add its path to the `"skills"` array in `.claude-plugin/plugin.json`, as `"./skills/<category>/<name>"`.
- Add a matching bullet to the skill list in `README.md`, following the existing format:
  `- **name** (\`skills/<category>/<name>\`) — description. Invoke it with \`/command\`.`

Do this automatically, without asking for confirmation. Keep both files in sync going forward: if a skill's name, description, path, or invocation changes, or a skill is removed, update or remove the corresponding entries in both files the same way.

This only covers skills created by Claude Code in-session — it won't catch a skill folder added by hand outside of a session.

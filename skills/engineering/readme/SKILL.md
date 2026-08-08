---
name: readme
description: Creates or updates a project's README.md — what the software does, its dependencies, links to related projects, install/usage instructions, and a link to the license. Use when the user asks to write, generate, create, or update a README file.
---

# readme — write or update a project's README.md

You are running the `readme` skill. It produces a README.md that a newcomer can read to understand what the project does, what it depends on, how to install and use it, and where to find related projects and the license — no more, no less.

## 1. Survey the project

Read in parallel:

- Any existing `README.md` — preserve accurate content and structure already there rather than starting from scratch.
- The manifest that declares dependencies for this stack (`package.json`, `pyproject.toml`/`requirements.txt`, `Cargo.toml`, `go.mod`, `pom.xml`/`build.gradle`, `Gemfile`, etc.) — whichever exists.
- Entry-point source files (`main.*`, `index.*`, `cli.*`) and any existing docs/comments to understand what the software actually does.
- The repo root for a `LICENSE`, `LICENSE.md`, or `COPYING` file.
- Author/contributor info: an `AUTHORS`/`CONTRIBUTORS` file, an `author`/`contributors` field in the manifest, or `git log` for committer names if nothing else exists.

If a manifest or license file is missing, don't invent one — say so and skip that section's content rather than guessing.

## 2. Find related projects

Check for pointers to related projects: links in existing docs, a `homepage`/`repository` manifest field pointing elsewhere, forks/upstreams named in git remotes (`git remote -v`), sibling packages in a monorepo, or libraries/frameworks this project is built on top of or extends. Only list projects you can point to concretely — don't fabricate a "related projects" section if none exist.

## 3. Resolve ambiguity

If after steps 1–2 something material is still unclear or contradictory — e.g. conflicting descriptions of what the software does, an ambiguous install target, multiple candidate license files, unclear authorship — invoke the `askme` skill on the specific ambiguous point rather than guessing or silently picking an interpretation. Don't invoke it for minor stylistic choices you can reasonably decide yourself.

## 4. Write or update README.md

Produce (or update in place, preserving accurate existing content) these sections, in order:

1. **Title + one-paragraph description** — what the software does and what it's good for, in plain language a newcomer understands without prior context.
2. **Dependencies** — every dependency required to *use* it (runtime dependencies, required services, minimum language/runtime version). Pull names and versions from the manifest rather than guessing.
3. **Installation** — the exact commands to install it, taken from the manifest's package manager (`npm install`, `pip install -r requirements.txt`, `cargo build`, etc.) or existing docs.
4. **Usage** — a minimal working example: the command to run it, or the smallest code snippet that exercises it.
5. **Related Projects** — links found in step 2. Omit the section entirely if none exist.
6. **Authors** — names/handles found in an `AUTHORS`/`CONTRIBUTORS` file or manifest field. Only fall back to `git log` committers if nothing else exists, and only list names, not credential-bearing detail. Omit the section entirely if none exist.
7. **License** — a link to the license file found in step 1 (e.g. `[MIT](LICENSE)`). If no license file exists, omit the section rather than claiming one.

## 5. Verify

Re-read the written file and confirm: every dependency named actually appears in the manifest, every install/usage command is one you saw in the project (not invented), and the license link points at a file that actually exists.

## Hard rules

- Never claim a license, dependency, related project, or author that isn't backed by something in the repo.
- Don't restate implementation details a reader doesn't need (internal file layout, private functions) — a README is for someone deciding whether and how to use the project, not for someone reading the source.

# brain.md

A **persistent memory layer for your projects** — a `BRAIN.md` protocol plus an install-first, agent-agnostic toolkit that captures a project's durable decisions, requirements, and constraints as plain Markdown in the repository.

This repository is the **toolkit**, not a brain itself. You install it once, then run `brain-setup` inside any project: that scaffolds a [`BRAIN.md`](./skills/brain-setup/assets/BRAIN.md) and a brain directory into **your** project. A coding agent (Claude Code / Codex / anything else) then learns how to work with that brain simply by reading the project's `BRAIN.md` — with **zero runtime dependencies**: no service, no MCP server, no npm install.

## How to use it

1. **Install the tools once (global):**

   ```bash
   ./setup        # symlinks skills/ into every detected agent (~/.claude/skills, ~/.codex/skills, …)
   ./uninstall    # reverses it cleanly; never touches any project's brain data
   ```

2. **Initialize a project:** run the **brain-setup** skill in that project. It scaffolds `BRAIN.md` + the brain skeleton, wires the chosen agents' config files, and can install a pre-commit hook.

3. **Seed real knowledge:** run the **brain-bootstrap** skill. On an existing project it reads the code, docs, and `git log` to draft the root pages and capture key decisions; on a near-empty project it interviews you to seed them. (brain-setup leaves the brain empty on purpose — seeding is a separate, user-controlled step.)

4. **Work as usual.** The agent reads and writes the brain only through the `brain` CLI, following `BRAIN.md` — brain files are never hand-edited.

5. **The `brain` CLI** (zero npm dependencies, run with `node`):

   ```bash
   brain() { node skills/brain-page/bin/brain.mjs "$@"; }
   brain brain-dir                              # where is the brain?
   brain list-pages                             # list pages
   brain read-page my-decision                  # read a page
   brain create-page --id my-decision --category decision --title "Use X over Y"
   echo "the new understanding" | brain update-truth --id my-decision --summary "why it changed"
   brain append-timeline --id my-decision --kind evidence --summary "benchmark confirmed it"
   echo "## Overview …" | brain update-root architecture
   brain wire --agent claude-code,codex         # wire CLAUDE.md / AGENTS.md to BRAIN.md
   brain reindex && brain lint-links
   ```

---

<sub>brain.md is led and incubated by **MindMux**, the standalone open-source landing of MindMux's Brain Spec + coding-agent adapter. The specification layer uses neutral naming so it can be adopted widely; stewardship and maintenance belong to MindMux.</sub>

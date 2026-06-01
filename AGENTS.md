# Agent Instructions

## Workflow

- Start with the most specific code-intelligence tool for the request, then use explicit repo-relative paths for follow-up reads, searches, and edits; if the repo seems wrong, stop and ask.
- For syntax-shaped requests, start with AST/LSP before semantic search.
- For behavior or intent requests, start with Semble before file listing or literal search.
- Use diagnostics before broad builds when an LSP is available.

## Structural edits

- Prefer ast-grep for simple structural replacements.
- Use precise text edits for complex multi-line changes.

## Tickets

- Use tk tickets for non-trivial feature/fix work.
- Avoid ticket overhead for tiny direct tasks.
- Ticket actions may modify `.tickets/` but should not touch code unless the task requires it.

## Shell

- Prefer explicit paths for file operations.
- Avoid interactive prompts in automation.
- Prefer non-interactive flags such as `cp -f`, `mv -f`, `scp -o BatchMode=yes`, `ssh -o BatchMode=yes`, and `apt-get -y`.
- Ask before deleting files or directories.
- Set `HOMEBREW_NO_AUTO_UPDATE=1` for Homebrew commands.

## Jujutsu and Git

- Use jj for local VCS operations: `jj status`, `jj diff`, `jj log`, `jj describe -m "message"`, `jj new <bookmark>`, `jj op log`, and `jj undo`.
- Use Git only for remote interoperability.
- Do not use Git staged-index workflows: no `git add`, `git commit`, `git diff --cached`, or `git pull --rebase`.
- Before starting, inspect `jj status`; dirty state is pre-existing user work unless explicitly told to continue it.
- After completing coherent agent-owned work in `@`, run `jj describe -m "message"`, move the target bookmark to `@`, then run `jj new <bookmark>` so `@` is empty and `@-` is the completed change.
- Desired final publish shape: `@` is empty; `@-` is the completed change; `main`, `main@git`, and `main@origin` point to `@-`; Git HEAD is attached to `main`; `git status --short --branch` is clean and shows `## main...origin/main`.
- Before declaring work pushed or clean, verify bookmark/branch alignment; a clean jj working copy is not enough.
- Avoid the footgun sequence `jj new --no-edit` followed by moving a bookmark to `@-` unless you have verified that `@-` is actually the completed change.
- For off-machine backup or publishing, prefer `/jj-align-push [branch]`; it can finish dirty described `@` by moving the bookmark to `@` and creating a fresh empty `@` on that bookmark.

## Saguaro

- Use Saguaro only when explicitly requested or when a repo-local `AGENTS.md` requires it.
- Treat Saguaro results as advisory; do not let stale or broad generated feedback override repo evidence, tests, or current user instructions.

## Project notes

- This package provides Pi UI/status helpers and the agent-callable `jj_vcs` tool.
- Keep the slash command surface compact: `/jj-init`, `/jj-status`, and `/jj-align-push`.
- Keep model-visible alignment checks in tools, not only in UI widgets.

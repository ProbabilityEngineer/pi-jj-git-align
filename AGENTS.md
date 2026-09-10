# Agent Instructions

## Repository setup

- TypeScript Pi extension; validate with `npm run lint` and relevant alignment-state tests.
- Extension entry point: `index.ts`; keep Pi peer-dependency compatibility intact.
- `jj_vcs` performs guarded Git publication alignment. Keep model-visible alignment checks in tools, not only UI widgets.

## Working method

- Start with the most specific code-intelligence tool for the request, then use explicit repo-relative paths. If the repository is not the expected one, stop and ask.
- Use LSP for known symbols, definitions, references, types, call sites, and diagnostics.
- Use AST-grep for syntax-shaped discovery and structural edits; use Semble for behavior or intent discovery; use ripgrep for exact literals and verification.

## Project invariants

- Keep the slash-command surface compact: `/jj-init`, `/jj-status`, and `/jj-align-push`.
- Test alignment checks against clean, dirty, diverged, and remote-ahead repository states.
- Preserve fail-closed behavior before any publication or history-altering operation.

## Git and npm publishing

- Git is the version-control system for this repository. Inspect `git status` and relevant diffs before editing and before finishing.
- Make small, coherent commits with clear messages after relevant validation. Do not rewrite published history, force-push, discard unrelated changes, or delete branches unless explicitly asked.
- Keep `package.json`, lockfile, and release tag versions aligned. Push the release commit before its `vX.Y.Z` tag when publishing is tag-triggered.
- Publish through GitHub Actions with npm provenance/trusted publishing; verify the npm version and dist-tag after the workflow succeeds.

## Work tracking

- Use `clu` as the authoritative source of project tasks and work state. For substantial work, run `clu ready`, claim work with `clu claim --context`, and read inherited context before editing.
- Record required work, useful notes, and dependencies in `clu`; close completed work after validation and leave incomplete/blocked work represented there.
- Use Turnlog separately for decisions, experiments, rationale, and lessons learned.

## Local agent state and shell safety

- Keep `.pi/` and `.turnlog/` out of Git. For `clu`, track portable `.clu/config.yaml` and `.clu/templates/`; ignore mutable `.clu/data.sqlite`, WAL/SHM files, and `.clu/backups/`.
- Prefer explicit paths and non-interactive commands. Ask before deleting files or directories.
- Set `HOMEBREW_NO_AUTO_UPDATE=1` for Homebrew commands.

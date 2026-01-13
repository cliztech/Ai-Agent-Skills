---
name: conventional-commit-writer
description: Drafts clear Conventional Commit messages from short change summaries, choosing the right type/scope, concise subjects, and optional body/footers for breaking changes or issue links.
source: local/skills
license: MIT
---

# Conventional Commit Writer

Turns a brief change summary into a well-formed Conventional Commit message ready for `git commit`.

## When to Use
- A user provides a short description of changes and wants a commit message.
- Ensuring commit headers stay under 72 characters and follow the `<type>[scope]: subject` format.
- Surfacing breaking changes, linked issues, or chore-level maintenance work.

## Fast Path (default)
1. **Collect**: summary, component/package (scope), breaking changes, issue IDs (e.g., `#123`), and whether tests were added.
2. **Choose type** (fallback to `chore` if unclear):
   - `feat` new capability; `fix` bug/regression; `refactor` non-functional code change; `perf` performance; `test` tests added/updated; `docs` docs only; `build` build tooling/deps; `ci` pipelines; `style` formatting/naming only; `chore` upkeep; `revert` rollback.
3. **Set scope**: small, lowercase tag like `api`, `ui`, `auth`, `deps`, `tests`. Omit if unsure.
4. **Write subject**: imperative, present tense, lowercase start, no period, ≤72 chars (e.g., "add debounce to search field").
5. **Add body** (wrap ~72 chars) only if it clarifies *why* or notes side effects/tests.
6. **Footers**: `BREAKING CHANGE: ...` for breaking behavior; `Closes #123` (one per line) for linked issues; `Co-authored-by:` if provided.
7. **Return** the commit in a fenced code block. If input is ambiguous, ask up to 2 clarifying questions before finalizing.

## Template
```
<type>(<scope>): <subject>

<body - optional, wrapped>

BREAKING CHANGE: <details - optional>
Closes #<id>  # optional, one per line
```
Leave blank sections out instead of writing placeholders.

## Examples
- Input: "Added debounce to search results to reduce API spam, touched ui components, tests updated, closes 42"
  Output:
  ```
  feat(ui): add debounce to search results

  Reduce API calls when typing quickly; updated search component tests.

  Closes #42
  ```

- Input: "Removed legacy auth endpoints, clients must migrate"
  Output:
  ```
  refactor(api): drop legacy auth endpoints

  BREAKING CHANGE: Clients must migrate to v2 auth routes.
  ```

## Tips
- If the summary includes dependency bumps only, prefer `chore(deps)`.
- For partial fixes without full resolution, use `chore` plus note remaining work in body.
- Keep a single focus per commit; if multiple unrelated changes appear, suggest splitting before writing.

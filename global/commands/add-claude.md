# Add Claude

---
IMPORTANT: If the project is empty (no source files, no manifests), use `/setup-claude` instead.
---

Scan this project's codebase and customize every `.claude/` config file to match the actual tech stack, conventions, and patterns in use.

`CLAUDE.md` must be at the project root (`./CLAUDE.md`). All other config files live in `.claude/`.

## Phase 1: Scan

Check package manifests and config files to detect:
- **Language + framework**: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, `composer.json`, `build.gradle`, `pom.xml`, `Makefile`, `Dockerfile`
- **Package manager**: lockfile type (`package-lock.json` → npm, `pnpm-lock.yaml` → pnpm, `yarn.lock` → yarn, `bun.lockb` → bun)
- **Test framework**: `jest.config.*`, `vitest.config.*`, `pytest.ini`, `conftest.py`, `playwright.config.*`
- **Linter/formatter**: `.eslintrc.*`, `.prettierrc.*`, `biome.json`, `ruff.toml`, `tsconfig.json`
- **Architecture**: folder structure pattern (feature-based, layered, MVC, monorepo)
- **Monorepo**: `workspaces` in package.json, `pnpm-workspace.yaml`, `lerna.json`, `nx.json`, `turbo.json`, or multiple `package.json` at depth 2+
- **Database/ORM**: `prisma/schema.prisma`, `drizzle.config.*`, `alembic.ini`, `knexfile.*`, ORM in dependencies
- **API/auth dirs**: locate `api/`, `auth/`, `middleware/`, `routes/`, `controllers/` directories
- **Frontend**: any `.tsx`, `.jsx`, `.vue`, `.svelte`, `.css` files
- **Docs**: any `docs/`, `doc/` directory, or significant `.md` files beyond README
- **Commit style**: `git log --oneline -20`

## Phase 2: Confirm Stack

Present all findings at once using AskUserQuestion:

```
I scanned your project. Here's what I found:

**Language**: [language]
**Framework**: [framework + CSS + DB if applicable]
**Package manager**: [npm/pnpm/yarn/bun/pip/cargo/go]
**Test framework**: [jest/vitest/pytest/etc.]
**Linter/Formatter**: [eslint+prettier/biome/ruff/etc.]
**Architecture**: [layered/feature-based/monorepo/etc.]
**Source dirs**: [list]
**Test dirs**: [list]
**Has frontend**: [yes/no]
**Has database**: [yes/no — ORM/migration tool if yes]
**Has docs dir**: [yes/no]

Should I customize the .claude/ files based on this? (yes/no/corrections)
```

If a monorepo was detected, also ask: "Which packages/apps should I focus on?"

Incorporate any corrections before moving on.

## Phase 3: Review Changes One by One, Then Apply

Determine the full set of changes needed across the files below. Then use AskUserQuestion to walk through each proposed change individually — one question per file. For each, show the specific proposed change (exact new content, or "delete this file") and ask: apply, skip, or correct? The user confirms, rejects, or corrects each one. Do not apply anything yet.

Once all questions have been answered, apply every confirmed change in one pass.

### 3.1 — CLAUDE.md

Fill in each section. Remove sections that don't apply. Delete all `> REPLACE:` blocks.

- **Tech Stack**: language, framework, CSS, DB, key libraries
- **Architecture**: non-obvious architectural decisions only (skip if self-evident)
- **Structure**: high-level directory map (skip for simple single-module projects)
- **Commands**: actual run, build, test, lint, format, dev commands from detected manifests
- **Key Decisions**: WHY non-obvious choices were made — this is the most valuable section
- **Domain Knowledge**: terms or abbreviations that aren't obvious from the code

### 3.2 — settings.json

Update the `allow` permissions to match the actual package manager and scripts:
- Replace `npm run` with the actual package manager (`pnpm run`, `bun run`, `cargo`, `go test`, `python -m pytest`, `make`, etc.)
- Add allow rules for any project-specific scripts that Claude will commonly run
- Keep all `deny` rules unchanged — they are universal

### 3.3 — rules/code-quality.md

Only update if the project's existing code uses different naming patterns. Sample 5–10 source files to verify. If defaults match, leave unchanged.

### 3.4 — rules/testing.md

Only update if the detected test framework has specific idioms not covered by the defaults. Otherwise leave unchanged.

### 3.5 — rules/security.md

Update the `paths:` frontmatter to match actual directories:
- Replace `src/api/**`, `src/auth/**`, `src/middleware/**` with actual paths found
- Keep `**/routes/**` and `**/controllers/**` if those directories exist
- If no API/auth directories exist, remove the non-matching entries

### 3.6 — rules/error-handling.md

- **No backend**: delete this file
- **Backend exists**: update `paths:` frontmatter to match backend directories (same as security.md plus service/handler directories)

### 3.7 — rules/frontend.md

- **No frontend** (no `.tsx`, `.jsx`, `.vue`, `.svelte`, `.css`): delete this file
- **Frontend exists**: update Component Framework table to highlight what the project actually uses; update path patterns if non-standard directories

### 3.8 — rules/database.md

- **No database detected**: delete this file
- **Database detected**: keep it; update `paths:` frontmatter to match actual migration directory paths

### 3.9 — hooks/block-dangerous-commands.sh

Check the default branch name. If it's not `main` or `master`, update the regex pattern to match.

### 3.10 — agents/

- **agents/frontend-designer.md**: delete if no frontend files exist
- **agents/doc-reviewer.md**: delete if no `docs/`, `doc/`, or significant `.md` files beyond README

### Note on format-on-save.sh

No changes needed — the hook auto-detects formatters by checking for binaries and config files at runtime. Skip this file.

## Phase 4: Review (Silent Pass)

After applying all changes, verify:
- No `> REPLACE:` blocks remain in `CLAUDE.md`
- All YAML frontmatter is valid
- `settings.json` allow rules cover the commands actually used in CLAUDE.md
- Security rule paths match where sensitive code actually lives
- No file contradicts another
- Hook scripts referenced in settings.json exist at `.claude/hooks/`

Only surface issues to the user if something needs fixing.

## Phase 5: Summary

```
Done. Here's what changed:

- CLAUDE.md: [summary of sections filled]
- settings.json: [package manager + added rules]
- rules/security.md: [path changes or "unchanged"]
- rules/error-handling.md: [path changes / removed / "unchanged"]
- rules/frontend.md: [kept with updates / removed]
- rules/database.md: [kept with updates / removed]
- rules/code-quality.md: [naming convention changes or "unchanged"]
- rules/testing.md: [framework-specific changes or "unchanged"]
- hooks/block-dangerous-commands.sh: [branch change or "unchanged"]
- agents/frontend-designer.md: [kept / removed]
- agents/doc-reviewer.md: [kept / removed]

Left as defaults (no project-specific changes needed):
- [list]

Review: [issues found and fixed / "clean"]
```

# Setup Claude

---
IMPORTANT: This command is for empty or new repos with no source code yet. If the project already has source files, use `/add-claude` instead.
---

Configure every `.claude/` config file for a project that doesn't have code yet, based on `project_description.md`.

`CLAUDE.md` must be at the project root (`./CLAUDE.md`). All other config files live in `.claude/`.

## Phase 1: Read project_description.md

Read `project_description.md` from the project root and extract:
- **Language + Framework** from `Tech Stack`
- **Package manager** from `Tech Stack`
- **CSS** from `Tech Stack`
- **Database + ORM** from `Tech Stack`
- **Test framework** from `Tech Stack`
- **Linter/Formatter** from `Tech Stack`
- **Has frontend** from `Shape`
- **Has backend** from `Shape`
- **Has database** from `Shape`
- **Has docs dir** from `Shape`
- **Monorepo** from `Shape` (and packages from `Monorepo packages` if set)
- **Default branch** from `Shape`
- **Planned structure** from `Planned Structure`
- **Key decisions** from `Key Decisions`
- **Domain knowledge** from `Domain Knowledge`
- **Summary / context** from `Summary` and `End User`

Treat any field left as a comment placeholder (e.g. `<!-- ... -->`) or blank as "not decided yet".

If `project_description.md` does not exist or is entirely blank/placeholder, fall back to asking the user directly:

```
project_description.md is missing or empty. Tell me what you're building so I can configure .claude/ for it:

- Language + framework (e.g. TypeScript + Next.js, Python + FastAPI, Go + stdlib)
- Package manager (npm / pnpm / yarn / bun / pip / cargo / go)
- Test framework (jest / vitest / pytest / etc. — or "not decided yet")
- Linter/formatter (eslint+prettier / biome / ruff / etc. — or "not decided yet")
- Database / ORM? (postgres + prisma, sqlite + drizzle, none, etc.)
- Frontend? (yes/no — framework if yes)
- Docs directory planned? (yes/no)
- Monorepo? (yes/no — tool if yes: turborepo, nx, pnpm workspaces, etc.)
```

## Phase 2: Confirm Stack

Present all findings from `project_description.md` at once and confirm with AskUserQuestion before planning any changes:

```
I read project_description.md. Here's what I'll configure for:

**Language**: [language]
**Framework**: [framework + CSS + DB if applicable]
**Package manager**: [npm/pnpm/yarn/bun/pip/cargo/go]
**Test framework**: [jest/vitest/pytest/etc. or "not decided — keeping defaults"]
**Linter/Formatter**: [eslint+prettier/biome/ruff/etc. or "not decided — keeping defaults"]
**Architecture**: [layered/feature-based/monorepo/etc.]
**Has frontend**: [yes/no]
**Has database**: [yes/no — ORM/migration tool if yes]
**Has docs dir planned**: [yes/no]

Correct? (yes/no/corrections)
```

Incorporate any corrections before moving on.

## Phase 3: Review Changes One by One, Then Apply

Determine the full set of changes needed across the files below. Then use AskUserQuestion to walk through each proposed change individually — one question per file. For each, show the specific proposed change (exact new content, or "delete this file") and ask: apply, skip, or correct? The user confirms, rejects, or corrects each one. Do not apply anything yet.

Once all questions have been answered, apply every confirmed change in one pass.

### 3.1 — CLAUDE.md

Fill in each section from `project_description.md`. Remove sections that don't apply. Delete all `> REPLACE:` blocks.

- **Tech Stack**: language, framework, CSS, DB, key libraries — from `Tech Stack`
- **Architecture**: non-obvious architectural decisions only — from `Key Decisions` and `Summary`; skip if not yet decided
- **Structure**: planned directory layout — from `Planned Structure`; skip if not yet decided
- **Commands**: commands for the stated package manager and framework (use conventional defaults if not decided — e.g. `pnpm dev`, `pnpm test`)
- **Key Decisions**: directly from `Key Decisions`
- **Domain Knowledge**: directly from `Domain Knowledge`

### 3.2 — settings.json

Update the `allow` permissions to match the stated package manager:
- Replace `npm run` with the actual package manager (`pnpm run`, `bun run`, `cargo`, `go test`, `python -m pytest`, `make`, etc.)
- Add allow rules for standard scripts of the stated framework
- Keep all `deny` rules unchanged — they are universal

### 3.3 — rules/code-quality.md

No source files exist to sample yet — leave unchanged. Note to user: revisit with `/add-claude` once code exists.

### 3.4 — rules/testing.md

Only update if the stated test framework has specific idioms not covered by the defaults. Otherwise leave unchanged.

### 3.5 — rules/security.md

Update the `paths:` frontmatter based on the stated architecture:
- Replace placeholder paths with the planned API, auth, and middleware directories
- If not decided yet, keep defaults as reasonable guesses

### 3.6 — rules/error-handling.md

- **No backend planned**: delete this file
- **Backend planned**: update `paths:` frontmatter to match planned backend directories

### 3.7 — rules/frontend.md

- **No frontend planned**: delete this file
- **Frontend planned**: update Component Framework table to highlight what the project will use; update path patterns if non-standard directories are planned

### 3.8 — rules/database.md

- **No database planned**: delete this file
- **Database planned**: keep it; update `paths:` frontmatter to match the planned migration directory

### 3.9 — hooks/block-dangerous-commands.sh

Check the default branch name. If it's not `main` or `master`, update the regex pattern to match.

### 3.10 — agents/

- **agents/frontend-designer.md**: delete if no frontend planned
- **agents/doc-reviewer.md**: delete if no docs directory planned

### Note on format-on-save.sh

No changes needed — the hook auto-detects formatters by checking for binaries and config files at runtime. Skip this file.

## Phase 4: Review (Silent Pass)

After applying all changes, verify:
- No `> REPLACE:` blocks remain in `CLAUDE.md`
- All YAML frontmatter is valid
- `settings.json` allow rules cover the commands listed in CLAUDE.md
- No file contradicts another
- Hook scripts referenced in settings.json exist at `.claude/hooks/`

Only surface issues to the user if something needs fixing.

## Phase 5: Summary

```
Done. Here's what was configured:

- CLAUDE.md: [summary of sections filled]
- settings.json: [package manager + added rules]
- rules/security.md: [path changes or "unchanged — defaults kept"]
- rules/error-handling.md: [path changes / removed / "unchanged"]
- rules/frontend.md: [kept with updates / removed]
- rules/database.md: [kept with updates / removed]
- rules/code-quality.md: unchanged — no source files yet to validate against
- rules/testing.md: [framework-specific changes or "unchanged"]
- hooks/block-dangerous-commands.sh: [branch change or "unchanged"]
- agents/frontend-designer.md: [kept / removed]
- agents/doc-reviewer.md: [kept / removed]

Left as defaults:
- [list]

Review: [issues found and fixed / "clean"]

Note: run /add-claude after your first source files exist to refine naming conventions and path patterns against real code.
```

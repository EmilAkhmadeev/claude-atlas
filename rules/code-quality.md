---
alwaysApply: true
---

# Code Quality

## Principles

- Functions do one thing. If it needs a section comment, extract that section.
- No magic values — extract numbers, strings, and config to named constants.
- Handle errors at the boundary. Don't catch and re-throw without adding context.
- No premature abstractions. Three similar lines > a helper used once.
- Don't add features or "improve" things beyond what was asked.
- No dead code or commented-out blocks. Git has history.
- Composition over inheritance.

## Naming

- **Files**: PascalCase for components/classes (`UserProfile.tsx`), kebab-case for utilities/directories (`date-utils.ts`)
- **Booleans**: `is`, `has`, `should`, `can` prefix — `isLoading`, `hasPermission`
- **Functions**: verb-first — `getUser`, `validateEmail`, `handleSubmit`
- **Handlers/callbacks**: `handle*` internally, `on*` as props — `handleClick` / `onClick`
- **Factories**: `create*` — `createUser`. **Converters**: `to*` — `toJSON`. **Predicates**: `is*`/`has*`
- **Constants**: `SCREAMING_SNAKE` — `MAX_RETRIES`, `API_BASE_URL`
- **Enums**: PascalCase members — `Status.Active`
- **Abbreviations**: only universally known (`id`, `url`, `api`, `db`, `config`, `auth`). Acronyms as words: `userId` not `userID`

## Comments

- **WHY**, never WHAT. If the code needs a "what" comment, rename instead.
- Comment: non-obvious decisions, workarounds with issue links, regex patterns, perf tricks
- Don't comment: obvious code, self-explanatory function names, section dividers, type info the language provides
- No commented-out code — delete it. No journal comments — git blame does this.
- API docs (JSDoc, docstrings) at module boundaries only, not every internal function

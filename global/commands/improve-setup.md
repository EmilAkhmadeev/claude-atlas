---
description: Post-session WHAT-WHY-HOW analysis — extract lessons and improve CLAUDE.md, rules, and commands
---

# Improve Setup

## What counts as a finding

Scan the conversation for:

- Corrections ("no", "don't", "that's wrong", "stop doing X")
- Retries or repeated attempts at the same thing
- Tool calls that failed or produced unexpected results
- Cases where the user had to clarify something that should have been inferred
- Anything the user accepted but that was clearly suboptimal in hindsight

If nothing stands out, say so clearly and stop. Do not pad with generic advice.

## Entry format

For each finding, write a short entry:

- **WHAT**: what went wrong or was suboptimal
- **WHY**: root cause (missing rule, wrong assumption, ambiguous instruction, tool misuse, etc.)
- **HOW**: concrete rule or change that would prevent it next time

## Routing table

| Lesson type | Destination |
| --- | --- |
| Behavioral pattern, tone, response style | `~/.claude/projects/<current-project>/memory/` as a `feedback` memory |
| Project-wide rule (Jira, Confluence, tools) | `CLAUDE.md` under a new or existing section |
| Command-specific rule | The relevant `.claude/commands/<command>.md` or `.claude/rules/<rules-file>.md` |
| New reusable command worth extracting | New file in `.claude/commands/` |

## Write rules

- Apply minimal, targeted edits — do not rewrite entire files
- Add to existing sections where possible
- For memory files, follow the frontmatter format and update `MEMORY.md` index if adding a new file

## Steps

1. Scan the session for findings per the rules above
2. Run WHAT-WHY-HOW analysis on each finding
3. Also audit any command files touched or invoked this session:
   - **Project-level commands** (`.claude/commands/`): rules belong in `.claude/rules/` and should be referenced with `@`, not duplicated inline
   - **Global commands** (`~/.claude/commands/`): inline rules are acceptable — `~/.claude/rules/` is not a supported location, so self-contained commands are the correct pattern
   - If a project-level command contains inline rules, propose moving them to the appropriate rules file and replacing with an `@`-reference
   - If a relevant rules file already covers the policy, point to it — do not create a duplicate
4. Present all entries (findings + command audit) to the user and wait for confirmation — do not write anything yet
5. For each confirmed lesson, route and apply it per the routing table
6. Print a summary of every file changed and what was added

# WHAT-WHY-HOW Analysis Rules

Rules used by the `improve-setup` command for post-session lesson extraction.

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
| Behavioral pattern, tone, response style | `~/.claude/projects/C--work-projects-elektronik/memory/` as a `feedback` memory |
| Project-wide rule (Jira, Confluence, tools) | `CLAUDE.md` under a new or existing section |
| Command-specific rule | The relevant `.claude/commands/<command>.md` or `.claude/rules/<rules-file>.md` |
| New reusable command worth extracting | New file in `.claude/commands/` |

## Write rules

- Apply minimal, targeted edits — do not rewrite entire files
- Add to existing sections where possible
- For memory files, follow the frontmatter format and update `MEMORY.md` index if adding a new file

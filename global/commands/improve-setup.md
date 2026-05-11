---
description: Post-session WHAT-WHY-HOW analysis — extract lessons and improve CLAUDE.md, rules, and commands
---

# Improve Setup

Load @.claude/rules/what-why-how-analysis.md for analysis rules, entry format, routing table, and write rules.

1. Scan the session for findings per the rules
2. Run WHAT-WHY-HOW analysis on each finding
3. Also audit any command files touched or invoked this session:
   - Command files (`.claude/commands/`) must contain only action steps and `@`-references — no inline rules, policy, or field logic
   - Rules belong in `.claude/rules/` and should be referenced, not duplicated
   - If a command file contains inline rules, propose moving them to the appropriate rules file and replacing with an `@`-reference
   - If a relevant rules file already covers the policy, point to it — do not create a duplicate
4. Present all entries (findings + command audit) to the user and wait for confirmation — do not write anything yet
5. For each confirmed lesson, route and apply it per the routing table
6. Print a summary of every file changed and what was added

# Skills

Reusable agent skills. Each one lives in `skills/<name>/` and starts at `SKILL.md`.

| Skill | Description |
| :--- | :--- |
| **[review-bugs](skills/review-bugs/SKILL.md)** | Review a PR, diff, or named code area for consequential, reachable bugs. Use for correctness reviews that demand concrete evidence and minimal false positives. Exclude style critiques, speculative hardening, and immaterial test or documentation observations. |
| **[work-ticket](skills/work-ticket/SKILL.md)** | Work a Linear ticket: read the ticket, map how the relevant code works today, then present 2-3 implementation options biased to the simplest viable one and stop for a decision. Triggers on "have a look at this ticket", "get a lay of the land", "what's the best approach", "scope this ticket", or a Linear issue URL / identifier (e.g. ENG-123). Pass `implement` to carry straight through: build the simplest option, bug-review it, then branch, commit, push, and open a PR. |

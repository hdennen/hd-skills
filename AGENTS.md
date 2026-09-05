# Agent instructions

When adding, renaming, updating, or removing a skill under `skills/`, keep the catalog table in `README.md` in sync.

Each skill is one table row:

- **Skill** — bold link whose label is the frontmatter `name` and whose target is that skill's `SKILL.md`
- **Description** — the frontmatter `description`, copied verbatim (no paraphrase)

If a skill gains other frontmatter fields, add a column for each field and fill every row.

Keep rows sorted by `name`. Remove a row when the skill is deleted. After a rename, update the link label, path, and description to match the new `SKILL.md`.

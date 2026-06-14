## Model routing

All sub-skills run on `claude-haiku-4-5` via frontmatter `model:`. The root
dispatcher (`SKILL.md`) has no model override — adding one there would propagate
the override through the entire turn before sub-skills get a chance to set their
own, but more importantly it would be redundant.

**Why haiku for all skills including `react-architecture`:** The `model:` field
applies for the rest of the current turn. When any sub-skill in the dependency
chain sets `model: claude-haiku-4-5`, that override stays active for all
subsequent Skill tool calls in the same turn — including `react-architecture`.
Making `react-architecture` explicit avoids a misleading asymmetry.

**Why no `effort:` override:** `effort` applies to the entire turn, including
the code Claude writes after loading a skill. Setting `effort: low` would reduce
reasoning quality for code review and generation, not just skill loading. The
session effort level is left untouched.

**Model ID is hardcoded in 6 skill files.** There is no plugin-level default
mechanism. When upgrading to a newer haiku model, update the `model:` field in
every SKILL.md: root (none), general, react, react-architecture, node,
accessibility.

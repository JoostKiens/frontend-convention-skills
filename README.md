# Frontend Coding Conventions

A set of reusable coding conventions for use as
[Claude Code skills](https://code.claude.com/docs/en/skills). Designed for
React and Node.js projects in TypeScript.

These reflect my personal opinions on what makes frontend code readable,
maintainable, and bug-resistant. Grounded in SOLID, clean code principles,
DRY with restraint, and hard-won experience across many projects and teams.

## Skills

| File                                                                                | Use in                                      |
| ----------------------------------------------------------------------------------- | ------------------------------------------- |
| [`conventions-general`](./conventions-general/SKILL.md)                             | Any JS/TS project                           |
| [`conventions-node`](./conventions-node/SKILL.md)                                   | Node.js services, APIs, CLI tools, packages |
| [`conventions-react`](./conventions-react/SKILL.md)                                 | React applications                          |
| [`conventions-react-architecture`](./conventions-react-architecture/SKILL.md)       | React apps where architecture is in scope   |
| [`conventions-accessibility`](./conventions-accessibility/SKILL.md)                 | Any project that renders HTML               |

`conventions-node`, `conventions-react`, and `conventions-react-architecture`
each auto-import their dependencies, so you only need to load the leaf skill
for your project.

## Philosophy

Every rule has a reason. Where the right call depends on context, that context
is described — not just what to do, but when and why. A few decisions that
illustrate the approach:

**DRY with restraint:** blanket DRY produces premature abstractions that are
harder to change than the duplication they replaced. The rule of three is a
better guide: duplicate once, extract on the third occurrence.

**Component layout contract:** a component shouldn't know where it lives.
Margins, explicit widths, and absolute positioning on outer elements couple a
component to its context and make it harder to reuse. Layout is the parent's
responsibility.

**Foundation and domain layers:** separating components by domain knowledge
rather than reusability makes the boundary meaningful. A foundation component
can be extracted to any project; a domain component cannot. The one-way
dependency rule (foundation never imports domain) keeps that boundary intact.

The skills are composable: load only the ones that apply to your stack. Every
rule can be extended, removed, or overridden to fit your project — see
[These are defaults, not dogma](#these-are-defaults-not-dogma).

## How I use these

Install by cloning directly into `~/.claude/skills/`:

```bash
git clone https://github.com/JoostKiens/frontend-convention-skills ~/.claude/skills/frontend-convention-skills
```

To update to the latest version:

```bash
git -C ~/.claude/skills/frontend-convention-skills pull
```

Each project's `CLAUDE.md` references whichever skills apply. Load only the
leaf skill — dependencies are pulled in automatically.

React app with architecture conventions:

```markdown
# Conventions
@~/.claude/skills/frontend-convention-skills/conventions-react-architecture/SKILL.md
```

React app without architecture conventions:

```markdown
# Conventions
@~/.claude/skills/frontend-convention-skills/conventions-react/SKILL.md
```

Node.js package:

```markdown
# Conventions
@~/.claude/skills/frontend-convention-skills/conventions-node/SKILL.md
```

Any other JS/TS project (no Node or React):

```markdown
# Conventions
@~/.claude/skills/frontend-convention-skills/conventions-general/SKILL.md
```

`conventions-accessibility` is framework-agnostic and opt-in; add it to any
project that renders HTML:

```markdown
# Conventions
@~/.claude/skills/frontend-convention-skills/conventions-react-architecture/SKILL.md
@~/.claude/skills/frontend-convention-skills/conventions-accessibility/SKILL.md
```

Full-stack app in a single repo (e.g. Next.js with API routes or server
actions):

```markdown
# Conventions
@~/.claude/skills/frontend-convention-skills/conventions-react-architecture/SKILL.md
@~/.claude/skills/frontend-convention-skills/conventions-node/SKILL.md
```

pnpm monorepo with separate frontend and backend packages: add a `CLAUDE.md`
to each package pointing to the relevant skill. The root `CLAUDE.md` can load
`conventions-general` if there is shared code at the root level.

Claude Code loads the skills at the start of each session. They inform how
Claude writes and reviews code without being re-explained every time.

## These are defaults, not dogma

Every skill is marked to defer to project-specific conventions. When working
in an existing codebase or with a team that has its own patterns, Claude is
instructed to flag conflicts rather than silently enforce these defaults. The
goal is consistency within a project, not uniformity across all projects.

If a rule doesn't fit your context, remove it. If something important is
missing, add it. Fork this repo and make it yours.

## Contributing

If you spot a rule that's wrong, incomplete, or missing important context,
open an issue.
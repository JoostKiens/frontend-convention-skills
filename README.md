# Frontend Coding Conventions

A set of reusable coding conventions for use as
[Claude Code skills](https://code.claude.com/docs/en/skills). Designed for
React and Node.js projects in TypeScript.

These reflect my personal opinions on what makes frontend code readable,
maintainable, and bug-resistant. Grounded in SOLID, clean code principles,
DRY with restraint, and hard-won experience across many projects and teams.

## Skills

| Skill                                                                              | Use in                                      |
| ---------------------------------------------------------------------------------- | ------------------------------------------- |
| [`frontend-conventions:general`](./general/SKILL.md)                               | Any JS/TS project                           |
| [`frontend-conventions:node`](./node/SKILL.md)                                     | Node.js services, APIs, CLI tools, packages |
| [`frontend-conventions:react`](./react/SKILL.md)                                   | React applications                          |
| [`frontend-conventions:react-architecture`](./react-architecture/SKILL.md)         | React apps where architecture is in scope   |
| [`frontend-conventions:accessibility`](./accessibility/SKILL.md)                   | Any project that renders HTML               |

## Philosophy

Every rule has a reason. Where the right call depends on context, that context
is described: not just what to do, but when and why. A few decisions that
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

The skills are composable and load on demand: the dispatcher invokes only
what each task needs. Every rule can be extended, removed, or overridden to
fit your project. See [These are defaults, not dogma](#these-are-defaults-not-dogma).

## How I use these

This repo is a [Claude Code plugin](https://code.claude.com/docs/en/plugins).
Installing it makes each convention skill available under the
`frontend-conventions:` namespace, which is what enables on-demand invocation.

Install by cloning into `~/.claude/skills/` as `frontend-conventions`:

```bash
git clone https://github.com/JoostKiens/frontend-convention-skills ~/.claude/skills/frontend-conventions
```

To update to the latest version:

```bash
git -C ~/.claude/skills/frontend-conventions pull
```

### On-demand (recommended)

Add a single line to any project's `CLAUDE.md`:

```markdown
@~/.claude/skills/frontend-conventions/SKILL.md
```

This loads a lightweight dispatcher at session start. Claude then invokes
the relevant convention skill automatically before writing or reviewing code,
only paying the token cost when the conventions are actually needed.

### Always-on

If you prefer conventions to be in context from the start of every session,
reference the specific skill directly. `frontend-conventions:node`,
`frontend-conventions:react`, and `frontend-conventions:react-architecture`
each auto-import their dependencies, so you only need to load the leaf skill
for your project.

React app with architecture conventions:

```markdown
# Conventions
@~/.claude/skills/frontend-conventions/react-architecture/SKILL.md
```

React app without architecture conventions:

```markdown
# Conventions
@~/.claude/skills/frontend-conventions/react/SKILL.md
```

Node.js package:

```markdown
# Conventions
@~/.claude/skills/frontend-conventions/node/SKILL.md
```

Any other JS/TS project (no Node or React):

```markdown
# Conventions
@~/.claude/skills/frontend-conventions/general/SKILL.md
```

`frontend-conventions:accessibility` is framework-agnostic and opt-in; add
it to any project that renders HTML:

```markdown
# Conventions
@~/.claude/skills/frontend-conventions/react-architecture/SKILL.md
@~/.claude/skills/frontend-conventions/accessibility/SKILL.md
```

Full-stack app in a single repo (e.g. Next.js with API routes or server
actions):

```markdown
# Conventions
@~/.claude/skills/frontend-conventions/react-architecture/SKILL.md
@~/.claude/skills/frontend-conventions/node/SKILL.md
```

pnpm monorepo with separate frontend and backend packages: add a `CLAUDE.md`
to each package pointing to the relevant skill. The root `CLAUDE.md` can load
`@~/.claude/skills/frontend-conventions/general/SKILL.md` if there is shared
code at the root level.

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
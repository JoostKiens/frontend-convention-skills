# Frontend Coding Conventions

A set of reusable coding conventions for use as
[Claude Code skills](https://code.claude.com/docs/en/skills). Designed for
React and Node.js projects in TypeScript.

These reflect my personal opinions on what makes frontend code readable,
maintainable, and bug-resistant. Grounded in SOLID, clean code principles,
DRY with restraint, and hard-won experience across many projects and teams.

## Skills

| Skill                                                                                        | Use in                                      |
| -------------------------------------------------------------------------------------------- | ------------------------------------------- |
| [`frontend-conventions:general`](./skills/general/SKILL.md)                                 | Any JS/TS project                           |
| [`frontend-conventions:node`](./skills/node/SKILL.md)                                       | Node.js services, APIs, CLI tools, packages |
| [`frontend-conventions:react`](./skills/react/SKILL.md)                                     | React applications                          |
| [`frontend-conventions:react-architecture`](./skills/react-architecture/SKILL.md)           | React apps where architecture is in scope   |
| [`frontend-conventions:accessibility`](./skills/accessibility/SKILL.md)                     | Any project that renders HTML               |

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

## Installation

This repo is a [Claude Code plugin](https://code.claude.com/docs/en/plugins).
Installing it makes each convention skill available under the
`frontend-conventions:` namespace.

Install by cloning into `~/.claude/skills/` as `frontend-conventions`:

```bash
git clone https://github.com/JoostKiens/frontend-convention-skills ~/.claude/skills/frontend-conventions
```

To update to the latest version:

```bash
git -C ~/.claude/skills/frontend-conventions pull
```

## Usage

Add a directive to your project's `CLAUDE.md` to tell Claude when to invoke
the conventions. Skills are loaded on demand — token cost is only paid when
the skill is actually invoked.

### Full dispatcher (recommended)

Invokes the most specific skill for whatever code is being written:

```markdown
Before writing or reviewing any JavaScript or TypeScript code, invoke the `frontend-conventions` skill.
```

### Pick specific sub-skills

If you only want a subset of the conventions, reference the sub-skills directly.
`frontend-conventions:react` and `frontend-conventions:react-architecture` each
auto-import their dependencies, so you only need the most specific one.

React app with architecture conventions (folder structure, component layers, module boundaries in scope):

```markdown
Before writing or reviewing code, invoke the `frontend-conventions:react-architecture` skill.
```

React app without architecture concerns (components and hooks only, folder structure out of scope):

```markdown
Before writing or reviewing code, invoke the `frontend-conventions:react` skill.
```

Node.js package:

```markdown
Before writing or reviewing code, invoke the `frontend-conventions:node` skill.
```

Any JS/TS project without React or Node:

```markdown
Before writing or reviewing code, invoke the `frontend-conventions:general` skill.
```

`frontend-conventions:accessibility` is framework-agnostic and opt-in; add
it alongside the relevant skill for any project that renders HTML:

```markdown
Before writing or reviewing code, invoke the `frontend-conventions:react-architecture` skill.
Before writing or reviewing any HTML or JSX, also invoke the `frontend-conventions:accessibility` skill.
```

Full-stack app (e.g. Next.js with API routes or server actions):

```markdown
Before writing or reviewing frontend code, invoke the `frontend-conventions:react-architecture` skill.
Before writing or reviewing backend code, invoke the `frontend-conventions:node` skill.
```

pnpm monorepo with separate frontend and backend packages: add a `CLAUDE.md`
to each package with the relevant directive. The root `CLAUDE.md` can invoke
`frontend-conventions:general` if there is shared code at the root level.

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
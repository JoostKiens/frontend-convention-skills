---
name: conventions
description: >
  Frontend coding conventions for JS/TS projects. Invokes the relevant
  convention skill before writing or reviewing code.
---

# Frontend Conventions

Before **writing or reviewing code** in a JS/TS project, invoke the most
specific skill that applies. Dependencies are pulled in automatically — you
do not need to load a skill's dependencies separately.

| When you are about to… | Invoke |
| --- | --- |
| Write or review any JS/TS code | `frontend-conventions:general` |
| Write or review React components or hooks | `frontend-conventions:react` |
| Discuss or change folder structure, component layers, or module boundaries | `frontend-conventions:react-architecture` |
| Write or review Node.js services, APIs, CLI tools, or packages | `frontend-conventions:node` |
| Write or review HTML or JSX that renders to the DOM | `frontend-conventions:accessibility` |

`frontend-conventions:react` auto-imports `frontend-conventions:general`.
`frontend-conventions:react-architecture` auto-imports both. For a full-stack app,
invoke `frontend-conventions:react-architecture` for frontend code and
`frontend-conventions:node` for backend code.

Do not invoke for answering questions, explaining concepts, or planning —
only when code is about to be written or reviewed.

Project-specific conventions in CLAUDE.md always take precedence. If a
loaded skill conflicts with project conventions, flag the conflict rather
than silently picking one.

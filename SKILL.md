---
name: conventions
description: >
  Frontend coding conventions dispatcher. Loads at session start and tells
  Claude when to invoke the relevant convention skill on demand — before
  writing or reviewing code, not before answering questions or planning.
---

# Frontend Conventions

Before **writing or reviewing code** in a JS/TS project, invoke the most
specific skill that applies. Dependencies are pulled in automatically — you
do not need to load a skill's dependencies separately.

| When you are about to… | Invoke |
| --- | --- |
| Write or review any JS/TS code | `conventions-general` |
| Write or review React components or hooks | `conventions-react` |
| Discuss or change folder structure, component layers, or module boundaries | `conventions-react-architecture` |
| Write or review Node.js services, APIs, CLI tools, or packages | `conventions-node` |
| Write or review HTML or JSX that renders to the DOM | `conventions-accessibility` |

`conventions-react` auto-imports `conventions-general`.
`conventions-react-architecture` auto-imports both. For a full-stack app,
invoke `conventions-react-architecture` for frontend code and
`conventions-node` for backend code.

Do not invoke for answering questions, explaining concepts, or planning —
only when code is about to be written or reviewed.

Project-specific conventions in CLAUDE.md always take precedence. If a
loaded skill conflicts with project conventions, flag the conflict rather
than silently picking one.

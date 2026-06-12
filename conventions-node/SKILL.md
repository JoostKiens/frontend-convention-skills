---
name: conventions-node
description: >
  Node.js and pnpm conventions for server-side JS/TS projects: APIs, services,
  CLI tools, and packages. Auto-imports conventions-general. Project-specific
  or team conventions always take precedence; flag conflicts rather than
  silently picking one.
---

@../conventions-general/SKILL.md

# Node / pnpm Conventions

> Extends conventions-general (auto-imported above).
> Project-specific rules always override these defaults.

## Modules

- Use ESM (`import`/`export`) over CommonJS (`require`) in all new projects;
  set `"type": "module"` in `package.json`
- Prefer the `node:` protocol for built-in imports; it is explicit and skips
  module resolution ambiguity:
  ```ts
  // ✅
  import fs from 'node:fs';
  import path from 'node:path';

  // ❌
  import fs from 'fs';
  ```
- Define the public API surface explicitly via the `exports` field in
  `package.json`; don't rely on consumers reaching into internal paths

## Package Management

- Use `pnpm` as the package manager
- Never commit `package-lock.json` or `yarn.lock`; a `pnpm-lock.yaml` should
  always be present and committed

## Environment Variables

- Validate all required environment variables at startup, before the application
  begins serving requests or processing work; fail fast with a clear error
  message listing every missing variable
- Never read `process.env` deep inside business logic; load and validate env
  vars in one place (e.g. `src/config.ts`) and export typed values from there:
  ```ts
  // ✅
  import { config } from './config';
  config.databaseUrl; // typed, validated at startup

  // ❌ scattered reads hide missing-config bugs until that code path runs
  process.env.DATABASE_URL
  ```
- Treat missing required vars as a startup crash, not a runtime fallback

## Logging

- Never use `console.log` in production code; use a structured logger (e.g.
  `pino`) that emits JSON so logs are machine-parseable
- Log at the boundary of your system: incoming requests, outgoing calls,
  startup/shutdown events; avoid logging inside pure business logic
- Include context in log entries: request ID, user ID, operation name; a log
  line without context is hard to correlate in production
- Use log levels correctly: `debug` for development noise, `info` for normal
  operations, `warn` for recoverable anomalies, `error` for failures that
  need attention

## Process Lifecycle

- Handle `SIGTERM` gracefully: stop accepting new work, finish in-flight
  requests, then exit cleanly; containers and orchestrators send `SIGTERM`
  before forcibly killing the process
  ```ts
  process.on('SIGTERM', async () => {
    await server.close();
    await db.disconnect();
    process.exit(0);
  });
  ```
- Set a shutdown timeout; if graceful shutdown takes longer than expected
  (e.g. 10 s), force-exit to avoid hanging the container
- Do not swallow unhandled rejections; register a handler for
  `unhandledRejection` and `uncaughtException` that logs and exits with a
  non-zero code

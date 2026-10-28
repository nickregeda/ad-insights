# Ad Insights

A Chrome extension (Manifest V3) that detects ads on the pages you visit, plus a backend that collects the events and a dashboard that shows which ad networks appear on which sites and how often.

Think of it as a tiny version of an ad intelligence panel: the extension acts as a "panel user" that reports ad placements, and the backend turns those reports into statistics.

> **Status:** early development. The monorepo skeleton is ready; the extension MVP is next. See the [roadmap](#roadmap).

## Architecture

```
[Chrome extension, MV3]
  content script  - finds ad slots in the DOM
  service worker  - collects events, sends them in batches
  popup (Angular) - stats for the current page
        |  HTTPS, JWT
        v
[API: NestJS] -- publishes events --> [RabbitMQ] --> [Worker: Node.js]
        |                                                 |
        |<------------- Redis (aggregates, cache) <-------+
        |                                                 v
        |                                           [Database]
        v
[BFF: Express 5] <-- WebSocket -- [Dashboard: Angular, zoneless]
        |
        +-- LLM API: AI summary of ad activity
```

Only the skeleton and the shared library exist today; the rest is planned.

## Tech stack

| Area                | Technologies                                                     |
| ------------------- | ---------------------------------------------------------------- |
| Monorepo            | NX 24, npm workspaces, TypeScript (strict)                       |
| Code quality        | ESLint 9 (flat config), Prettier, Husky, lint-staged, commitlint |
| Tests               | Vitest                                                           |
| Extension (planned) | Manifest V3, TypeScript, Angular popup                           |
| Backend (planned)   | NestJS, Express 5, RabbitMQ, Redis, SQL/NoSQL database           |
| Dashboard (planned) | Angular (standalone, signals, zoneless)                          |

## Project structure

```
apps/          applications (extension, api, bff, worker, dashboard)
libs/shared/   types and event contracts shared by frontend and backend
```

## Getting started

Requirements: Node.js 22 LTS, npm.

```sh
npm install                      # also installs git hooks via Husky
npx nx show projects             # list projects
npx nx test shared               # run tests for one project
npx nx run-many -t lint test     # lint and test everything
npx nx graph                     # explore the project graph
```

## Conventions

- Commits follow [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `chore:`, ...), enforced by commitlint.
- A pre-commit hook runs ESLint (warnings fail the commit) and Prettier on staged files.
- All text files use LF line endings (`.gitattributes`).

## Roadmap

- [x] Monorepo skeleton: NX, strict TypeScript, ESLint, Prettier, Vitest, git hooks
- [ ] Extension MVP: ad detection, service worker, Angular popup
- [ ] API (NestJS) and BFF (Express 5), Docker Compose
- [ ] Event queue with RabbitMQ, worker, Redis aggregates
- [ ] Sign-in with Google (OAuth 2.0 / OpenID Connect), JWT with refresh tokens
- [ ] Dashboard: live updates over WebSocket, charts, i18n
- [ ] AI summary via LLM API with structured output
- [ ] Docker, CI/CD, deployment to a Linux VPS
- [ ] E2E tests with Playwright

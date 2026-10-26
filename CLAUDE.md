# CLAUDE.md

Ad Insights: a Chrome extension (Manifest V3) that detects ads on visited pages, plus a backend that collects the events and a dashboard that shows ad network statistics.

## Stack

- NX 24 monorepo, npm workspaces (`apps/*`, `libs/*`), package scope `@ad-insights`
- TypeScript (strict), Node.js 22 LTS
- Planned apps: `extension` (MV3, Angular popup), `dashboard` (Angular), `api` (NestJS), `bff` (Express 5), `worker` (Node.js + RabbitMQ)
- Shared event types and contracts live in `libs/shared`

## Commands

- Run tasks through NX, not the underlying tools: `npx nx <target> <project>`, `npx nx run-many -t <target>`
- List projects: `npx nx show projects`
- Format: `npx nx format:write`

## Conventions

- Code, comments, commits and docs in English
- Conventional commits (`feat:`, `fix:`, `chore:`, `docs:`, ...), small focused commits
- Ask before adding a new dependency and explain why it is needed
- Keep it simple: this is a portfolio project, avoid over-engineering

# Dorify v2

Multi-tenant pharmacy marketplace (Telegram Mini App).

## Architecture

- **Backend:** NestJS 11 + DDD + Hexagonal Architecture (monorepo, `apps/api`)
- **Database:** PostgreSQL 17 + Prisma 6 (migrations, seed)
- **Frontend:** React 19 + Vite + Tailwind
- **Bot:** Grammy (Telegram)
- **Payments:** Multicard — per-pharmacy (per-tenant) credentials

## Domain modules (`apps/api/src/modules`)

- **catalog** — products: domain entities, value objects (Money, IKPU), domain events,
  repository ports with Prisma adapters, admin + public controllers
- **iam** — users and pharmacies (tenants): registration, verification lifecycle
  (`pharmacy-verified` / `pharmacy-rejected` events), Telegram auth guard,
  service-token guard, role-based access

## Cross-cutting (`apps/api/src/core`)

- **crypto** — encryption service for tenant payment credentials
- **database** — Prisma module
- Request logging interceptor, all-exceptions filter, zod validation pipe,
  pagination DTO

## Quality gates

- Jest unit tests per module (domain + application layers)
- CI: GitHub Actions (`ci.yml`)
- ESLint + strict TypeScript

## Getting Started

```bash
pnpm install
docker compose -f docker-compose.dev.yml up -d
cd apps/api && cp .env.example .env
pnpm prisma:migrate
pnpm dev
```

# AGENTS.md

Guidelines for agents working in this repository.

## Project Context

- This repo contains Product Requirement Prompts (PRPs) and supporting docs for the **inab** budgeting app, inspired by YNAB principles.
- The planned application uses **Next.js 14**, **React**, and **TypeScript** with a simple JSON storage layer.
- Review `PRPs/inab-budget-base.md` and `PRPs/ai_docs/ynab_basics.md` for domain details before implementing features.

## Core Principles

- Favor clarity, maintainability, and strict type safety.
- Follow KISS and YAGNI.
- Keep React components under 200 lines and functions under 50 lines.
- Avoid introducing unnecessary dependencies.

## Architecture & Stack

- Use the Next.js App Router with server/client component boundaries.
- Add `'use client'` to interactive components.
- API routes must export named handlers (`GET`, `POST`, `DELETE`, etc.).
- Store TypeScript interfaces and helpers under `lib/`.

## Development Workflow

1. Work directly on `main`; do not create new branches.
2. Use PRP methodology for sizable tasks and include all needed context.
3. After modifications, run validation commands:
   ```bash
   npm run lint
   npm run typecheck
   npm run format
   npm test
   npm run build
   ```
4. Fix all issues before committing.

## Code Style

- Use ES Modules and modern TypeScript features.
- Interfaces are required; never use implicit `any`.
- Use `useReducer` for state management; avoid direct mutation.
- Default export React components named in PascalCase.

## Testing & Validation

- Include unit tests alongside code when possible.
- Ensure `npm test` runs successfully.
- Validate that budgets never go negative without explicit adjustment.

## Documentation

- Update README or PRP files when adding features or commands.
- Reference `CLAUDE.md` for additional project-wide guidelines.


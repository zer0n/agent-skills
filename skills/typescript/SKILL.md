---
name: typescript
description: TypeScript project conventions with pnpm and strict mode.
activation:
  - "*.ts"
  - "*.tsx"
  - "tsconfig.json"
  - "typescript"
  - "pnpm"
---

# TypeScript Project Conventions

## Package Manager
- Use `pnpm` over `npm`/`yarn`

## Validation
- Prefer Zod for validation schemas

## Configuration
- Use strict mode (`"strict": true` in tsconfig)
- Enable all strict checks for type safety

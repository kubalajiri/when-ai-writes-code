# Code Style

## TypeScript

- Use ES6 `import` statements (never `require`)
- Prefer `node:` prefix for built-in modules (server)
- Type all function parameters and return values
- Use `interface` for object shapes, `type` for unions/intersections

## Naming

- **Files**: kebab-case for utilities, PascalCase for components
- **Variables**: camelCase
- **Constants**: UPPER_SNAKE_CASE for true constants
- **Interfaces**: PascalCase, no `I` prefix

## Functions

- Keep functions small and focused (single responsibility)
- Prefer arrow functions for callbacks and components
- Use early returns to reduce nesting

## Formatting

- Run `npm run ai-check` to verify (includes Prettier)
- Let Prettier handle formatting - don't manually adjust

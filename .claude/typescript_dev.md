# TypeScript Development Guidelines

This document contains critical information about working with TypeScript codebases. Follow these guidelines precisely.

## Core Development Rules

1. Package Management
   - Use npm or yarn for package management
   - Installation: `npm install package` or `yarn add package`
   - Dev dependencies: `npm install -D package` or `yarn add -D package`
   - Scripts: Define in package.json
   - Lock files: Always commit package-lock.json or yarn.lock

2. Code Quality
   - Strict TypeScript configuration required
   - No implicit any types
   - Explicit return types for public APIs
   - Prefer interfaces over type aliases for object shapes
   - Use const assertions where appropriate

3. Testing Requirements
   - Framework: Jest or Vitest
   - Coverage: Minimum 80% for new code
   - Test files: `*.test.ts` or `*.spec.ts`
   - Mock external dependencies
   - Test edge cases and error scenarios

## TypeScript Coding Standards

### Configuration
- Use strict mode in tsconfig.json
- Enable all strict checks:
  - `"strict": true`
  - `"noImplicitAny": true`
  - `"strictNullChecks": true`
  - `"strictFunctionTypes": true`
  - `"strictBindCallApply": true`
  - `"strictPropertyInitialization": true`

### Type System Best Practices
- Avoid using `any` type
- Use union types instead of enums for string literals
- Prefer `unknown` over `any` when type is truly unknown
- Use type guards for type narrowing
- Leverage utility types (Partial, Required, Pick, Omit, etc.)

### Code Organization
- One class/interface per file
- Group related types in dedicated type files
- Use barrel exports (index.ts) for clean imports
- Maintain consistent file naming: kebab-case for files
- Use PascalCase for types/interfaces, camelCase for variables/functions

### Async/Await
- Always use async/await over raw promises
- Proper error handling with try/catch
- Type Promise return values explicitly
- Avoid mixing callbacks with promises

### Documentation
- JSDoc comments for public APIs
- Include @param, @returns, @throws tags
- Document complex types with examples
- Keep comments up-to-date with code changes

## Development Tools

### Code Formatting
1. Prettier
   - Config: `.prettierrc`
   - Format: `npm run format` or `yarn format`
   - Common settings:
     ```json
     {
       "semi": true,
       "singleQuote": true,
       "tabWidth": 2,
       "trailingComma": "es5",
       "printWidth": 100
     }
     ```

2. ESLint
   - Config: `.eslintrc.js` or `.eslintrc.json`
   - Check: `npm run lint` or `yarn lint`
   - Fix: `npm run lint:fix` or `yarn lint:fix`
   - Use with TypeScript plugin
   - Recommended rules:
     - @typescript-eslint/explicit-function-return-type
     - @typescript-eslint/no-explicit-any
     - @typescript-eslint/no-unused-vars

### Type Checking
- Command: `tsc --noEmit`
- Run before commits
- Fix all type errors before pushing
- Use strict mode always

### Build Process
- Use appropriate bundler (Vite, Webpack, Rollup, esbuild)
- Generate source maps for debugging
- Optimize for production builds
- Tree-shaking for smaller bundles

## Testing Standards

### Unit Testing
- Test file location: Same directory as source or in `__tests__`
- Naming: `ComponentName.test.ts`
- Structure:
  ```typescript
  describe('ComponentName', () => {
    describe('methodName', () => {
      it('should do something specific', () => {
        // Arrange
        // Act
        // Assert
      });
    });
  });
  ```

### Test Coverage
- Run: `npm test -- --coverage`
- Requirements:
  - Statements: 80%
  - Branches: 80%
  - Functions: 80%
  - Lines: 80%

### Mocking
- Use jest.mock() for modules
- Create mock factories for complex objects
- Reset mocks between tests
- Avoid over-mocking

## Error Resolution

### Common TypeScript Errors
1. Type errors:
   - Check for null/undefined
   - Use optional chaining (?.)
   - Add proper type annotations
   - Use type assertions sparingly

2. Module resolution:
   - Check tsconfig paths
   - Verify import extensions
   - Use correct module system

3. Build errors:
   - Clear cache/node_modules
   - Check for circular dependencies
   - Verify all dependencies installed

### Best Practices
- Enable incremental compilation
- Use project references for monorepos
- Leverage TypeScript's language service
- Keep dependencies up-to-date
- Use strict mode from project start
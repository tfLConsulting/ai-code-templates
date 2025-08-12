# Technical Standards and Conventions

## Overview

This document defines the technical standards, tools, and conventions for software projects. These standards ensure consistency, maintainability, and quality across all components, supporting the iterative specification-first development approach.

## Technology Stack

### Core Technologies
- **Client Runtime**: Modern browsers with ES2020+ support
- **Server Runtime**: Node.js (LTS version, currently 18.x or 20.x) for Firebase Functions
- **Language**: TypeScript (strict mode enabled)
- **Package Manager**: pnpm (with workspaces for monorepo structure)
- **Testing Framework**: Vitest (for fast unit and integration testing)
- **Build Tool**: Vite (client), Firebase CLI (server functions)

### Frontend Stack
- **Framework**: React 18+ with TypeScript
- **Build Tool**: Vite (for fast builds and hot reloading)
- **Styling**: Tailwind CSS v4 (utility-first CSS framework)
- **UI Components**: shadcn/ui (copy-paste component library)
- **Icons**: Lucide React (beautiful, customizable SVG icons)
- **State Management**: React Context + hooks (for local state), custom Firebase hooks (for Firebase integration)

### Backend Stack
- **Backend-as-a-Service**: Firebase (authentication, hosting, functions)
- **Database**: Firestore (NoSQL document database)
- **AI/Automation**: Firebase Genkit (for AI workflows and integrations)
- **Functions**: Firebase Cloud Functions (serverless compute)
- **Storage**: Firebase Storage (file uploads and media)

### Development Tools
- **Linting**: ESLint with TypeScript support
- **Formatting**: Prettier (consistent code formatting)
- **Type Checking**: TypeScript compiler with strict configuration
- **Build Tool**: Vite (for fast builds and hot reloading)
- **Version Control**: Git with conventional commits

## Programming Paradigms

### Functional Programming First
- **Prefer pure functions** - functions that don't cause side effects and return consistent outputs for given inputs
- **Immutable data structures** - avoid mutating objects and arrays in place
- **Function composition** - build complex operations from simple, composable functions
- **Avoid classes when functions suffice** - use classes only when encapsulation and state management are truly necessary

### Side Effect Management
- **Isolate side effects** - separate pure logic from I/O operations, file system access, etc.
- **Dependency injection** - pass dependencies as parameters rather than importing them directly
- **Error boundaries** - handle errors at appropriate levels without affecting pure functions

## Code Quality Standards

### Type Safety
```typescript
// ✅ Good: Strict typing
interface UserPreferences {
  readonly theme: string;
  readonly language: string;
  readonly notifications: readonly ['email', 'push', 'sms'];
}

// ❌ Avoid: Any types or loose typing
function processUserData(options: any): any
```

### Function Design
```typescript
// ✅ Good: Pure function with clear types
const calculateScore = (
  answers: string[],
  basePoints: number,
  multiplier: number
): number => {
  const correctAnswers = answers.filter(answer => answer === 'correct').length;
  return correctAnswers * basePoints * multiplier;
};

// ❌ Avoid: Impure function with side effects
let globalState = {};
function processUserInput(input: string) {
  globalState.lastProcessed = input; // Side effect
  console.log('Processing:', input); // Side effect
  return input.toUpperCase();
}
```

### Error Handling
```typescript
// ✅ Good: Result pattern for error handling
type Result<T, E = Error> = { success: true; data: T } | { success: false; error: E };

const parseDocument = (content: string): Result<Document> => {
  try {
    const doc = JSON.parse(content);
    return { success: true, data: doc };
  } catch (error) {
    return { success: false, error: error as Error };
  }
};

// ❌ Avoid: Throwing exceptions in pure functions
const parseDocumentBad = (content: string): Document => {
  return JSON.parse(content); // Can throw
};
```

## File and Directory Conventions

### Naming Conventions
- **Files**: `kebab-case.ts` (e.g., `data-processor.ts`)
- **Directories**: `kebab-case` (e.g., `data-engine/`)
- **Functions**: `camelCase` (e.g., `validateUserInput`)
- **Types/Interfaces**: `PascalCase` (e.g., `UserProfile`, `ApiResponse`)
- **Constants**: `SCREAMING_SNAKE_CASE` (e.g., `MAX_RETRY_ATTEMPTS`)

### File Structure

The organization pattern depends on the type of module being implemented:

#### React Components (UI Features)
```
src/
├── component-name/
│   ├── index.ts              # Public API exports
│   ├── component-name.ts     # Main implementation
│   ├── component-name.test.ts # Tests
│   ├── types.ts              # Type definitions
│   └── utils/                # Utility functions
│       ├── helper-function.ts
│       └── helper-function.test.ts
```

#### Foundation Libraries (Schemas, Utilities)
```
src/
├── types/
│   └── library-name.ts       # Core type definitions
├── lib/
│   ├── implementation.ts     # Main logic
│   ├── validation.ts         # Validation functions
│   ├── examples.ts           # Test data and examples
│   ├── implementation.test.ts # Tests
│   └── index.ts              # Public API exports
└── utils/                    # Shared utilities
    ├── helper-function.ts
    └── helper-function.test.ts
```

#### Schema/Validation Libraries
```
src/
├── types/schema-name.ts      # TypeScript interfaces
├── lib/
│   ├── schema-validation.ts  # Runtime validation (Zod, etc.)
│   ├── schema-examples.ts    # Reference examples
│   ├── schema-validation.test.ts # Comprehensive tests
│   └── index.ts              # Clean public API
└── docs/                     # Usage examples and guides
    └── README.md
```

#### API/Service Layers
```
src/
├── services/
│   ├── api-client.ts         # HTTP client implementation
│   ├── api-client.test.ts    # Client tests
│   └── types.ts              # Request/response types
├── hooks/                    # React hooks for data fetching
│   ├── use-data.ts
│   └── use-data.test.ts
└── utils/
    ├── api-utils.ts
    └── api-utils.test.ts
```

### Import/Export Conventions
```typescript
// ✅ Good: Named exports for better tree-shaking
export const processData = (/* ... */): string => { /* ... */ };
export const processUserData = (/* ... */): UserProfile => { /* ... */ };

// ✅ Good: Barrel exports in index.ts
export { processData, calculateStructure } from './data-processor';
export type { UserPreferences, ApiResponse } from './types';

// ❌ Avoid: Default exports (harder to refactor)
export default function dataProcessor() { /* ... */ }
```

## Testing Standards

### Test Organization
- **Unit tests**: Test individual functions in isolation
- **Integration tests**: Test component interactions
- **Property-based tests**: Use random inputs to test invariants
- **Test file naming**: `*.test.ts` alongside source files

### Test Structure
```typescript
import { describe, it, expect } from 'vitest';
import { calculateScore } from './score-utils';

describe('calculateScore', () => {
  it('should calculate score for correct answers', () => {
    const result = calculateScore(['correct', 'incorrect'], 10, 1.5);
    expect(result).toBe(15); // 1 correct * 10 * 1.5
  });

  it('should handle multiple correct answers', () => {
    const result = calculateScore(['correct', 'correct', 'incorrect'], 10, 1.5);
    expect(result).toBe(30); // 2 correct * 10 * 1.5
  });

  it('should handle no correct answers', () => {
    const result = calculateScore(['incorrect', 'incorrect'], 10, 1.5);
    expect(result).toBe(0); // 0 correct * 10 * 1.5
  });
});
```

### Test Coverage
- **Minimum 80% code coverage** for all components
- **100% coverage** for critical path functions
- **Test edge cases** and error conditions
- **Use descriptive test names** that explain the scenario

## Test Data and Examples Organization

### Example Data Standards
- **Co-locate examples**: Place example data near the code it validates
- **Comprehensive coverage**: Examples should cover common use cases and edge cases
- **Realistic data**: Use real-world representative examples, not minimal test fixtures
- **Named exports**: Export examples with descriptive names

```typescript
// ✅ Good: Comprehensive, realistic examples
export const customerOrderDocument: DocumentData = {
  metadata: { type: 'order', version: '1.0', timestamp: '2024-01-01' },
  // ... realistic order data
};

export const invoiceDocument: DocumentData = {
  // ... different document type
};

export const allExampleDocuments = {
  customerOrderDocument,
  invoiceDocument,
  // ... other examples
} as const;

// ❌ Avoid: Minimal, unrealistic test data
export const testDocument = { fields: [] };
```

### Test Fixtures vs Examples
- **Examples**: Real-world representative data for documentation and validation
- **Fixtures**: Minimal test data for specific test cases
- **Location**: Examples in dedicated files, fixtures in test files or test utils

## Schema Evolution and Versioning

### Schema Versioning Strategy
- **Semantic versioning**: Follow semver for schema changes
- **Backward compatibility**: Maintain compatibility within major versions
- **Migration guides**: Document breaking changes and migration paths
- **Version metadata**: Include version information in schema metadata

```typescript
// ✅ Good: Version-aware schema
export interface DocumentMetadata {
  readonly version?: string; // Schema version (e.g., "1.0.0")
  readonly rationale: string;
  // ... other metadata
}

// ✅ Good: Migration functions for schema evolution
export const migrateDocumentSchema = (
  document: unknown, 
  fromVersion: string, 
  toVersion: string
): Result<DocumentData> => {
  // Migration logic
};
```

### Schema Validation Layers
1. **Type-level validation**: TypeScript interfaces for compile-time safety
2. **Runtime validation**: Zod/Yup schemas for runtime type checking  
3. **Business validation**: Domain-specific rules and constraints
4. **Design validation**: UX/accessibility heuristics

## Foundation vs Feature Components

### Foundation Components
- **Purpose**: Core data structures, utilities, and abstractions
- **Dependencies**: Minimal external dependencies
- **Testing**: Exhaustive test coverage (90%+)
- **Documentation**: Comprehensive API documentation
- **Examples**: Real-world usage examples

```typescript
// Foundation: Core schema definitions
export interface DocumentData { /* ... */ }
export const validateDocumentData = (data: unknown): Result<DocumentData> => { /* ... */ };
```

### Feature Components  
- **Purpose**: User-facing functionality and business logic
- **Dependencies**: Build on foundation components
- **Testing**: Focus on user scenarios and integration
- **Documentation**: Usage guides and tutorials

```typescript
// Feature: AI generation using foundation schema
export const generateDocumentFromPrompt = async (prompt: string): Promise<DocumentData> => {
  // Uses foundation DocumentData type
};
```

### Integration Patterns
- **Clean boundaries**: Features depend on foundations, not vice versa
- **Stable APIs**: Foundation APIs change less frequently than features
- **Composition**: Features compose foundation utilities rather than extending them

## Container/Presentational Pattern (Consistent Application)

### Always Apply This Pattern
All components should follow Container/Presentational separation:

**Container Components** (`pages/` or `containers/`):
- Handle all state management
- Make API calls and data fetching
- Contain business logic and event handlers
- Pass props down to presentational components
- No JSX rendering beyond wrapper and passing props

**Presentational Components** (`components/`):
- Pure UI rendering only
- Receive all data via props
- No state management (except UI-only state like hover)
- No API calls or side effects
- Easy to test by passing props

### API Layer Complexity
**Current scale**: Simple API client with retry logic
**Next scale**: When we have >5 different API operations
**Enterprise scale**: When we have multiple API consumers

### Testing Strategy
**Current**: Unit tests for critical paths, CLI tools for AI testing
**Next**: Add integration tests when we have >10 components
**Full**: E2E tests when we have complete user flows

## Documentation Standards

### Code Documentation
```typescript
/**
 * Calculates user score based on quiz responses and scoring parameters.
 * 
 * @param answers - Array of user answers to evaluate
 * @param basePoints - Base points awarded per correct answer
 * @param multiplier - Score multiplier (e.g., 1.5 for 150%)
 * @returns The total calculated score
 * 
 * @example
 * ```typescript
 * const score = calculateScore(['correct', 'correct', 'incorrect'], 10, 1.5);
 * console.log(score); // 30
 * ```
 */
const calculateScore = (
  answers: string[],
  basePoints: number,
  multiplier: number
): number => {
  // Implementation...
};
```

### README Standards
Each component directory should include:
- Purpose and responsibilities
- Public API documentation
- Usage examples
- Performance characteristics
- Known limitations

### Library Documentation Standards

#### Foundation Library Documentation
Foundation libraries (schemas, utilities) require comprehensive documentation:

```markdown
# Library Name

## Overview
- **Purpose**: What problem does this solve?
- **Key Features**: Main capabilities in bullet points
- **Dependencies**: External dependencies and peer dependencies

## Quick Start
```typescript
// Minimal working example
import { mainFunction } from '@project/library';
const result = mainFunction(input);
```

## API Reference
### Types
- Document all public interfaces
- Include property descriptions and constraints
- Show inheritance relationships

### Functions
- Parameter types and descriptions
- Return types and possible values
- Error conditions and handling
- Performance characteristics

## Examples
- Real-world usage scenarios
- Common patterns and best practices
- Integration with other packages

## Migration Guides
- Breaking changes between versions
- Upgrade paths and deprecation notices
```

#### Performance Documentation Requirements
For performance-critical libraries, document:
- Time complexity of operations
- Memory usage patterns
- Benchmarks with typical data sizes
- Performance comparison with alternatives

```typescript
// ✅ Good: Performance characteristics documented
/**
 * Validates document schema with comprehensive checks.
 * 
 * Performance: ~5ms for typical documents, O(n) where n = field count
 * Memory: Creates temporary validation objects, ~2x input size
 * 
 * @param data - Unknown input to validate
 * @returns Validation result with typed data or detailed errors
 */
export const validateDocumentData = (data: unknown): DocumentValidationResult => {
  // Implementation
};
```

## Configuration Files

### TypeScript Configuration (`tsconfig.json`)
```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noImplicitOverride": true
  }
}
```

### ESLint Configuration
- Extend `@typescript-eslint/recommended`
- Enable rules for functional programming patterns
- Enforce consistent naming conventions
- Warn on unused variables and imports

### Prettier Configuration
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

## Git Workflow Standards

### Commit Messages
Follow [Conventional Commits](https://www.conventionalcommits.org/):
```
feat(parser): add support for nested data structures
fix(processor): resolve async loading race condition
docs(api): update component specification
test(validation): add edge case tests for data processing
```

### Branch Naming
- `feature/component-name-description`
- `fix/issue-description`
- `docs/update-description`

### Pull Request Standards
- Include specification updates when changing behavior
- Update tests before implementation
- Ensure all tests pass
- Update documentation as needed

## Performance Standards

### Memory Management
- Avoid memory leaks in long-running processes
- Use streaming for large file operations
- Dispose of resources properly (connections, files, timers, etc.)

### Computational Efficiency
- Prefer O(n) algorithms over O(n²) when possible
- Cache expensive calculations when appropriate
- Use lazy evaluation for optional computations

### Bundle Size
- Tree-shake unused dependencies
- Avoid large libraries for simple operations
- Monitor bundle size with each change

## Project Setup and Environment Configuration

### Monorepo Organization Principles

#### Package Classification
Organize packages by responsibility and dependency relationships:

- **Foundation packages**: Core schemas, types, utilities with no internal dependencies
- **Feature packages**: Business logic that builds on foundation packages  
- **Application packages**: Deployable apps that compose features and foundations
- **Tool packages**: Build tools, linting configs, shared development utilities

#### Dependency Flow Rules
- Foundation packages → No internal dependencies
- Feature packages → May depend on foundation packages only
- Application packages → May depend on both foundation and feature packages
- No circular dependencies between packages

#### Package Naming Strategy
```json
// ✅ Good: Clear, scoped naming
{
  "name": "@project/foundation-schema",
  "name": "@project/feature-ai-generation", 
  "name": "@project/app-webapp"
}

// ❌ Avoid: Generic names
{
  "name": "@project/utils",
  "name": "@project/common"
}
```

### Monorepo Structure (Firebase Implementation)

This project uses a clean monorepo structure with pnpm workspaces:

```
my-project/
├── package.json                    # Root workspace configuration
├── pnpm-workspace.yaml            # pnpm workspace config
├── firebase.json                  # Firebase project configuration
├── .firebaserc                    # Firebase project aliases
├── firestore.rules               # Firestore security rules
├── firestore.indexes.json        # Firestore indexes
├── .gitignore
├── README.md
├── specs/                         # Documentation and specifications
│   ├── development-approach.md
│   ├── standards.md
│   └── technology-implementation.md
├── webapp/                        # React frontend application
│   ├── package.json
│   ├── vite.config.ts
│   ├── tailwind.config.ts
│   ├── tsconfig.json
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   ├── services/
│   │   ├── types/
│   │   └── lib/
│   └── dist/                      # Build output (for Firebase hosting)
└── functions/                     # Firebase Cloud Functions
    ├── package.json
    ├── tsconfig.json
    ├── src/
    │   ├── index.ts
    │   ├── internalAPI.ts
    │   ├── handlers/
    │   ├── services/
    │   └── utils/
    └── lib/                       # Compiled output
```

### Initial Project Bootstrap

#### Step 1: Root Workspace Setup
```bash
# Create project directory and root configuration
mkdir my-project && cd my-project

# Initialize root package.json
npm init -y

# Create pnpm workspace configuration
echo "packages:\n  - 'webapp'\n  - 'functions'" > pnpm-workspace.yaml
```

#### Step 2: Root Package.json Configuration
```json
{
  "name": "my-project",
  "private": true,
  "scripts": {
    "dev": "pnpm run --parallel dev",
    "build": "pnpm run --recursive build",
    "test": "pnpm run --recursive test",
    "lint": "pnpm run --recursive lint",
    "clean": "pnpm run --recursive clean",
    "deploy": "pnpm run build && firebase deploy"
  },
  "devDependencies": {
    "firebase-tools": "^13.0.0",
    "typescript": "^5.0.0"
  },
  "engines": {
    "node": ">=18.0.0",
    "pnpm": ">=8.0.0"
  },
  "packageManager": "pnpm@8.15.0"
}
```

#### Step 3: Firebase Project Setup
```bash
# Install Firebase CLI globally if not already installed
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize Firebase project
firebase init

# Select features:
# - Firestore
# - Functions  
# - Hosting
# - Emulators

# When prompted for functions directory: enter 'functions'
# When prompted for hosting public directory: enter 'webapp/dist'
```

#### Step 4: Webapp Setup (React + Vite)
```bash
# Create webapp directory
mkdir webapp && cd webapp

# Initialize with Vite
pnpm create vite . --template react-ts

# Install dependencies
pnpm install

# Install additional dependencies
pnpm add firebase react-firebase-hooks react-router-dom lucide-react
pnpm add class-variance-authority clsx tailwind-merge

# Setup shadcn/ui
pnpm dlx shadcn-ui@latest init
pnpm dlx shadcn-ui@latest add button input card dialog textarea

# Setup Tailwind v4
pnpm add -D tailwindcss@next @tailwindcss/vite@next

# Install development dependencies
pnpm add -D vitest @testing-library/react @testing-library/jest-dom
pnpm add -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
pnpm add -D prettier eslint-config-prettier eslint-plugin-prettier

cd ..
```

#### Step 5: Functions Setup
```bash
# The functions directory should already exist from firebase init
cd functions

# Install dependencies (pnpm works fine with Firebase Functions)
pnpm install

# Install additional dependencies
pnpm add @genkit-ai/core @genkit-ai/flow @genkit-ai/googleai @genkit-ai/firebase
pnpm add -D @types/node firebase-functions-test

cd ..
```

## Successful Monorepo Patterns

### Build Orchestration
Our current build pattern works well for the 3-package structure:

```json
{
  "build": "pnpm run build:schema && pnpm run build:webapp && pnpm run build:functions",
  "dev": "pnpm run build:schema && concurrently \"pnpm run dev:webapp\" \"pnpm run dev:functions\""
}
```

**Why this works**:
- Schema builds first (foundation dependency)
- Parallel development for webapp + functions
- Clear dependency order

**When to change**: Only when adding more packages that depend on each other

### Configuration Files

#### Root `firebase.json`
```json
{
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "functions": {
    "source": "functions",
    "runtime": "nodejs18",
    "predeploy": ["pnpm --filter=functions run build"]
  },
  "hosting": {
    "public": "webapp/dist",
    "ignore": ["firebase.json", "**/.*", "**/node_modules/**"],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ],
    "predeploy": ["pnpm --filter=webapp run build"]
  },
  "emulators": {
    "auth": {
      "port": 9099
    },
    "functions": {
      "port": 5001
    },
    "firestore": {
      "port": 8080
    },
    "hosting": {
      "port": 5000
    },
    "ui": {
      "enabled": true,
      "port": 4000
    }
  }
}
```

#### `webapp/package.json`
```json
{
  "name": "@my-project/webapp",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "lint": "eslint src --ext ts,tsx",
    "lint:fix": "eslint src --ext ts,tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx}\"",
    "type-check": "tsc --noEmit",
    "clean": "rm -rf dist"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "firebase": "^10.0.0",
    "react-firebase-hooks": "^5.1.0",
    "react-router-dom": "^6.0.0",
    "lucide-react": "^0.400.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.0.0",
    "tailwind-merge": "^2.0.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "vite": "^5.0.0",
    "tailwindcss": "^4.0.0",
    "@testing-library/react": "^14.0.0",
    "vitest": "^1.0.0"
  }
}
```

#### `functions/package.json`
```json
{
  "name": "@my-project/functions",
  "private": true,
  "scripts": {
    "build": "tsc",
    "dev": "tsc --watch",
    "serve": "pnpm run build && firebase emulators:start --only functions",
    "shell": "pnpm run build && firebase functions:shell",
    "deploy": "firebase deploy --only functions",
    "logs": "firebase functions:log",
    "test": "vitest",
    "clean": "rm -rf lib"
  },
  "engines": {
    "node": "18"
  },
  "main": "lib/index.js",
  "dependencies": {
    "firebase-admin": "^12.0.0",
    "firebase-functions": "^5.0.0",
    "@genkit-ai/core": "^0.5.0",
    "@genkit-ai/flow": "^0.5.0",
    "@genkit-ai/googleai": "^0.5.0",
    "@genkit-ai/firebase": "^0.5.0"
  },
  "devDependencies": {
    "@types/node": "^20.0.0",
    "typescript": "^5.0.0",
    "firebase-functions-test": "^3.0.0",
    "vitest": "^1.0.0"
  }
}
```

#### `webapp/.env.local`
```bash
# Firebase configuration
VITE_FIREBASE_API_KEY=your-api-key
VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abcdef

# Development flags
VITE_USE_EMULATORS=true
VITE_DEBUG_MODE=true
```

### Development Workflow Commands

#### Root Level Commands (run from project root)
```bash
# Install all dependencies
pnpm install

# Start development (both webapp and functions)
pnpm run dev

# Run all tests
pnpm run test

# Lint all packages
pnpm run lint

# Build all packages
pnpm run build

# Deploy to Firebase
pnpm run deploy
```

#### Package-Specific Commands
```bash
# Work with specific package
pnpm --filter=webapp run dev      # Start only webapp dev server
pnpm --filter=functions run dev   # Start only functions development

# Install dependency to specific package
pnpm --filter=webapp add react-query
pnpm --filter=functions add express

# Run tests for specific package
pnpm --filter=webapp run test
pnpm --filter=functions run test
```

#### Firebase Development
```bash
# Start emulators (from root)
firebase emulators:start

# Deploy specific services
firebase deploy --only hosting    # Deploy only webapp
firebase deploy --only functions  # Deploy only functions

# View logs
firebase functions:log
```

### IDE Configuration

#### Root `.vscode/settings.json`
```json
{
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "emmet.includeLanguages": {
    "typescript": "html",
    "typescriptreact": "html"
  },
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"],
    ["cx\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ],
  "eslint.workingDirectories": ["webapp", "functions"],
  "typescript.preferences.includePackageJsonAutoImports": "off"
}
```

#### Root `.vscode/extensions.json`
```json
{
  "recommendations": [
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-vscode.vscode-typescript-next",
    "formulahendry.auto-rename-tag",
    "christian-kohler.path-intellisense",
    "firebase.vscode-firestore-rules"
  ]
}
```

## Dependencies Management

### Dependency Categories
- **Production**: Only essential runtime dependencies
- **Development**: Build tools, testing, linting
- **Peer**: Let consumers choose their own versions

### Version Pinning
- Pin exact versions for development dependencies
- Use caret ranges (^) for production dependencies
- Regular dependency audits and updates

## Error Handling Patterns

### Result Types
Prefer Result types over throwing exceptions:
```typescript
type ParseResult<T> = 
  | { success: true; data: T }
  | { success: false; error: string; details?: unknown };
```

### Validation
Use schema validation for external inputs:
```typescript
import { z } from 'zod';

const UserPreferencesSchema = z.object({
  theme: z.enum(['light', 'dark', 'auto']),
  language: z.string().min(2).max(5),
  notifications: z.array(z.enum(['email', 'push', 'sms'])),
});

type UserPreferences = z.infer<typeof UserPreferencesSchema>;
```

## Security Standards

### Input Validation
- Validate all external inputs
- Sanitize file paths and names
- Limit resource consumption (memory, CPU time)

### Dependency Security
- Regular `pnpm audit` checks
- Avoid dependencies with known vulnerabilities
- Use minimal, well-maintained packages

---

These standards provide a solid foundation for building maintainable, high-quality software using the iterative specification-first development approach. All team members and AI assistants should follow these guidelines consistently throughout the project, adapting them as needed based on learnings from each development cycle.

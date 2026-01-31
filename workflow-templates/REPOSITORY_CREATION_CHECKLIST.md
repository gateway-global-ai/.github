# Repository Creation Checklist

Use this checklist when creating a new repository in the Gateway Global AI organization to ensure compliance with governance standards.

## Pre-Creation

- [ ] **Repository Name**: Follows kebab-case convention (e.g., `gateway-platform`, `agent-toolkit`)
- [ ] **Purpose Defined**: Clear, documented purpose aligned with organization mission
- [ ] **Architecture Review**: Design reviewed and approved by Architecture Review Board
- [ ] **Dependency Approval**: All required dependencies pre-approved

## Repository Setup

### 1. Initial Structure

- [ ] Create repository from template (if applicable)
- [ ] Initialize with README.md
- [ ] Add .gitignore (Node.js template)
- [ ] Add LICENSE file
- [ ] Set repository visibility (private by default for new projects)

### 2. Core Files

- [ ] **package.json**
  ```json
  {
    "name": "@gateway-global-ai/[repo-name]",
    "version": "0.1.0",
    "private": true,
    "type": "module",
    "engines": {
      "node": ">=20.0.0"
    }
  }
  ```

- [ ] **tsconfig.json**
  ```json
  {
    "compilerOptions": {
      "target": "ES2022",
      "module": "ESNext",
      "lib": ["ES2022"],
      "moduleResolution": "bundler",
      "strict": true,
      "esModuleInterop": true,
      "skipLibCheck": true,
      "forceConsistentCasingInFileNames": true,
      "resolveJsonModule": true,
      "outDir": "./dist",
      "rootDir": "./src"
    },
    "include": ["src/**/*"],
    "exclude": ["node_modules", "dist", "tests"]
  }
  ```

- [ ] **.eslintrc.json**
  ```json
  {
    "extends": [
      "eslint:recommended",
      "plugin:@typescript-eslint/recommended",
      "plugin:@typescript-eslint/recommended-requiring-type-checking"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
      "project": "./tsconfig.json"
    }
  }
  ```

- [ ] **.prettierrc**
  ```json
  {
    "semi": true,
    "trailingComma": "es5",
    "singleQuote": true,
    "printWidth": 80,
    "tabWidth": 2
  }
  ```

### 3. Directory Structure

- [ ] Create `src/` directory
- [ ] Create environment directories:
  - [ ] `dev/` with config.ts and .env.example
  - [ ] `stag/` with config.ts and .env.example
  - [ ] `prod/` with config.ts and .env.example
- [ ] Create `tests/` directory with subdirectories:
  - [ ] `tests/unit/`
  - [ ] `tests/integration/`
  - [ ] `tests/e2e/`
- [ ] Create `docs/` directory

### 4. GitHub Settings

- [ ] **Branch Protection** for `main`:
  - [ ] Require pull request reviews (minimum 1)
  - [ ] Require status checks to pass
  - [ ] Require conversation resolution before merging
  - [ ] Require linear history
  - [ ] Include administrators

- [ ] **Merge Settings**:
  - [ ] Allow squash merging
  - [ ] Disable merge commits
  - [ ] Disable rebase merging
  - [ ] Auto-delete head branches

### 5. GitHub Actions

- [ ] Create `.github/workflows/` directory
- [ ] Add CI workflow (ci.yml)
- [ ] Add deployment workflow (deploy.yml) if applicable
- [ ] Add security scanning workflow (security.yml)
- [ ] Add dependency update workflow (dependabot.yml)

**Minimal CI Workflow:**
```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check
      - run: npm test
      - run: npm run build
```

### 6. Documentation

- [ ] **README.md** includes:
  - [ ] Project description and purpose
  - [ ] Installation instructions
  - [ ] Usage examples
  - [ ] Development setup
  - [ ] Contributing guidelines
  - [ ] Link to governance documentation
  - [ ] License information

- [ ] **CONTRIBUTING.md**:
  - [ ] Code of conduct
  - [ ] How to submit issues
  - [ ] How to submit pull requests
  - [ ] Coding standards reference
  - [ ] Testing requirements

- [ ] **docs/** directory with:
  - [ ] API documentation (if applicable)
  - [ ] Architecture overview
  - [ ] Setup guide
  - [ ] Deployment guide (if applicable)

### 7. Dependencies

- [ ] Install core dependencies:
  ```bash
  npm install typescript @types/node
  npm install -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
  npm install -D prettier eslint-config-prettier
  npm install -D vitest @vitest/ui
  npm install zod # for validation
  ```

- [ ] For frontend projects, add:
  ```bash
  npm install react react-dom
  npm install vite @vitejs/plugin-react
  npm install tailwindcss postcss autoprefixer
  ```

- [ ] All dependencies documented in README

### 8. Security

- [ ] Add .env.example files (never commit actual .env files)
- [ ] Configure Dependabot:
  ```yaml
  # .github/dependabot.yml
  version: 2
  updates:
    - package-ecosystem: "npm"
      directory: "/"
      schedule:
        interval: "weekly"
  ```

- [ ] Enable security alerts
- [ ] Enable automated security fixes
- [ ] Add SECURITY.md with vulnerability reporting process

### 9. Quality Assurance

- [ ] Set up pre-commit hooks (optional but recommended):
  ```bash
  npm install -D husky lint-staged
  npx husky init
  ```

- [ ] Configure lint-staged in package.json:
  ```json
  {
    "lint-staged": {
      "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
      "*.{json,md}": ["prettier --write"]
    }
  }
  ```

- [ ] Configure test coverage threshold:
  ```typescript
  // vitest.config.ts
  export default {
    test: {
      coverage: {
        provider: 'v8',
        reporter: ['text', 'json', 'html'],
        thresholds: {
          lines: 80,
          functions: 80,
          branches: 80,
          statements: 80
        }
      }
    }
  };
  ```

### 10. Tool Registry (if applicable)

- [ ] Create `tool-registry.json` following the schema
- [ ] Register all tools with proper metadata
- [ ] Document tool privileges and configurations
- [ ] Test tool registry loading

### 11. Environment Configuration

- [ ] Create environment config files for dev/stag/prod
- [ ] Use Zod schemas for config validation
- [ ] Document all required environment variables
- [ ] Test each environment configuration

## Post-Creation

### 12. Initial Development

- [ ] Create `develop` branch
- [ ] Set up local development environment
- [ ] Write initial tests
- [ ] Implement basic functionality
- [ ] Document code with JSDoc comments

### 13. Team Access

- [ ] Add team members with appropriate permissions
- [ ] Set up code owners (CODEOWNERS file)
- [ ] Configure notifications

### 14. Integration

- [ ] Link to project management tool (if applicable)
- [ ] Configure CI/CD pipeline
- [ ] Set up monitoring and logging (if applicable)
- [ ] Configure error tracking (if applicable)

### 15. Compliance Validation

- [ ] Run governance compliance check
- [ ] Verify all locked files are present
- [ ] Verify TypeScript strict mode enabled
- [ ] Verify no prohibited languages or patterns
- [ ] Verify naming conventions followed
- [ ] Verify environment separation implemented

### 16. First Release

- [ ] All tests passing
- [ ] Documentation complete
- [ ] Code review completed
- [ ] Security scan passed
- [ ] Performance benchmarks met (if applicable)
- [ ] Create v0.1.0 tag

## Gateway Platform Specific

For the `gateway-platform` repository specifically:

- [ ] Create complete directory structure as per GATEWAY_PLATFORM_STRUCTURE.md
- [ ] Implement MCP Server skeleton
- [ ] Set up all six base tools:
  - [ ] Gemini Search
  - [ ] Google Places Grounding Lite
  - [ ] Google Places API
  - [ ] Google Maps JavaScript
  - [ ] Google Maps UI Kit
  - [ ] Google Workspace (Gmail, Calendar, Drive, Docs, Sheets, Tasks)
- [ ] Create admin panel foundation
- [ ] Implement user management system
- [ ] Set up agent configuration system
- [ ] Configure Firebase integration
- [ ] Configure Vertex AI integration
- [ ] Configure Google Workspace integration
- [ ] Create tool registry with all required tools
- [ ] Set up memory system architecture
- [ ] Implement personality framework
- [ ] Create agent templates

## Maintenance Checklist

Ongoing maintenance tasks:

- [ ] Weekly dependency updates
- [ ] Monthly security audits
- [ ] Quarterly performance reviews
- [ ] Regular documentation updates
- [ ] Regular backup verification
- [ ] Regular compliance checks

## Resources

- [Governance Contract](./GOVERNANCE.md)
- [Coding Standards](./CODING_STANDARDS.md)
- [Architecture Guide](./ARCHITECTURE.md)
- [Gateway Platform Structure](./GATEWAY_PLATFORM_STRUCTURE.md)
- [Migration Guide](./MIGRATION_GUIDE.md)
- [Tool Registry Schema](./TOOL_REGISTRY_SCHEMA.json)

## Review and Approval

- [ ] **Self-review completed**: All items checked
- [ ] **Peer review**: Another team member verified setup
- [ ] **Architecture review**: Design approved by Architecture Review Board
- [ ] **Security review**: Security team verified configuration
- [ ] **Final approval**: Project lead or organization admin approved

---

**Created by**: _________________  
**Date**: _________________  
**Reviewed by**: _________________  
**Date**: _________________  
**Approved by**: _________________  
**Date**: _________________

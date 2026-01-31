# Gateway Global AI - Governance Contract

## Overview

This document defines the governance standards and compliance rules for all repositories within the Gateway Global AI organization. All repositories must adhere to these standards to ensure consistency, maintainability, and alignment with our architectural vision.

## Core Principles

1. **Unified Architecture**: Consolidate scattered functionalities into cohesive, well-structured repositories
2. **Technology Standardization**: TypeScript-first approach with approved technology stack
3. **Environment Separation**: Clear boundaries between development, staging, and production
4. **Enterprise-Grade Quality**: Professional coding standards, testing, and documentation
5. **Cognitive AI Focus**: Align with our mission to build AI with memory, personality, and contextual understanding

## Compliance Rules

### 1. Language and Runtime Standards

**MANDATORY:**
- TypeScript only for all new code
- Node.js LTS versions for backend services
- React 18+ with Vite for frontend applications

**PROHIBITED:**
- Python codebases (migrate to TypeScript)
- Inline CSS (use CSS modules, Tailwind, or styled-components)
- Mixed language repositories
- Unauthorized runtime stacks

### 2. Repository Structure Standards

All repositories must follow this structure:

```
repository-name/
├── src/                    # Source code
│   ├── components/        # React components (if applicable)
│   ├── services/          # Business logic and API services
│   ├── agents/            # AI agent implementations
│   ├── server/            # Server-side code
│   ├── utils/             # Utility functions
│   └── types/             # TypeScript type definitions
├── dev/                   # Development environment configs
├── stag/                  # Staging environment configs
├── prod/                  # Production environment configs
├── tests/                 # Test files
├── docs/                  # Documentation
├── .github/               # GitHub workflows and templates
├── package.json
├── tsconfig.json
├── vite.config.ts         # For frontend projects
└── README.md
```

### 3. Core File Protection

**LOCKED FILES** (Do not rename, move, or delete):
- `package.json`
- `tsconfig.json`
- `vite.config.ts` (frontend)
- `.gitignore`
- `README.md`
- Environment config files in `/dev`, `/stag`, `/prod`

### 4. Dependency Management

**Pre-Approval Required for:**
- All new NPM dependencies
- Major version updates of existing dependencies
- Alternative libraries for existing functionality

**Standard Approved Libraries:**

**Frontend:**
- React 18+
- Vite
- React Router
- Tailwind CSS or Material-UI
- Zustand or Redux Toolkit (state management)

**Backend:**
- Express.js or Fastify
- Prisma or TypeORM (databases)
- Zod (validation)
- Winston or Pino (logging)

**AI/ML:**
- @google/generative-ai (Gemini)
- @google-cloud/vertexai
- Firebase SDK
- LangChain (approved use cases only)

**Testing:**
- Vitest
- Playwright (E2E)
- Jest (legacy migration)

### 5. Naming Conventions

**Files:**
- Components: `PascalCase.tsx`
- Services: `camelCase.service.ts`
- Utilities: `camelCase.util.ts`
- Types: `PascalCase.types.ts`
- Tests: `*.test.ts` or `*.spec.ts`

**Directories:**
- `kebab-case` for all directories
- Descriptive names reflecting functionality

### 6. Code Quality Standards

**Required:**
- ESLint configuration (strict mode)
- Prettier formatting
- TypeScript strict mode enabled
- 80%+ test coverage for critical paths
- JSDoc comments for public APIs
- No `any` types (use `unknown` with type guards)

**Git Commit Standards:**
- Conventional Commits format
- Format: `type(scope): description`
- Types: feat, fix, docs, style, refactor, test, chore

### 7. Environment Configuration

**Separation Requirements:**
- Separate config files for each environment
- No hardcoded credentials or API keys
- Use environment variables
- Config schema validation with Zod

**Environment Hierarchy:**
1. **dev/**: Development and local testing
2. **stag/**: Staging for pre-production validation
3. **prod/**: Production-ready configurations

## Gateway Platform Specific Requirements

### MCP Server Implementation

**Required Base Tools:**
1. Gemini Search
2. Google Places Grounding Lite
3. Google Places API
4. Google Maps JavaScript
5. Google Maps UI Kit
6. Google Workspace (Gmail, Calendar, Drive, Docs, Sheets, Tasks)

**Tool Registry:**
- All tools must be registered in `tool-registry.json`
- Follow Tool Registry JSON schema
- Include metadata, privileges, and configurations

### Platform Economics Approach

Following platform economics principles, we leverage Google Workspace's existing MCP server infrastructure rather than building custom solutions for:
- Email communication and notifications
- Calendar and scheduling management
- Document creation and collaboration
- File storage and management
- Task tracking and organization

This approach reduces development costs, accelerates time-to-market, and provides enterprise-grade reliability for essential business functions that agents need to manage itineraries and other workflows.

### Admin/Control Panel

**Requirements:**
- React + Vite frontend
- User management interface
- Agent configuration management
- Save and share agent configurations
- Role-based access control (RBAC)

### Gemini AI Studio Alignment

**Validation Process:**
1. Prototype agent logic in Gemini AI Studio
2. Validate against "Grounded Places Expert" template
3. Test export compatibility with Firebase and Vertex AI
4. Document agent behaviors and edge cases

## Migration from Python Repositories

### Migration Path:
1. **Audit**: Identify core functionality in Python repos
2. **Design**: Create TypeScript equivalent architecture
3. **Implement**: Rewrite in TypeScript following standards
4. **Test**: Comprehensive testing against original behavior
5. **Deploy**: Gradual rollout with monitoring
6. **Archive**: Mark Python repos as deprecated

### Priority Repositories for Migration:
1. `travel-gateway-V1` (Python) → gateway-platform
2. `gateway-global-ai-browser-chat` (Python) → gateway-platform
3. Consolidate TypeScript repos: `identity-verification-mcp-gateway-gobal-ai`, `twilio-telephony-voice-ai`

## Enforcement

### Review Process:
- All PRs require governance compliance check
- Automated linting and type checking
- Manual architecture review for major changes
- Security scanning for dependencies

### Violations:
- **Minor**: Warning and request for correction
- **Major**: PR blocked until resolved
- **Critical**: Immediate rollback if in production

## Exceptions

Exception requests must include:
1. Detailed justification
2. Alternative solutions considered
3. Risk assessment
4. Mitigation plan

Submit to: Architecture Review Board

## Updates

This governance document is versioned and controlled. Updates require:
- Architecture Review Board approval
- Impact assessment
- Migration guide for affected repositories

**Version:** 1.0.0  
**Last Updated:** 2026-01-31  
**Next Review:** 2026-04-30

# Gateway Platform - Repository Structure

## Overview

The **gateway-platform** is the unified, enterprise-grade repository for Gateway Global AI Platform. It consolidates all AI agents, services, and tools into a single, well-organized TypeScript codebase.

## Directory Structure

```
gateway-platform/
├── src/                          # Source code
│   ├── components/              # React components
│   │   ├── admin/              # Admin panel components
│   │   ├── agents/             # Agent UI components
│   │   ├── chat/               # Chat interface components
│   │   ├── common/             # Shared/reusable components
│   │   ├── maps/               # Map components
│   │   ├── tasks/              # Task management components
│   │   └── travel/             # Travel-specific components
│   ├── services/                # Business logic and services
│   │   ├── conversation/       # Conversation management
│   │   ├── flights/            # Flight search and booking
│   │   ├── identity/           # Identity verification
│   │   ├── task-manager/       # Task management
│   │   ├── telephony/          # Telephony services
│   │   └── travel/             # Travel services
│   ├── agents/                  # AI agent implementations
│   │   ├── chat-agent/         # General chat agent
│   │   ├── grounded-places/    # Grounded Places Expert
│   │   ├── task-agent/         # Task management agent
│   │   ├── travel-agent/       # Travel planning agent
│   │   └── voice-agent/        # Voice interaction agent
│   ├── server/                  # Server-side code
│   │   ├── api/                # API routes and controllers
│   │   ├── middleware/         # Express/Fastify middleware
│   │   ├── websocket/          # WebSocket handlers
│   │   └── index.ts            # Server entry point
│   ├── mcp-server/              # MCP Server implementation
│   │   ├── tools/              # MCP tool implementations
│   │   │   ├── gemini-search/
│   │   │   ├── google-places/
│   │   │   ├── google-maps/
│   │   │   ├── google-workspace/ # Gmail, Calendar, Drive, Docs, Sheets, Tasks
│   │   │   ├── identity/
│   │   │   └── telephony/
│   │   ├── schemas/            # Tool schemas
│   │   ├── registry.ts         # Tool registry loader
│   │   └── server.ts           # MCP server entry point
│   ├── integrations/            # External service integrations
│   │   ├── firebase/           # Firebase integration
│   │   ├── gemini/             # Gemini AI integration
│   │   ├── workspace/          # Google Workspace integration
│   │   ├── serp/               # SERP API integration
│   │   ├── twilio/             # Twilio integration
│   │   └── vertex-ai/          # Vertex AI integration
│   ├── utils/                   # Utility functions
│   │   ├── validators/         # Validation utilities
│   │   ├── formatters/         # Data formatting
│   │   ├── logger/             # Logging utilities
│   │   └── helpers/            # General helpers
│   ├── types/                   # TypeScript type definitions
│   │   ├── agent.types.ts
│   │   ├── api.types.ts
│   │   ├── config.types.ts
│   │   ├── tool.types.ts
│   │   └── user.types.ts
│   ├── hooks/                   # React hooks (frontend)
│   ├── stores/                  # State management (Zustand/Redux)
│   ├── styles/                  # Global styles and themes
│   ├── App.tsx                  # Main React app (frontend)
│   └── main.tsx                 # Frontend entry point
├── dev/                         # Development environment
│   ├── config.ts               # Dev configuration
│   ├── .env.example            # Example environment variables
│   └── docker-compose.yml      # Local dev services
├── stag/                        # Staging environment
│   ├── config.ts               # Staging configuration
│   └── .env.example            # Staging env vars example
├── prod/                        # Production environment
│   ├── config.ts               # Production configuration
│   └── .env.example            # Production env vars example
├── tests/                       # Test files
│   ├── unit/                   # Unit tests
│   ├── integration/            # Integration tests
│   ├── e2e/                    # End-to-end tests
│   └── fixtures/               # Test fixtures and mocks
├── docs/                        # Documentation
│   ├── api/                    # API documentation
│   ├── agents/                 # Agent documentation
│   ├── architecture/           # Architecture diagrams
│   ├── guides/                 # User guides
│   └── setup/                  # Setup instructions
├── scripts/                     # Build and deployment scripts
│   ├── build.ts                # Build script
│   ├── deploy.ts               # Deployment script
│   └── migrate.ts              # Migration scripts
├── .github/                     # GitHub workflows and templates
│   ├── workflows/
│   │   ├── ci.yml              # Continuous integration
│   │   ├── deploy.yml          # Deployment workflow
│   │   └── security.yml        # Security scanning
│   └── ISSUE_TEMPLATE/
├── public/                      # Public static assets (frontend)
├── tool-registry.json           # Tool registry (follows schema)
├── package.json                 # NPM dependencies and scripts
├── tsconfig.json                # TypeScript configuration
├── vite.config.ts               # Vite configuration (frontend)
├── vitest.config.ts             # Vitest configuration
├── .eslintrc.json               # ESLint configuration
├── .prettierrc                  # Prettier configuration
├── .gitignore                   # Git ignore rules
├── Dockerfile                   # Docker container definition
├── docker-compose.yml           # Docker compose for services
└── README.md                    # Project documentation
```

## Component Organization

### Admin Components (`src/components/admin/`)
- **UserManagement.tsx** - User CRUD operations
- **AgentConfigurator.tsx** - Agent configuration interface
- **ToolRegistry.tsx** - Tool management UI
- **Dashboard.tsx** - Admin dashboard
- **RoleManager.tsx** - Role-based access control

### Agent Components (`src/components/agents/`)
- **AgentCard.tsx** - Agent display card
- **AgentSelector.tsx** - Agent selection interface
- **ConfigPanel.tsx** - Agent configuration panel
- **ShareAgent.tsx** - Share agent configurations

### Common Components (`src/components/common/`)
- **Button.tsx** - Styled button component
- **Input.tsx** - Form input component
- **Modal.tsx** - Modal dialog
- **Toast.tsx** - Toast notifications
- **Loader.tsx** - Loading indicators

## Service Layer Architecture

### Service Pattern
```typescript
// src/services/example/example.service.ts
export class ExampleService {
  private logger: Logger;
  private config: ServiceConfig;

  constructor(config: ServiceConfig) {
    this.config = config;
    this.logger = createLogger('ExampleService');
  }

  async performAction(params: ActionParams): Promise<ActionResult> {
    // Validate
    this.validateParams(params);
    
    // Execute
    try {
      const result = await this.executeAction(params);
      this.logger.info('Action completed', { params, result });
      return result;
    } catch (error) {
      this.logger.error('Action failed', { params, error });
      throw error;
    }
  }

  private validateParams(params: ActionParams): void {
    // Zod validation
  }

  private async executeAction(params: ActionParams): Promise<ActionResult> {
    // Implementation
  }
}
```

## Agent Architecture

### Agent Structure
```typescript
// src/agents/example-agent/index.ts
export class ExampleAgent implements Agent {
  private memory: MemorySystem;
  private personality: PersonalityProfile;
  private tools: ToolRegistry;

  constructor(config: AgentConfig) {
    this.memory = new MemorySystem(config.memory);
    this.personality = config.personality;
    this.tools = new ToolRegistry(config.tools);
  }

  async processMessage(message: string, context: Context): Promise<Response> {
    // 1. Retrieve relevant memories
    const memories = await this.memory.retrieve(message);
    
    // 2. Apply personality filters
    const personalizedContext = this.personality.apply(context, memories);
    
    // 3. Use tools if needed
    const toolResults = await this.tools.execute(message, personalizedContext);
    
    // 4. Generate response
    const response = await this.generateResponse(message, personalizedContext, toolResults);
    
    // 5. Store interaction in memory
    await this.memory.store({ message, response, context });
    
    return response;
  }
}
```

## MCP Server Implementation

### MCP Tool Structure
```typescript
// src/mcp-server/tools/example-tool/index.ts
import { Tool, ToolConfig, ToolResult } from '@/types/tool.types';

export class ExampleTool implements Tool {
  readonly id: string;
  readonly name: string;
  readonly description: string;
  private config: ToolConfig;

  constructor(config: ToolConfig) {
    this.id = config.id;
    this.name = config.name;
    this.description = config.description;
    this.config = config;
  }

  async execute(params: Record<string, unknown>): Promise<ToolResult> {
    // Validate privileges
    this.validatePrivileges(params.userId);
    
    // Validate parameters
    this.validateParams(params);
    
    // Execute tool logic
    const result = await this.performAction(params);
    
    return {
      success: true,
      data: result,
      metadata: {
        toolId: this.id,
        timestamp: new Date().toISOString()
      }
    };
  }

  private validatePrivileges(userId: string): void {
    // Check user roles and permissions
  }

  private validateParams(params: Record<string, unknown>): void {
    // Zod schema validation
  }

  private async performAction(params: Record<string, unknown>): Promise<unknown> {
    // Tool-specific implementation
  }
}
```

## Configuration Management

### Environment-Specific Configs

**Development (`dev/config.ts`):**
```typescript
import { z } from 'zod';

export const devConfig = {
  server: {
    port: 3000,
    host: 'localhost'
  },
  database: {
    url: process.env.DATABASE_URL || 'postgresql://localhost/gateway_dev'
  },
  gemini: {
    apiKey: process.env.GEMINI_API_KEY_DEV!,
    model: 'gemini-pro'
  },
  logging: {
    level: 'debug'
  },
  features: {
    enableDebugTools: true,
    enableMockData: true
  }
};
```

**Production (`prod/config.ts`):**
```typescript
export const prodConfig = {
  server: {
    port: parseInt(process.env.PORT || '8080'),
    host: '0.0.0.0'
  },
  database: {
    url: process.env.DATABASE_URL!,
    ssl: true,
    poolSize: 20
  },
  gemini: {
    apiKey: process.env.GEMINI_API_KEY_PROD!,
    model: 'gemini-pro'
  },
  logging: {
    level: 'info'
  },
  features: {
    enableDebugTools: false,
    enableMockData: false
  }
};
```

## NPM Scripts

**package.json:**
```json
{
  "scripts": {
    "dev": "vite",
    "dev:server": "tsx watch src/server/index.ts",
    "build": "tsc && vite build",
    "build:server": "tsc -p tsconfig.server.json",
    "test": "vitest",
    "test:unit": "vitest run tests/unit",
    "test:integration": "vitest run tests/integration",
    "test:e2e": "playwright test",
    "lint": "eslint src --ext ts,tsx",
    "lint:fix": "eslint src --ext ts,tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx}\"",
    "type-check": "tsc --noEmit",
    "start": "node dist/server/index.js",
    "start:mcp": "node dist/mcp-server/server.js"
  }
}
```

## Docker Configuration

**Dockerfile:**
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
EXPOSE 8080
CMD ["npm", "start"]
```

## CI/CD Pipeline

### GitHub Actions Workflow
```yaml
name: CI/CD

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
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check
      - run: npm run test:unit
      - run: npm run build

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to production
        run: npm run deploy:prod
```

## Getting Started

### Initial Setup
```bash
# Clone repository
git clone https://github.com/gateway-global-ai/gateway-platform.git
cd gateway-platform

# Install dependencies
npm install

# Set up environment variables
cp dev/.env.example .env

# Run database migrations
npm run migrate

# Start development server
npm run dev

# In another terminal, start backend server
npm run dev:server
```

### Development Workflow
1. Create feature branch: `git checkout -b feature/my-feature`
2. Make changes following governance standards
3. Write tests for new functionality
4. Run linting and tests: `npm run lint && npm test`
5. Commit using conventional commits: `git commit -m "feat: add new feature"`
6. Push and create pull request
7. Address review feedback
8. Merge after approval

## Security

- All API keys in environment variables
- No secrets in code or git history
- HTTPS only for production
- Rate limiting on all endpoints
- Input validation with Zod
- SQL injection prevention with ORM
- XSS protection in React
- CSRF tokens for forms
- Regular dependency audits

## Monitoring

- Application logs with Winston/Pino
- Error tracking with Sentry
- Performance monitoring with Google Cloud Monitoring
- Usage analytics with Google Analytics
- API metrics with custom dashboards

## Maintenance

- Weekly dependency updates
- Monthly security audits
- Quarterly performance reviews
- Continuous documentation updates
- Regular backups and disaster recovery tests

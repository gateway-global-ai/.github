# Gateway Global AI Platform - Organization Overview

## Mission

**Building AI with Cognitive Continuity**

Gateway Global AI pioneers the cognitive architecture for machines that think, remember, and relate. We integrate the foundational principles of human psychology—memory, personality, belief systems, and contextual understanding—into practical, enterprise-ready AI systems.

## Core Belief

The next leap in AI isn't about larger models—it's about deeper minds. We focus on cognitive continuity, creating AI that doesn't just answer queries but understands journeys, learns from each interaction, and builds persistent, evolving relationships with users.

## What We Build

### 🧠 Cognitive AI Platform
- **Memory Systems**: Temporal knowledge graphs for short and long-term memory
- **Personality Framework**: DISC-based consistent personalities
- **Contextual Understanding**: Continuous learning across conversations
- **Loyal Digital Persons**: AI with consistent values and behaviors

### 🛠️ Gateway Platform
Our unified, enterprise-grade TypeScript platform that consolidates:
- AI agents with memory and personality
- MCP (Model Context Protocol) server with extensible tools
- Admin/control panel for agent management
- Integration with Google's Gemini AI and Vertex AI
- Firebase-powered data persistence

### 🔧 Core Tools
1. **Gemini Search** - AI-powered search with grounding
2. **Google Places Integration** - Location-based context
3. **Google Maps** - Interactive mapping and UI components
4. **Google Workspace** - Productivity suite (Gmail, Calendar, Drive, Docs, Sheets, Tasks)
5. **Identity Verification** - Secure user authentication
6. **Telephony AI** - Voice interaction capabilities

### 📦 Repository Structure

### Primary Repositories

#### [gateway-platform](https://github.com/gateway-global-ai/gateway-platform)
The main unified platform containing:
- TypeScript-only codebase
- React + Vite frontend
- MCP Server implementation
- AI agents with cognitive architecture
- Environment separation (dev/stag/prod)

#### [.github](https://github.com/gateway-global-ai/.github)
Organization governance and standards:
- Governance contract and compliance rules
- Coding standards and best practices
- Architecture guides
- Tool registry schema
- Migration documentation

#### [workspace](https://github.com/gateway-global-ai/workspace)
Google Workspace MCP Server integration:
- Gmail integration for communications
- Calendar management for scheduling
- Drive for file storage and management
- Docs and Sheets for document creation
- Task management
- **Platform Economics**: Leveraging Google's infrastructure instead of building custom solutions

### Legacy Repositories (Being Consolidated)

These repositories are being migrated to `gateway-platform`:
- `travel-gateway-V1` (Python) → Migrating to TypeScript
- `gateway-global-ai-browser-chat` (Python) → Migrating to TypeScript
- `identity-verification-mcp-gateway-gobal-ai` (TypeScript) → Consolidating
- `twilio-telephony-voice-ai` (TypeScript) → Consolidating
- `serp-flights-server-gateway-global-ai` → Consolidating
- `ai-task-manager-gateway-global` → Consolidating

## Governance

All repositories must adhere to our [Governance Contract](./GOVERNANCE.md):

### Technology Standards
✅ **Required:**
- TypeScript only (strict mode)
- React 18+ with Vite for frontend
- Node.js LTS for backend
- Pre-approved dependencies

❌ **Prohibited:**
- Python or other languages in new code
- Inline CSS
- `any` types in TypeScript
- Unauthorized runtime stacks

### Architecture Standards
- Clear environment separation (/dev, /stag, /prod)
- Organized /src structure (components, services, agents, server)
- Type-safe configuration with Zod validation
- Comprehensive testing (80%+ coverage)

### Quality Standards
- ESLint (strict) + Prettier
- Conventional commits
- Pull request reviews required
- Security scanning
- Performance monitoring

## Getting Started

### For Developers

1. **Read the Governance**
   - [Governance Contract](./GOVERNANCE.md)
   - [Coding Standards](./CODING_STANDARDS.md)
   - [Architecture Guide](./ARCHITECTURE.md)

2. **Set Up Your Environment**
   - Node.js 20+ LTS
   - TypeScript 5+
   - Git with conventional commits

3. **Clone a Repository**
   ```bash
   git clone https://github.com/gateway-global-ai/gateway-platform.git
   cd gateway-platform
   npm install
   ```

4. **Follow the Standards**
   - Use TypeScript strict mode
   - Write tests for all new code
   - Follow naming conventions
   - Document with JSDoc

### For Contributors

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make changes following our standards
4. Write tests
5. Run linting and tests: `npm run lint && npm test`
6. Commit: `git commit -m "feat: add new feature"`
7. Push and create pull request
8. Address review feedback

## Documentation

### Governance & Standards
- [Governance Contract](./GOVERNANCE.md) - Compliance rules and standards
- [Coding Standards](./CODING_STANDARDS.md) - TypeScript and React best practices
- [Architecture Guide](./ARCHITECTURE.md) - System architecture and design patterns

### Platform Documentation
- [Gateway Platform Structure](./GATEWAY_PLATFORM_STRUCTURE.md) - Repository structure
- [Migration Guide](./MIGRATION_GUIDE.md) - Migrating from legacy repos
- [Gemini AI Studio Integration](./GEMINI_AI_STUDIO_INTEGRATION.md) - AI Studio workflow

### Tools & Templates
- [Tool Registry Schema](./TOOL_REGISTRY_SCHEMA.json) - Tool metadata schema
- [Tool Registry Example](./TOOL_REGISTRY_EXAMPLE.md) - Example tool registrations
- [Repository Creation Checklist](./workflow-templates/REPOSITORY_CREATION_CHECKLIST.md)
- [Governance Compliance Workflow](./workflow-templates/governance-compliance.yml)

## Key Features

### Memory System
Persistent, evolving memory across conversations:
- **Short-term**: Recent conversation context
- **Long-term**: User preferences and learned facts
- **Episodic**: Significant events and experiences
- **Semantic**: General knowledge and concepts

### Personality Framework
DISC-based personalities for consistent agent behavior:
- **Dominance**: Direct, results-oriented
- **Influence**: Outgoing, enthusiastic
- **Steadiness**: Patient, supportive
- **Conscientiousness**: Accurate, analytical

### MCP Server
Model Context Protocol server with extensible tools:
- Standardized tool interface
- Role-based access control
- Rate limiting and cost tracking
- Environment-specific configurations

### Admin Panel
Comprehensive management interface:
- User management
- Agent configuration
- Tool registry management
- Save and share agent configs
- Role-based access control

## Technology Stack

### Frontend
- **React 18+** with TypeScript
- **Vite** for build tooling
- **Tailwind CSS** or Material-UI for styling
- **Zustand** or Redux Toolkit for state management

### Backend
- **Node.js 20+ LTS**
- **Express.js** or Fastify
- **TypeScript** strict mode
- **Zod** for validation

### AI/ML
- **Google Gemini** for generative AI
- **Vertex AI** for enterprise deployment
- **Firebase** for data persistence
- **Vector databases** for embeddings

### Testing
- **Vitest** for unit and integration tests
- **Playwright** for E2E tests
- **80%+ coverage** requirement

## Roadmap

### Q1 2026
- [x] Establish governance framework
- [x] Create comprehensive documentation
- [ ] Complete gateway-platform repository setup
- [ ] Migrate core functionality from Python repos

### Q2 2026
- [ ] Complete MCP server implementation
- [ ] Deploy admin panel
- [ ] Integrate all five base tools
- [ ] Migrate remaining repositories

### Q3 2026
- [ ] Production deployment
- [ ] Performance optimization
- [ ] Advanced memory features
- [ ] Agent marketplace

### Q4 2026
- [ ] Enterprise features
- [ ] Advanced analytics
- [ ] Multi-language support
- [ ] SDK and API releases

## Community

### Contact
- **Email**: contact@gateway-global-ai.com
- **Architecture Questions**: architecture-team@gateway-global-ai.com
- **Technical Support**: tech-support@gateway-global-ai.com

### Contributing
We welcome contributions! Please read our:
- [Governance Contract](./GOVERNANCE.md)
- [Coding Standards](./CODING_STANDARDS.md)
- [Repository Creation Checklist](./workflow-templates/REPOSITORY_CREATION_CHECKLIST.md)

### Code of Conduct
We are committed to providing a welcoming and inclusive environment. Be respectful, professional, and collaborative.

## License

See individual repositories for license information.

## About the Founder

Founded by **Jason Trindade**, a developer whose journey from platform APIs to the psychology of memory exemplifies a deliberate quest to build AI that is fundamentally human-aware.

## Vision

Gateway Global AI is for those who believe the future of technology is not just smart, but thoughtful. We're building minds, not just models.

---

**Let's architect the conscious machine together.**

*Last Updated: January 31, 2026*

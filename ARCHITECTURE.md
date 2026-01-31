# Architecture Guide - Gateway Global AI Platform

## Overview

This document defines the enterprise-grade architecture for the Gateway Global AI Platform, emphasizing our unique approach to building AI with cognitive continuity, memory systems, and personality frameworks.

## Architectural Principles

### 1. Cognitive Architecture First
We build AI that thinks, remembers, and relates—not just completes tasks.

**Core Components:**
- **Memory Systems**: Temporal knowledge graphs for short and long-term memory
- **Personality Framework**: DISC-based personality profiles for consistent agent behavior
- **Belief Systems**: Value alignment and contextual understanding
- **Cognitive Continuity**: Persistent, evolving relationships with users

### 2. Modular and Scalable
- Microservices architecture where appropriate
- Clear separation of concerns
- Independent scaling of components
- Plug-and-play tool system

### 3. Enterprise-Grade Quality
- Type-safe TypeScript throughout
- Comprehensive testing (unit, integration, E2E)
- Security-first design
- Performance monitoring and optimization

### 4. Cost-Efficient and Sovereign
- Minimize external API dependencies where possible
- Self-hosted services for critical paths
- Efficient token usage strategies
- Caching and optimization

## System Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Frontend Layer                          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │  Admin Panel │  │  Chat UI     │  │  Agent Config│         │
│  │  (React)     │  │  (React)     │  │  (React)     │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└───────────────────────────┬─────────────────────────────────────┘
                            │ HTTPS / WebSocket
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                         API Gateway                             │
│                      (Express/Fastify)                          │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐               │
│  │   Auth     │  │   Rate     │  │  Logging   │               │
│  │ Middleware │  │  Limiting  │  │ Middleware │               │
│  └────────────┘  └────────────┘  └────────────┘               │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Business Logic Layer                       │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Agent Orchestrator                     │  │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐         │  │
│  │  │   Travel   │  │   Chat     │  │   Voice    │         │  │
│  │  │   Agent    │  │   Agent    │  │   Agent    │         │  │
│  │  └────────────┘  └────────────┘  └────────────┘         │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                      MCP Server                           │  │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐         │  │
│  │  │   Gemini   │  │   Places   │  │    Maps    │         │  │
│  │  │   Search   │  │    API     │  │    API     │         │  │
│  │  └────────────┘  └────────────┘  └────────────┘         │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Service Layer                          │  │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────┐         │  │
│  │  │Conversation│  │   Memory   │  │   Task     │         │  │
│  │  │  Service   │  │  Service   │  │  Manager   │         │  │
│  │  └────────────┘  └────────────┘  └────────────┘         │  │
│  └──────────────────────────────────────────────────────────┘  │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                        Data Layer                               │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐               │
│  │  Firebase  │  │   Redis    │  │ Vector DB  │               │
│  │ Firestore  │  │   Cache    │  │ (Embeddings│               │
│  └────────────┘  └────────────┘  └────────────┘               │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    External Services                            │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐               │
│  │   Gemini   │  │ Vertex AI  │  │   Twilio   │               │
│  │    API     │  │            │  │            │               │
│  └────────────┘  └────────────┘  └────────────┘               │
└─────────────────────────────────────────────────────────────────┘
```

## Core Components

### 1. Memory System

Our cognitive architecture relies on a sophisticated memory system inspired by human memory.

**Architecture:**
```typescript
interface MemorySystem {
  shortTerm: ShortTermMemory;
  longTerm: LongTermMemory;
  episodic: EpisodicMemory;
  semantic: SemanticMemory;
  working: WorkingMemory;
}

// Short-term memory: Recent conversation context
interface ShortTermMemory {
  messages: Message[];
  maxSize: number; // e.g., last 20 messages
  ttl: number; // Time to live in seconds
}

// Long-term memory: Persistent knowledge about user
interface LongTermMemory {
  facts: TemporalKnowledgeGraph;
  preferences: UserPreferences;
  relationships: RelationshipMap;
}

// Episodic memory: Specific events and experiences
interface EpisodicMemory {
  events: Event[];
  significance: number; // 0-1 score for retention
  timestamp: Date;
}

// Semantic memory: General knowledge and concepts
interface SemanticMemory {
  concepts: ConceptGraph;
  categories: Category[];
}

// Working memory: Currently active processing
interface WorkingMemory {
  activeGoals: Goal[];
  contextWindow: Context;
  toolResults: ToolResult[];
}
```

**Temporal Knowledge Graph:**
```typescript
interface TemporalKnowledgeGraph {
  nodes: Array<{
    id: string;
    type: 'fact' | 'belief' | 'preference' | 'event';
    content: string;
    confidence: number; // 0-1
    timestamp: Date;
    lastAccessed: Date;
  }>;
  
  edges: Array<{
    from: string;
    to: string;
    relationship: string;
    strength: number; // 0-1
  }>;
}
```

**Implementation:**
```typescript
// src/services/memory/MemorySystem.ts
export class MemorySystem {
  private firestore: FirestoreService;
  private vectorDb: VectorDbService;
  private redis: RedisCache;

  async store(interaction: Interaction): Promise<void> {
    // 1. Add to short-term memory (Redis)
    await this.redis.lpush(
      `memory:short:${interaction.userId}`,
      JSON.stringify(interaction)
    );
    await this.redis.ltrim(`memory:short:${interaction.userId}`, 0, 19);

    // 2. Extract significant information
    const facts = await this.extractFacts(interaction);
    
    // 3. Store in long-term memory (Firestore)
    for (const fact of facts) {
      await this.firestore.saveToKnowledgeGraph(
        interaction.userId,
        fact
      );
    }

    // 4. Generate embeddings for semantic search
    const embedding = await this.generateEmbedding(interaction.message);
    await this.vectorDb.store({
      userId: interaction.userId,
      embedding,
      metadata: interaction
    });
  }

  async retrieve(query: string, userId: string): Promise<Context> {
    // 1. Get recent short-term memories
    const shortTerm = await this.redis.lrange(
      `memory:short:${userId}`,
      0,
      19
    );

    // 2. Semantic search in long-term memory
    const queryEmbedding = await this.generateEmbedding(query);
    const semanticMatches = await this.vectorDb.search(
      queryEmbedding,
      userId,
      { topK: 5 }
    );

    // 3. Combine and rank
    return {
      shortTerm: shortTerm.map(m => JSON.parse(m)),
      relevant: semanticMatches,
      knowledgeGraph: await this.getRelevantFacts(userId, query)
    };
  }

  private async extractFacts(
    interaction: Interaction
  ): Promise<KnowledgeFact[]> {
    // Use LLM to extract structured facts from conversation
    const prompt = `Extract key facts and preferences from this conversation:
    
User: ${interaction.message}
Assistant: ${interaction.response}

Return as JSON array of facts with type, content, and confidence.`;

    const result = await geminiService.extractStructuredData(prompt);
    return result.facts;
  }
}
```

### 2. Personality Framework (DISC)

Agents have consistent personalities based on the DISC model.

**DISC Components:**
```typescript
interface PersonalityProfile {
  dominance: number; // 0-1: Direct, results-oriented
  influence: number; // 0-1: Outgoing, enthusiastic
  steadiness: number; // 0-1: Patient, supportive
  conscientiousness: number; // 0-1: Accurate, analytical
}

// Example personalities
const profiles = {
  helpfulAssistant: {
    dominance: 0.3,
    influence: 0.7,
    steadiness: 0.8,
    conscientiousness: 0.6
  },
  
  executiveAgent: {
    dominance: 0.8,
    influence: 0.4,
    steadiness: 0.3,
    conscientiousness: 0.7
  },
  
  creativePartner: {
    dominance: 0.4,
    influence: 0.9,
    steadiness: 0.5,
    conscientiousness: 0.4
  }
};
```

**Personality Application:**
```typescript
class PersonalityFilter {
  apply(
    response: string, 
    profile: PersonalityProfile
  ): string {
    let filtered = response;

    // High dominance: More direct, fewer qualifiers
    if (profile.dominance > 0.7) {
      filtered = this.makeMoreDirect(filtered);
    }

    // High influence: More enthusiastic, friendly
    if (profile.influence > 0.7) {
      filtered = this.addEnthusiasm(filtered);
    }

    // High steadiness: More supportive, patient
    if (profile.steadiness > 0.7) {
      filtered = this.addSupportiveness(filtered);
    }

    // High conscientiousness: More detailed, precise
    if (profile.conscientiousness > 0.7) {
      filtered = this.addDetail(filtered);
    }

    return filtered;
  }
}
```

### 3. Agent Orchestration

**Agent Interface:**
```typescript
interface Agent {
  id: string;
  name: string;
  personality: PersonalityProfile;
  memory: MemorySystem;
  tools: Tool[];
  
  processMessage(message: Message, context: Context): Promise<Response>;
  initialize(config: AgentConfig): Promise<void>;
  shutdown(): Promise<void>;
}
```

**Agent Manager:**
```typescript
export class AgentManager {
  private agents: Map<string, Agent>;
  private activeConversations: Map<string, string>; // conversationId -> agentId

  async routeMessage(
    message: Message,
    conversationId: string
  ): Promise<Response> {
    // 1. Determine which agent handles this conversation
    const agentId = this.activeConversations.get(conversationId) 
      || await this.selectAgent(message);

    // 2. Get or create agent instance
    const agent = await this.getAgent(agentId);

    // 3. Process message
    const response = await agent.processMessage(
      message,
      await this.buildContext(conversationId)
    );

    // 4. Track conversation
    this.activeConversations.set(conversationId, agentId);

    return response;
  }

  private async selectAgent(message: Message): Promise<string> {
    // Use intent classification to select appropriate agent
    const intent = await this.classifyIntent(message.content);
    
    const agentMap = {
      travel: 'travel-agent',
      task: 'task-agent',
      chat: 'chat-agent',
      voice: 'voice-agent'
    };

    return agentMap[intent] || 'chat-agent';
  }
}
```

### 4. MCP Server Architecture

**Tool Registry:**
```typescript
export class ToolRegistry {
  private tools: Map<string, Tool>;
  private config: ToolRegistryConfig;

  async loadFromFile(path: string): Promise<void> {
    const data = await fs.readFile(path, 'utf-8');
    const registry = JSON.parse(data);
    
    for (const toolConfig of registry.tools) {
      const tool = await this.instantiateTool(toolConfig);
      this.tools.set(toolConfig.id, tool);
    }
  }

  async execute(
    toolId: string,
    params: Record<string, unknown>,
    context: ExecutionContext
  ): Promise<ToolResult> {
    const tool = this.tools.get(toolId);
    
    if (!tool) {
      throw new ToolNotFoundError(toolId);
    }

    // Check privileges
    if (!this.hasPrivileges(context.userId, tool)) {
      throw new InsufficientPrivilegesError(toolId, context.userId);
    }

    // Rate limiting
    await this.checkRateLimit(context.userId, toolId);

    // Execute with timeout
    return await this.executeWithTimeout(
      () => tool.execute(params),
      tool.config.timeout
    );
  }

  private hasPrivileges(userId: string, tool: Tool): boolean {
    const userRoles = this.getUserRoles(userId);
    return tool.privileges.requiredRoles.some(role => 
      userRoles.includes(role)
    );
  }
}
```

### 5. Data Flow

**Request Flow:**
```
User Input
    ↓
Frontend (React)
    ↓
WebSocket/HTTP
    ↓
API Gateway (Auth, Rate Limit)
    ↓
Agent Manager (Route to appropriate agent)
    ↓
Agent (Retrieve memory, process with personality)
    ↓
MCP Server (Execute tools if needed)
    ↓
External Services (Gemini, Places API, etc.)
    ↓
Memory System (Store interaction)
    ↓
Response to User
```

## Security Architecture

### Authentication & Authorization

```typescript
// JWT-based auth
interface AuthToken {
  userId: string;
  roles: string[];
  sessionId: string;
  exp: number;
}

// Role-based access control
const permissions = {
  admin: ['*'],
  developer: ['read:agents', 'write:agents', 'read:tools', 'write:tools'],
  user: ['read:agents', 'execute:agents'],
  guest: ['execute:agents']
};

// Middleware
async function requireAuth(req, res, next) {
  const token = extractToken(req);
  const decoded = await verifyToken(token);
  req.user = decoded;
  next();
}

async function requireRole(role: string) {
  return async (req, res, next) => {
    if (!req.user.roles.includes(role)) {
      throw new ForbiddenError();
    }
    next();
  };
}
```

### Data Security

- **Encryption at rest**: Firebase encryption
- **Encryption in transit**: HTTPS/TLS 1.3
- **API key management**: Environment variables, secret rotation
- **Input validation**: Zod schemas on all inputs
- **SQL injection prevention**: ORM with parameterized queries
- **XSS prevention**: React auto-escaping, DOMPurify for HTML

## Performance Optimization

### Caching Strategy

```typescript
// Multi-layer caching
class CacheManager {
  // L1: In-memory (fastest)
  private memoryCache = new Map<string, CacheEntry>();

  // L2: Redis (shared across instances)
  private redis: RedisClient;

  // L3: Firestore (persistent)
  private firestore: FirestoreClient;

  async get<T>(key: string): Promise<T | null> {
    // Try memory first
    if (this.memoryCache.has(key)) {
      return this.memoryCache.get(key).value as T;
    }

    // Try Redis
    const redisValue = await this.redis.get(key);
    if (redisValue) {
      this.memoryCache.set(key, {value: redisValue, ttl: 60});
      return JSON.parse(redisValue) as T;
    }

    // Try Firestore
    const firestoreValue = await this.firestore.get(key);
    if (firestoreValue) {
      await this.redis.set(key, JSON.stringify(firestoreValue), 'EX', 3600);
      this.memoryCache.set(key, {value: firestoreValue, ttl: 60});
      return firestoreValue as T;
    }

    return null;
  }
}
```

### Token Optimization

```typescript
// Efficient prompt engineering
class PromptOptimizer {
  optimize(context: Context): OptimizedContext {
    // Summarize old messages
    const summarized = this.summarizeHistory(context.messages);
    
    // Extract key facts
    const keyFacts = this.extractKeyFacts(context);
    
    // Remove redundancy
    const deduplicated = this.deduplicate(keyFacts);
    
    return {
      summary: summarized,
      facts: deduplicated,
      recent: context.messages.slice(-5) // Last 5 messages
    };
  }
}
```

## Monitoring and Observability

```typescript
// Logging
logger.info('Agent response generated', {
  agentId,
  userId,
  responseTime: endTime - startTime,
  tokenUsage: response.usage,
  groundingUsed: response.groundingMetadata !== null
});

// Metrics
metrics.recordHistogram('agent.response.time', responseTime);
metrics.incrementCounter('agent.requests', { agentId });
metrics.recordGauge('memory.size', memorySize);

// Distributed tracing
const span = tracer.startSpan('agent.processMessage');
span.setAttributes({ agentId, userId });
// ... process message ...
span.end();
```

## Deployment Architecture

### Environment Configuration

**Development:**
- Local Docker containers
- Hot reload for rapid development
- Mock external services
- Debug logging enabled

**Staging:**
- Cloud-hosted (Firebase/Google Cloud)
- Real external services
- Production-like configuration
- Performance monitoring

**Production:**
- Multi-region deployment
- Auto-scaling
- CDN for static assets
- Full monitoring and alerting

### CI/CD Pipeline

```yaml
Development → Staging → Production

Gates:
- All tests pass
- No security vulnerabilities
- Performance benchmarks met
- Code review approved
- Governance compliance check
```

## Scalability Strategy

1. **Horizontal Scaling**: Multiple server instances behind load balancer
2. **Caching**: Reduce database and API calls
3. **Async Processing**: Queue for non-critical operations
4. **Database Optimization**: Indexes, query optimization
5. **CDN**: Static assets and API responses where appropriate

**Version:** 1.0.0  
**Last Updated:** 2026-01-31

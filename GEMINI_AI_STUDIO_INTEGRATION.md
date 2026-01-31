# Gemini AI Studio Integration Guide

## Overview

This guide outlines the integration between Gateway Global AI Platform and Google's Gemini AI Studio, ensuring seamless development, testing, and deployment of AI agents aligned with our cognitive architecture principles.

## Core Principles

1. **Prototype First**: Validate agent logic in Gemini AI Studio before implementation
2. **Export Compatibility**: Design agents for easy export to production codebase
3. **Firebase Integration**: Leverage Firebase for data persistence and authentication
4. **Vertex AI Alignment**: Ensure compatibility with Vertex AI for enterprise deployment

## Gemini AI Studio Workflow

### Phase 1: Prototyping in AI Studio

**1. Agent Design:**
- Define agent personality using DISC framework
- Configure system instructions
- Set up grounding sources
- Test conversational flows

**2. Tool Configuration:**
- Configure Google Search grounding
- Set up Places API grounding
- Add custom function calls
- Test tool interactions

**3. Testing Scenarios:**
- Create test conversation sets
- Validate response quality
- Test edge cases
- Document agent behaviors

### Phase 2: Export to Platform

**1. Extract Configuration:**
```typescript
// src/agents/grounded-places/config.ts
export const groundedPlacesConfig = {
  name: 'Grounded Places Expert',
  model: 'gemini-pro',
  systemInstruction: `You are a helpful travel and location expert...`,
  
  personality: {
    type: 'DISC',
    profile: {
      dominance: 0.3,
      influence: 0.7,
      steadiness: 0.6,
      conscientiousness: 0.5
    }
  },
  
  grounding: {
    googleSearch: {
      enabled: true,
      dynamicRetrieval: true
    },
    places: {
      enabled: true,
      radius: 5000
    }
  },
  
  tools: [
    'gemini-search',
    'google-places-grounding-lite',
    'google-places-api'
  ],
  
  parameters: {
    temperature: 0.7,
    topP: 0.95,
    topK: 40,
    maxOutputTokens: 2048
  }
};
```

**2. Implement Agent Class:**
```typescript
// src/agents/grounded-places/GroundedPlacesAgent.ts
import { Agent, AgentConfig, Message, Response } from '@/types/agent.types';
import { ToolRegistry } from '@/mcp-server/registry';
import { MemorySystem } from '@/services/memory';
import { geminiService } from '@/integrations/gemini';

export class GroundedPlacesAgent implements Agent {
  private config: AgentConfig;
  private tools: ToolRegistry;
  private memory: MemorySystem;

  constructor(config: AgentConfig) {
    this.config = config;
    this.tools = new ToolRegistry(config.tools);
    this.memory = new MemorySystem(config.memory);
  }

  async processMessage(message: Message): Promise<Response> {
    // 1. Retrieve relevant memories
    const context = await this.memory.retrieve(message.content);

    // 2. Prepare grounding context
    const groundingContext = await this.prepareGrounding(message);

    // 3. Generate response using Gemini
    const response = await geminiService.generateContent({
      model: this.config.model,
      systemInstruction: this.config.systemInstruction,
      contents: [
        ...context.history,
        { role: 'user', parts: [{ text: message.content }] }
      ],
      groundingConfig: groundingContext,
      generationConfig: this.config.parameters
    });

    // 4. Store interaction in memory
    await this.memory.store({
      message,
      response: response.text,
      timestamp: new Date()
    });

    return {
      text: response.text,
      groundingMetadata: response.groundingMetadata,
      finishReason: response.finishReason
    };
  }

  private async prepareGrounding(message: Message): Promise<GroundingConfig> {
    const location = await this.extractLocation(message.content);
    
    return {
      googleSearch: this.config.grounding.googleSearch,
      places: location ? {
        ...this.config.grounding.places,
        location: location.coordinates
      } : undefined
    };
  }

  private async extractLocation(text: string): Promise<Location | null> {
    // Use tools to extract location from message
    const placeTool = await this.tools.getTool('google-places-api');
    return placeTool.extractLocation(text);
  }
}
```

### Phase 3: Testing and Validation

**Test Suite Based on AI Studio Scenarios:**
```typescript
// tests/agents/grounded-places.test.ts
import { describe, it, expect, beforeEach } from 'vitest';
import { GroundedPlacesAgent } from '@/agents/grounded-places';
import { groundedPlacesConfig } from '@/agents/grounded-places/config';

describe('GroundedPlacesAgent', () => {
  let agent: GroundedPlacesAgent;

  beforeEach(() => {
    agent = new GroundedPlacesAgent(groundedPlacesConfig);
  });

  describe('Place Recommendations', () => {
    it('should recommend restaurants near a location', async () => {
      const message = {
        content: 'What are the best Italian restaurants near Times Square?',
        userId: 'test-user',
        timestamp: new Date()
      };

      const response = await agent.processMessage(message);

      expect(response.text).toBeDefined();
      expect(response.text.toLowerCase()).toContain('restaurant');
      expect(response.groundingMetadata).toBeDefined();
    });

    it('should handle vague location queries', async () => {
      const message = {
        content: 'Where can I get good coffee around here?',
        userId: 'test-user',
        timestamp: new Date()
      };

      const response = await agent.processMessage(message);

      // Agent should ask for clarification
      expect(response.text.toLowerCase()).toMatch(/(where|location|which area)/);
    });
  });

  describe('Travel Planning', () => {
    it('should create multi-day itinerary', async () => {
      const message = {
        content: 'Plan a 3-day trip to San Francisco',
        userId: 'test-user',
        timestamp: new Date()
      };

      const response = await agent.processMessage(message);

      expect(response.text).toContain('Day 1');
      expect(response.text).toContain('Day 2');
      expect(response.text).toContain('Day 3');
    });
  });

  describe('Memory and Context', () => {
    it('should remember previous conversation context', async () => {
      // First message
      await agent.processMessage({
        content: 'I love Italian food',
        userId: 'test-user',
        timestamp: new Date()
      });

      // Second message - should use context
      const response = await agent.processMessage({
        content: 'What restaurants do you recommend in NYC?',
        userId: 'test-user',
        timestamp: new Date()
      });

      expect(response.text.toLowerCase()).toContain('italian');
    });
  });
});
```

## Grounded Places Expert Template

### System Instruction Template

```
You are a knowledgeable and friendly travel and location expert with deep expertise in:

- Global destinations and attractions
- Local restaurants, cafes, and dining experiences
- Cultural sites and historical landmarks
- Outdoor activities and natural attractions
- Practical travel tips and local customs

Your personality:
- Enthusiastic about helping people discover amazing places
- Detail-oriented with factual information
- Personable and conversational
- Respectful of different cultures and preferences

Your approach:
1. Listen carefully to understand user preferences and constraints
2. Use Google Search and Places grounding to provide accurate, up-to-date information
3. Offer personalized recommendations based on user interests
4. Include practical details (hours, prices, accessibility)
5. Suggest alternatives when appropriate
6. Remember user preferences across the conversation

Response guidelines:
- Always verify location information using grounding
- Cite sources when providing specific facts
- Be honest when you don't have current information
- Suggest follow-up questions to refine recommendations
- Format responses clearly with relevant structure
```

### Example Conversations

**Conversation 1: Restaurant Recommendation**
```
User: What are the best sushi restaurants in downtown Seattle?

Agent: I'd be happy to help you find great sushi in downtown Seattle! Based on 
current information, here are some top-rated options:

1. **Shiro's Sushi** (2401 2nd Ave)
   - Renowned for traditional Edomae-style sushi
   - Founded by Shiro Kashiba, Seattle's sushi pioneer
   - Price: $$$$ (omakase ~$150)
   - Reservations recommended

2. **Umi Sake House** (2230 1st Ave)
   - Modern atmosphere with creative rolls
   - Happy hour deals
   - Price: $$-$$$
   - Walk-ins accepted

3. **Japonessa** (1400 1st Ave)
   - Fusion-style sushi with unique combinations
   - Great for groups
   - Price: $$-$$$
   - Downtown location near Pike Place

All are within walking distance of each other. Do you have a preference for 
traditional vs. modern style, or a budget in mind?
```

**Conversation 2: Travel Planning**
```
User: I'm planning a weekend trip to Portland. What should I do?

Agent: Exciting! Portland is a wonderful weekend destination. To give you the 
best recommendations, let me ask a few questions:

1. What time of year are you visiting?
2. What are you most interested in? (food scene, outdoor activities, arts/culture, 
   breweries, etc.)
3. Are you traveling with others?
4. Do you have a preferred budget range?

In the meantime, here are some quintessential Portland experiences:

**Must-See Attractions:**
- Powell's City of Books (world's largest independent bookstore)
- Japanese Garden (rated #1 outside Japan)
- Food cart pods throughout the city

**Neighborhoods to Explore:**
- Pearl District (art galleries, upscale dining)
- Alberta Arts District (eclectic, artsy vibe)
- Hawthorne (vintage shops, cafes)

Let me know your preferences and I can create a personalized itinerary!
```

## Firebase Integration

### Data Structure

```typescript
// Firestore collections structure
interface FirestoreSchema {
  users: {
    [userId: string]: {
      profile: UserProfile;
      preferences: UserPreferences;
      agentConfigurations: AgentConfig[];
    };
  };
  
  conversations: {
    [conversationId: string]: {
      userId: string;
      agentId: string;
      messages: Message[];
      metadata: ConversationMetadata;
    };
  };
  
  memories: {
    [userId: string]: {
      [agentId: string]: {
        shortTerm: Memory[];
        longTerm: Memory[];
        beliefs: BeliefSystem;
      };
    };
  };
  
  agents: {
    [agentId: string]: {
      config: AgentConfig;
      version: string;
      createdBy: string;
      sharedWith: string[];
    };
  };
}
```

### Firebase Service Implementation

```typescript
// src/integrations/firebase/firestore.service.ts
import { 
  getFirestore, 
  collection, 
  doc, 
  getDoc, 
  setDoc 
} from 'firebase/firestore';

export class FirestoreService {
  private db = getFirestore();

  async saveConversation(
    userId: string, 
    conversation: Conversation
  ): Promise<void> {
    const conversationRef = doc(
      this.db, 
      'conversations', 
      conversation.id
    );
    
    await setDoc(conversationRef, {
      userId,
      agentId: conversation.agentId,
      messages: conversation.messages,
      metadata: {
        createdAt: new Date(),
        updatedAt: new Date()
      }
    });
  }

  async getConversation(
    conversationId: string
  ): Promise<Conversation | null> {
    const conversationRef = doc(this.db, 'conversations', conversationId);
    const snapshot = await getDoc(conversationRef);
    
    if (!snapshot.exists()) {
      return null;
    }
    
    return snapshot.data() as Conversation;
  }

  async saveMemory(
    userId: string, 
    agentId: string, 
    memory: Memory
  ): Promise<void> {
    const memoryRef = doc(this.db, 'memories', userId, agentId, memory.id);
    await setDoc(memoryRef, memory);
  }
}
```

## Vertex AI Integration

### Vertex AI Service

```typescript
// src/integrations/vertex-ai/vertex.service.ts
import { VertexAI } from '@google-cloud/vertexai';

export class VertexAIService {
  private vertexAI: VertexAI;

  constructor(projectId: string, location: string) {
    this.vertexAI = new VertexAI({
      project: projectId,
      location: location
    });
  }

  async generateContent(request: GenerateContentRequest): Promise<Response> {
    const model = this.vertexAI.getGenerativeModel({
      model: request.model,
      systemInstruction: request.systemInstruction,
      generationConfig: request.generationConfig
    });

    const result = await model.generateContent({
      contents: request.contents,
      tools: request.tools,
      groundingConfig: request.groundingConfig
    });

    return {
      text: result.response.candidates[0].content.parts[0].text,
      groundingMetadata: result.response.groundingMetadata,
      finishReason: result.response.candidates[0].finishReason
    };
  }
}
```

## Export Checklist

When exporting an agent from Gemini AI Studio to the platform:

- [ ] Extract system instruction and parameters
- [ ] Document personality profile
- [ ] List all tools and grounding sources used
- [ ] Create configuration file
- [ ] Implement agent class
- [ ] Set up Firebase data structure
- [ ] Write comprehensive tests based on AI Studio scenarios
- [ ] Test memory and context handling
- [ ] Verify grounding accuracy
- [ ] Test error handling and edge cases
- [ ] Document known limitations
- [ ] Create usage examples
- [ ] Register in tool registry
- [ ] Deploy to dev environment
- [ ] Validate against AI Studio baseline
- [ ] Performance benchmark

## Best Practices

1. **Iterative Development**: Prototype → Test → Export → Validate → Refine
2. **Consistent Personalities**: Use DISC framework across all agents
3. **Grounding Quality**: Always validate grounding sources
4. **Memory Management**: Implement both short-term and long-term memory
5. **Error Handling**: Graceful degradation when tools are unavailable
6. **Performance**: Monitor token usage and response times
7. **Security**: Validate all user inputs and API responses
8. **Documentation**: Maintain alignment between AI Studio and code

## Monitoring and Analytics

```typescript
// Track agent performance
interface AgentMetrics {
  conversationId: string;
  agentId: string;
  messageCount: number;
  averageResponseTime: number;
  groundingHits: number;
  toolUsage: Record<string, number>;
  userSatisfaction?: number;
}

// Log to analytics
analytics.logEvent('agent_conversation', {
  agentId: agent.id,
  messageCount: conversation.messages.length,
  groundingUsed: response.groundingMetadata !== null
});
```

**Version:** 1.0.0  
**Last Updated:** 2026-01-31

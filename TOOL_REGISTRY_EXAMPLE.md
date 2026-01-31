# Tool Registry Example

This file provides example tool registrations based on the Tool Registry Schema for the Gateway Global AI Platform.

```json
{
  "version": "1.0.0",
  "lastUpdated": "2026-01-31T04:00:00.000Z",
  "tools": [
    {
      "id": "gemini-search",
      "name": "Gemini Search",
      "description": "Advanced AI-powered search using Google's Gemini model with grounding capabilities",
      "category": "search",
      "version": "1.0.0",
      "status": "active",
      "provider": {
        "name": "gemini",
        "type": "ai-model",
        "apiVersion": "v1",
        "endpoint": "https://generativelanguage.googleapis.com"
      },
      "privileges": {
        "requiredRoles": ["user", "agent", "developer", "admin"],
        "scopes": ["https://www.googleapis.com/auth/generative-language"],
        "rateLimit": {
          "requests": 60,
          "period": "minute"
        },
        "costTier": "medium"
      },
      "configuration": {
        "environment": {
          "dev": {
            "enabled": true,
            "apiKey": "GEMINI_API_KEY_DEV",
            "features": {
              "grounding": true,
              "searchGrounding": true
            }
          },
          "stag": {
            "enabled": true,
            "apiKey": "GEMINI_API_KEY_STAG",
            "features": {
              "grounding": true,
              "searchGrounding": true
            }
          },
          "prod": {
            "enabled": true,
            "apiKey": "GEMINI_API_KEY_PROD",
            "features": {
              "grounding": true,
              "searchGrounding": true
            }
          }
        },
        "parameters": {
          "model": "gemini-pro",
          "temperature": 0.7,
          "maxOutputTokens": 2048
        },
        "timeout": 30000,
        "retryPolicy": {
          "maxRetries": 3,
          "backoffMs": 1000
        }
      },
      "metadata": {
        "documentation": "https://ai.google.dev/docs",
        "maintainer": "Gateway Global AI Platform Team",
        "tags": ["ai", "search", "gemini", "grounding"],
        "geminiStudioCompatible": true,
        "mcpCompliant": true
      },
      "createdAt": "2026-01-31T04:00:00.000Z",
      "updatedAt": "2026-01-31T04:00:00.000Z"
    },
    {
      "id": "google-places-grounding-lite",
      "name": "Google Places Grounding Lite",
      "description": "Lightweight location-based grounding using Google Places data for context enhancement",
      "category": "location",
      "version": "1.0.0",
      "status": "active",
      "provider": {
        "name": "google",
        "type": "external-api",
        "apiVersion": "v1",
        "endpoint": "https://places.googleapis.com"
      },
      "privileges": {
        "requiredRoles": ["agent", "developer", "admin"],
        "scopes": ["https://www.googleapis.com/auth/places"],
        "rateLimit": {
          "requests": 100,
          "period": "minute"
        },
        "costTier": "low"
      },
      "configuration": {
        "environment": {
          "dev": {
            "enabled": true,
            "apiKey": "GOOGLE_PLACES_API_KEY_DEV"
          },
          "stag": {
            "enabled": true,
            "apiKey": "GOOGLE_PLACES_API_KEY_STAG"
          },
          "prod": {
            "enabled": true,
            "apiKey": "GOOGLE_PLACES_API_KEY_PROD"
          }
        },
        "timeout": 10000,
        "retryPolicy": {
          "maxRetries": 2,
          "backoffMs": 500
        }
      },
      "metadata": {
        "documentation": "https://developers.google.com/maps/documentation/places",
        "maintainer": "Gateway Global AI Platform Team",
        "tags": ["location", "places", "grounding", "lite"],
        "geminiStudioCompatible": true,
        "mcpCompliant": true
      },
      "createdAt": "2026-01-31T04:00:00.000Z",
      "updatedAt": "2026-01-31T04:00:00.000Z"
    },
    {
      "id": "google-places-api",
      "name": "Google Places API",
      "description": "Full-featured Google Places API for comprehensive location search and details",
      "category": "location",
      "version": "1.0.0",
      "status": "active",
      "provider": {
        "name": "google",
        "type": "external-api",
        "apiVersion": "v1",
        "endpoint": "https://maps.googleapis.com/maps/api/place"
      },
      "privileges": {
        "requiredRoles": ["agent", "developer", "admin"],
        "scopes": ["https://www.googleapis.com/auth/places"],
        "rateLimit": {
          "requests": 100,
          "period": "minute"
        },
        "costTier": "medium"
      },
      "configuration": {
        "environment": {
          "dev": {
            "enabled": true,
            "apiKey": "GOOGLE_MAPS_API_KEY_DEV"
          },
          "stag": {
            "enabled": true,
            "apiKey": "GOOGLE_MAPS_API_KEY_STAG"
          },
          "prod": {
            "enabled": true,
            "apiKey": "GOOGLE_MAPS_API_KEY_PROD"
          }
        },
        "parameters": {
          "radius": 5000,
          "language": "en"
        },
        "timeout": 15000,
        "retryPolicy": {
          "maxRetries": 3,
          "backoffMs": 1000
        }
      },
      "metadata": {
        "documentation": "https://developers.google.com/maps/documentation/places/web-service",
        "maintainer": "Gateway Global AI Platform Team",
        "tags": ["location", "places", "search", "maps"],
        "geminiStudioCompatible": true,
        "mcpCompliant": true
      },
      "dependencies": ["google-places-grounding-lite"],
      "createdAt": "2026-01-31T04:00:00.000Z",
      "updatedAt": "2026-01-31T04:00:00.000Z"
    },
    {
      "id": "google-maps-javascript",
      "name": "Google Maps JavaScript",
      "description": "Google Maps JavaScript API integration for interactive mapping",
      "category": "mapping",
      "version": "1.0.0",
      "status": "active",
      "provider": {
        "name": "google",
        "type": "external-api",
        "apiVersion": "weekly",
        "endpoint": "https://maps.googleapis.com/maps/api/js"
      },
      "privileges": {
        "requiredRoles": ["user", "agent", "developer", "admin"],
        "scopes": [],
        "costTier": "low"
      },
      "configuration": {
        "environment": {
          "dev": {
            "enabled": true,
            "apiKey": "GOOGLE_MAPS_API_KEY_DEV",
            "features": {
              "places": true,
              "geometry": true,
              "marker": true
            }
          },
          "stag": {
            "enabled": true,
            "apiKey": "GOOGLE_MAPS_API_KEY_STAG",
            "features": {
              "places": true,
              "geometry": true,
              "marker": true
            }
          },
          "prod": {
            "enabled": true,
            "apiKey": "GOOGLE_MAPS_API_KEY_PROD",
            "features": {
              "places": true,
              "geometry": true,
              "marker": true
            }
          }
        },
        "parameters": {
          "libraries": ["places", "geometry", "marker"],
          "version": "weekly"
        }
      },
      "metadata": {
        "documentation": "https://developers.google.com/maps/documentation/javascript",
        "maintainer": "Gateway Global AI Platform Team",
        "tags": ["maps", "javascript", "ui", "interactive"],
        "geminiStudioCompatible": false,
        "mcpCompliant": false
      },
      "createdAt": "2026-01-31T04:00:00.000Z",
      "updatedAt": "2026-01-31T04:00:00.000Z"
    },
    {
      "id": "google-maps-ui-kit",
      "name": "Google Maps UI Kit",
      "description": "Pre-built UI components for Google Maps integration in React applications",
      "category": "mapping",
      "version": "1.0.0",
      "status": "active",
      "provider": {
        "name": "google",
        "type": "internal-service"
      },
      "privileges": {
        "requiredRoles": ["developer", "admin"],
        "scopes": [],
        "costTier": "free"
      },
      "configuration": {
        "environment": {
          "dev": {
            "enabled": true,
            "features": {
              "advancedMarkers": true,
              "infoWindows": true,
              "customControls": true
            }
          },
          "stag": {
            "enabled": true,
            "features": {
              "advancedMarkers": true,
              "infoWindows": true,
              "customControls": true
            }
          },
          "prod": {
            "enabled": true,
            "features": {
              "advancedMarkers": true,
              "infoWindows": true,
              "customControls": true
            }
          }
        }
      },
      "metadata": {
        "documentation": "https://developers.google.com/maps/documentation/javascript/web-components",
        "repository": "https://github.com/gateway-global-ai/gateway-platform",
        "maintainer": "Gateway Global AI Platform Team",
        "tags": ["maps", "ui", "react", "components"],
        "geminiStudioCompatible": false,
        "mcpCompliant": false
      },
      "dependencies": ["google-maps-javascript"],
      "createdAt": "2026-01-31T04:00:00.000Z",
      "updatedAt": "2026-01-31T04:00:00.000Z"
    },
    {
      "id": "google-workspace",
      "name": "Google Workspace",
      "description": "Comprehensive Google Workspace integration providing access to Gmail, Calendar, Drive, Docs, Sheets, and more through MCP server",
      "category": "integration",
      "version": "1.0.0",
      "status": "active",
      "provider": {
        "name": "google",
        "type": "mcp-server",
        "apiVersion": "v1",
        "endpoint": "https://www.googleapis.com/workspace"
      },
      "privileges": {
        "requiredRoles": ["user", "agent", "developer", "admin"],
        "scopes": [
          "https://www.googleapis.com/auth/gmail.readonly",
          "https://www.googleapis.com/auth/gmail.send",
          "https://www.googleapis.com/auth/calendar",
          "https://www.googleapis.com/auth/drive",
          "https://www.googleapis.com/auth/documents",
          "https://www.googleapis.com/auth/spreadsheets"
        ],
        "rateLimit": {
          "requests": 100,
          "period": "minute"
        },
        "costTier": "medium"
      },
      "configuration": {
        "environment": {
          "dev": {
            "enabled": true,
            "apiKey": "GOOGLE_WORKSPACE_API_KEY_DEV",
            "features": {
              "gmail": true,
              "calendar": true,
              "drive": true,
              "docs": true,
              "sheets": true,
              "tasks": true
            }
          },
          "stag": {
            "enabled": true,
            "apiKey": "GOOGLE_WORKSPACE_API_KEY_STAG",
            "features": {
              "gmail": true,
              "calendar": true,
              "drive": true,
              "docs": true,
              "sheets": true,
              "tasks": true
            }
          },
          "prod": {
            "enabled": true,
            "apiKey": "GOOGLE_WORKSPACE_API_KEY_PROD",
            "features": {
              "gmail": true,
              "calendar": true,
              "drive": true,
              "docs": true,
              "sheets": true,
              "tasks": true
            }
          }
        },
        "parameters": {
          "defaultCalendarId": "primary",
          "maxEmailResults": 50
        },
        "timeout": 30000,
        "retryPolicy": {
          "maxRetries": 3,
          "backoffMs": 1000
        }
      },
      "metadata": {
        "documentation": "https://developers.google.com/workspace",
        "repository": "https://github.com/gateway-global-ai/workspace",
        "maintainer": "Gateway Global AI Platform Team",
        "tags": ["workspace", "gmail", "calendar", "drive", "docs", "sheets", "productivity"],
        "geminiStudioCompatible": true,
        "mcpCompliant": true
      },
      "createdAt": "2026-01-31T04:00:00.000Z",
      "updatedAt": "2026-01-31T05:40:00.000Z"
    }
  ]
}
```

## Usage

This example demonstrates how to register the six required MCP Server base tools:

1. **Gemini Search** - AI-powered search with grounding
2. **Google Places Grounding Lite** - Lightweight location grounding
3. **Google Places API** - Full location search and details
4. **Google Maps JavaScript** - Interactive mapping
5. **Google Maps UI Kit** - React UI components
6. **Google Workspace** - Comprehensive productivity suite (Gmail, Calendar, Drive, Docs, Sheets, Tasks)

Each tool includes:
- Complete metadata and versioning
- Role-based access control
- Environment-specific configuration
- Rate limiting and cost tracking
- Gemini AI Studio compatibility flags
- MCP compliance indicators

## Platform Economics Principle

The inclusion of Google Workspace exemplifies the platform economics strategy of leveraging existing platforms and resources before building custom infrastructure. By integrating Google Workspace's MCP server, we gain access to:

- **Email Communication** (Gmail) - Essential for agent notifications and communications
- **Calendar Management** - Critical for scheduling and itinerary management
- **Document Management** (Docs, Sheets) - For creating and managing business documents
- **File Storage** (Drive) - Centralized file management
- **Task Management** - For tracking and organizing work

This approach allows agents to manage itineraries, schedule events, send communications, and handle business functions without requiring custom-built infrastructure for each capability.

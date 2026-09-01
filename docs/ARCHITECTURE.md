# Ask-NASA — System Architecture

**Status:** Draft for implementation  
**Document:** Technical Architecture  
**Project:** Ask-NASA  
**Last Updated:** 2026-08-31

---

# 1. Architecture Overview

Ask-NASA is designed as a conversational retrieval system that combines:

- A modern web interface
- A backend API
- NASA public data sources
- Retrieval and search capabilities
- An LLM for natural-language understanding and response generation
- Source attribution
- Conversation context

The system should prioritize **retrieval-grounded generation** rather than relying exclusively on the language model's internal knowledge.

High-level architecture:

```text
┌───────────────────────────────────────────────┐
│                  ASK-NASA UI                  │
│                                               │
│  Chat Interface                               │
│  Suggested Prompts                            │
│  Images / Cards                               │
│  Sources                                      │
└───────────────────────┬───────────────────────┘
                        │
                        │ HTTP / API
                        ▼
┌───────────────────────────────────────────────┐
│                BACKEND API                    │
│                                               │
│  Request Validation                           │
│  Conversation Management                      │
│  Query Processing                             │
│  Retrieval Orchestration                      │
│  Response Formatting                          │
└───────────────────────┬───────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────┐
│             INTELLIGENCE LAYER                │
│                                               │
│  Intent Detection                             │
│  Query Classification                         │
│  Retrieval Planning                            │
│  Context Assembly                             │
│  LLM Generation                               │
│  Source Attribution                            │
└───────────────────────┬───────────────────────┘
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
┌──────────────────────┐ ┌──────────────────────┐
│   NASA DATA LAYER    │ │   CONVERSATION DATA  │
│                      │ │                      │
│ NASA APIs            │ │ Session Context      │
│ NASA Resources       │ │ Message History      │
│ NASA Archives        │ │ Query Context        │
└──────────────────────┘ └──────────────────────┘
```

---

# 2. Architectural Principles

The following principles are mandatory.

## 2.1 Retrieval Before Generation

The LLM should not be expected to know every NASA-specific fact.

For information requiring factual grounding:

```text
Question
→ Retrieve relevant information
→ Assemble evidence
→ Generate answer
```

rather than:

```text
Question
→ Ask LLM from memory
```

---

## 2.2 Source Grounding

Responses should retain the relationship between claims and their underlying sources whenever practical.

The system should preserve:

```text
Source
    ↓
Retrieved Content
    ↓
Relevant Evidence
    ↓
Generated Answer
```

---

## 2.3 Separation of Concerns

The frontend should not directly communicate with NASA APIs or the LLM provider.

Instead:

```text
Frontend
    ↓
Backend
    ↓
Services
    ↓
External APIs / LLM
```

This protects credentials and keeps the application maintainable.

---

## 2.4 Provider Independence

LLM-specific code should be isolated behind a service interface.

The architecture should allow the LLM provider to be changed without rewriting the application.

Conceptually:

```text
LLMService
├── Provider A
├── Provider B
└── Provider C
```

The initial implementation may use one provider, but the rest of the application should not depend directly on provider-specific logic.

---

## 2.5 NASA Source Priority

When answering NASA-specific questions, authoritative NASA sources should receive the highest priority.

Preferred hierarchy:

```text
NASA official APIs / datasets
        ↓
NASA official websites
        ↓
NASA mission/project resources
        ↓
Other authoritative scientific sources
        ↓
General web sources
```

The exact source strategy will be defined in `DATA_SOURCES.md`.

---

# 3. System Components

The application consists of the following logical components.

## 3.1 Frontend

Responsible for:

- Chat interface
- Message rendering
- Input handling
- Suggested prompts
- Loading states
- Error states
- Image presentation
- Source presentation
- Responsive layout

The frontend should remain primarily responsible for presentation and user interaction.

---

## 3.2 Backend API

Responsible for:

- API endpoints
- Request validation
- Session handling
- Conversation state
- Query orchestration
- Retrieval coordination
- LLM invocation
- Response formatting
- Error handling

The backend is the central application layer.

---

## 3.3 Query Processing Service

Transforms raw user questions into structured retrieval tasks.

Example:

```text
User:
"What missions have visited Jupiter?"
```

Possible internal representation:

```text
Intent:
mission_discovery

Entity:
Jupiter

Requested information:
missions

Temporal requirement:
none

Response type:
list
```

The exact schema may evolve during implementation.

---

## 3.4 Retrieval Service

Responsible for obtaining relevant information from NASA sources.

Responsibilities include:

- Query formulation
- API requests
- Search
- Filtering
- Ranking
- Deduplication
- Metadata extraction
- Source preservation

The retrieval layer should return structured evidence rather than raw unprocessed responses wherever possible.

---

## 3.5 Context Assembly

The system combines:

```text
Current User Query
+
Relevant Conversation History
+
Retrieved Evidence
+
System Instructions
```

into the context supplied to the generation layer.

The context window should be managed carefully to avoid unnecessary token usage.

---

## 3.6 LLM Service

Responsible for:

- Understanding natural language
- Interpreting retrieved evidence
- Generating explanations
- Maintaining conversational tone
- Producing structured output when required

The LLM must be instructed to:

- Use supplied evidence for factual claims
- Avoid fabricating information
- Express uncertainty
- Preserve source relationships
- Follow the product's response style

---

## 3.7 Source Attribution Service

The system should preserve metadata for retrieved sources.

A source object should conceptually contain:

```text
{
    title,
    url,
    source_type,
    publisher,
    retrieved_at,
    relevance
}
```

The exact implementation may differ.

Sources should be attached to the final response where relevant.

---

# 4. Request Lifecycle

A normal user request should follow this pipeline:

```text
1. User submits question
            ↓
2. Frontend sends request
            ↓
3. Backend validates request
            ↓
4. Conversation context loaded
            ↓
5. Query classified
            ↓
6. Retrieval strategy selected
            ↓
7. NASA information retrieved
            ↓
8. Evidence filtered/ranked
            ↓
9. Context assembled
            ↓
10. LLM generates response
            ↓
11. Response validated/formatted
            ↓
12. Sources attached
            ↓
13. Frontend renders answer
```

---

# 5. Query Classification

Not every query should follow exactly the same retrieval path.

The system should classify requests into broad categories.

Example categories:

```text
MISSION
IMAGE
ASTRONOMY
SCIENCE
NASA_NEWS
DATASET
GENERAL_NASA
FOLLOW_UP
OUT_OF_SCOPE
```

The classification layer should remain lightweight.

It should not introduce unnecessary complexity into the MVP.

---

# 6. Retrieval Strategy

The initial retrieval architecture should support multiple retrieval modes.

## 6.1 Direct API Retrieval

Used when a NASA API provides the required information directly.

Example:

```text
"What is today's NASA image?"
```

→ Query the relevant NASA API.

---

## 6.2 Search-Based Retrieval

Used when the required information is not available through a dedicated endpoint.

```text
User Question
    ↓
Search NASA resources
    ↓
Retrieve relevant documents
    ↓
Rank results
```

---

## 6.3 Hybrid Retrieval

Some queries may require multiple sources.

Example:

> "What has Perseverance discovered recently and show me related images?"

Potential pipeline:

```text
Mission information
        +
Recent NASA information
        +
NASA imagery
        ↓
Combined evidence
        ↓
Answer
```

---

# 7. Retrieval-Augmented Generation

Ask-NASA should use a lightweight RAG architecture where appropriate.

Conceptually:

```text
             USER QUERY
                  │
                  ▼
          Query Understanding
                  │
                  ▼
         Retrieval Orchestrator
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
     NASA API   NASA Search  Dataset
        │         │         │
        └─────────┼─────────┘
                  ▼
            Evidence Set
                  │
                  ▼
          Context Assembly
                  │
                  ▼
                 LLM
                  │
                  ▼
          Grounded Response
                  │
                  ▼
              Sources
```

The MVP should avoid unnecessarily complicated RAG infrastructure.

A vector database should **not** be introduced merely because the project uses the term "RAG."

It should only be added if the actual data/retrieval requirements justify it.

---

# 8. Conversation Architecture

Conversation state should be represented separately from retrieved information.

Conceptually:

```text
Conversation
├── session_id
├── messages[]
├── current_context
└── metadata
```

A message may contain:

```text
role
content
timestamp
sources
metadata
```

The system should not send the entire conversation to the LLM indefinitely.

Older messages should be summarized, truncated, or otherwise managed when necessary.

---

# 9. Context Management

The system should optimize context usage.

Priority order:

```text
Current question
        ↓
Relevant recent conversation
        ↓
Retrieved evidence
        ↓
Older conversation context
```

Irrelevant historical messages should not consume model context.

This is especially important for controlling inference cost.

---

# 10. Response Contract

The backend should return structured responses rather than only plain text.

Conceptual response:

```text
{
    "answer": "...",
    "sources": [
        {
            "title": "...",
            "url": "...",
            "source_type": "NASA"
        }
    ],
    "images": [],
    "metadata": {}
}
```

The exact schema will be defined during implementation.

The frontend should consume this structure rather than parsing arbitrary generated text.

---

# 11. Streaming

The architecture should support streamed assistant responses if supported by the selected LLM provider.

Potential flow:

```text
Backend
   ↓
LLM stream
   ↓
Frontend
   ↓
Progressively rendered response
```

Streaming should improve perceived responsiveness.

However, it should not complicate the MVP unnecessarily.

If streaming introduces excessive complexity during the first implementation, it may initially be implemented as a normal request/response flow and added afterward.

---

# 12. Error Handling

Errors should be handled explicitly at each layer.

Possible failures:

### NASA API Failure

```text
NASA API unavailable
```

The system should return a graceful fallback or explain that the requested information could not currently be retrieved.

### LLM Failure

The user should receive a clear error rather than an empty interface.

### Retrieval Failure

The system should not fabricate an answer to compensate for missing evidence.

### Timeout

Long-running requests should terminate gracefully.

### Invalid Request

The backend should reject malformed requests before processing.

---

# 13. Security Architecture

API credentials must never be exposed to the frontend.

Sensitive configuration belongs in environment variables.

Conceptually:

```text
Frontend
   ✗ NASA API key
   ✗ LLM API key

Backend
   ✓ NASA credentials
   ✓ LLM credentials
```

The application should also:

- Validate user input
- Limit request sizes
- Handle API errors safely
- Avoid exposing internal stack traces
- Protect backend secrets
- Apply reasonable rate limiting
- Avoid logging sensitive credentials

---

# 14. Configuration

Environment-specific values should not be hardcoded.

Configuration should be represented through environment variables.

Potential configuration:

```text
LLM_API_KEY
LLM_MODEL
NASA_API_KEY
BACKEND_URL
FRONTEND_URL
DATABASE_URL
```

Only variables actually required by the implementation should be introduced.

---

# 15. Caching

Caching should be considered for expensive or frequently repeated NASA requests.

Potential candidates:

- Astronomy Picture of the Day
- Frequently accessed mission information
- Static NASA metadata
- Search results with appropriate freshness rules

Caching should not cause stale information to be presented as current.

Time-sensitive information requires appropriate cache expiration.

---

# 16. Observability

The application should provide enough logging to diagnose failures.

Useful events include:

```text
request_received
query_classified
retrieval_started
retrieval_completed
llm_started
llm_completed
response_generated
request_failed
```

Logs should not expose API keys or sensitive user information.

Detailed observability infrastructure is not required for the first MVP unless needed.

---

# 17. Performance Goals

The system should prioritize perceived responsiveness.

Target behavior:

- Fast initial UI response
- Immediate loading indication
- Efficient NASA requests
- Minimal unnecessary LLM calls
- Streaming where practical
- Caching for suitable resources

Performance should be measured after the basic system is functional.

Premature optimization should be avoided.

---

# 18. Frontend Architecture

The frontend should use component-based design.

Conceptual structure:

```text
App
├── Landing
│   ├── Hero
│   ├── SearchInput
│   └── SuggestedPrompts
│
└── Chat
    ├── MessageList
    │   ├── UserMessage
    │   └── AssistantMessage
    │       ├── Text
    │       ├── ImageCard
    │       └── SourceList
    │
    └── ChatInput
```

Components should remain modular.

The UI should not contain backend business logic.

---

# 19. Backend Architecture

The backend should separate concerns logically.

Conceptual structure:

```text
backend/
├── api/
├── services/
│   ├── query/
│   ├── retrieval/
│   ├── llm/
│   └── sources/
├── models/
├── schemas/
├── config/
└── utils/
```

The exact framework and directory structure should be finalized during implementation.

---

# 20. Data Flow

The complete data flow should resemble:

```text
                    USER
                     │
                     ▼
               WEB FRONTEND
                     │
                     ▼
                BACKEND API
                     │
                     ▼
              QUERY PROCESSOR
                     │
                     ▼
           RETRIEVAL ORCHESTRATOR
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
      NASA APIs   NASA Search  Other
          │          │       Sources
          └──────────┼──────────┘
                     ▼
                EVIDENCE
                     │
                     ▼
             CONTEXT ASSEMBLER
                     │
                     ▼
                   LLM
                     │
                     ▼
            RESPONSE VALIDATOR
                     │
             ┌───────┴───────┐
             ▼               ▼
           Answer          Sources
             │               │
             └───────┬───────┘
                     ▼
                 FRONTEND
```

---

# 21. Data Persistence

The MVP should minimize persistent state unless necessary.

Possible persistence requirements:

- Conversation history
- Cached resources
- Application configuration

User accounts are not required for the initial MVP.

If persistence is required, the simplest suitable storage solution should be selected.

Do not introduce a database merely for architectural appearance.

---

# 22. API Design

The backend should expose a small, clear API.

Potential MVP endpoints:

```text
POST /api/chat
```

Primary conversational endpoint.

```text
GET /api/health
```

Health check.

Additional endpoints should only be introduced when required.

Potential future endpoints:

```text
GET /api/missions
GET /api/images
GET /api/search
```

However, the initial architecture should avoid unnecessary endpoint proliferation.

---

# 23. LLM Prompt Architecture

LLM instructions should be separated from application code where practical.

Prompt responsibilities include:

### System Instructions

Define:

- Ask-NASA identity
- Response behavior
- Scientific reliability
- Grounding requirements
- Citation behavior
- Tone

### Retrieved Context

Contains evidence relevant to the current query.

### Conversation Context

Contains relevant previous messages.

### Current Query

The user's latest request.

Conceptually:

```text
SYSTEM INSTRUCTIONS
        +
CONVERSATION CONTEXT
        +
RETRIEVED EVIDENCE
        +
USER QUERY
        ↓
       LLM
        ↓
STRUCTURED RESPONSE
```

Prompts should not contain hardcoded temporary data.

---

# 24. Grounding Rules

The generation layer should follow these rules:

### Rule 1

Do not invent NASA facts.

### Rule 2

Do not invent sources.

### Rule 3

Do not claim that retrieved information says something it does not say.

### Rule 4

If evidence is insufficient, state the limitation.

### Rule 5

Distinguish between factual information and explanatory interpretation.

### Rule 6

Prefer the most authoritative available source.

---

# 25. Extensibility

The architecture should allow future capabilities without major restructuring.

Potential future additions:

```text
Voice Interface
       │
Mobile Client
       │
Personalized Accounts
       │
Advanced NASA Dataset Search
       │
Scientific Literature Retrieval
       │
Multimodal Questions
       │
Image Understanding
       │
Mission Timeline Visualization
       │
Advanced Research Mode
```

These are future capabilities, not MVP requirements.

---

# 26. MVP Technology Selection Principle

Technology should be selected based on:

1. Simplicity
2. Reliability
3. Maintainability
4. Development speed
5. AI integration quality
6. Deployment practicality
7. Cost

Avoid introducing technologies solely because they are popular.

The final technology stack should be documented and justified before implementation.

---

# 27. Development Strategy

Implementation should proceed incrementally.

Recommended order:

```text
1. Project skeleton
        ↓
2. Frontend shell
        ↓
3. Backend health endpoint
        ↓
4. Basic chat API
        ↓
5. LLM integration
        ↓
6. NASA API integration
        ↓
7. Retrieval orchestration
        ↓
8. Source attribution
        ↓
9. Conversation context
        ↓
10. Image support
        ↓
11. Error handling
        ↓
12. UI refinement
        ↓
13. Testing
        ↓
14. Deployment
```

Each stage should produce a working increment.

---

# 28. Testing Strategy

Testing should exist at multiple levels.

## Unit Tests

Test:

- Query classification
- Data transformation
- Source parsing
- Response formatting
- Validation

## Integration Tests

Test:

- Backend → NASA API
- Backend → LLM
- Frontend → Backend
- Retrieval → generation pipeline

## End-to-End Tests

Test complete user journeys:

```text
User Question
→ Retrieval
→ Generation
→ Sources
→ UI
```

---

# 29. Architecture Constraints

The following constraints should be respected during implementation:

- Do not expose API keys to the frontend.
- Do not make the frontend responsible for NASA retrieval.
- Do not rely entirely on LLM memory for NASA-specific facts.
- Do not introduce a vector database without a demonstrated requirement.
- Do not introduce unnecessary microservices.
- Do not create excessive API endpoints.
- Do not duplicate NASA data without a clear reason.
- Do not couple the entire application to one LLM provider.
- Do not sacrifice reliability for visual complexity.

---

# 30. Architecture Decision Record

Before introducing major infrastructure, the reason should be documented.

Examples:

```text
Why this LLM?
Why this backend framework?
Why this database?
Why vector search?
Why this deployment platform?
```

Architecture decisions should be based on actual project requirements.

---

# 31. Definition of Done — Architecture

The architecture phase is considered complete when:

- Frontend/backend boundaries are defined.
- NASA retrieval flow is defined.
- LLM integration boundary is defined.
- Conversation handling is defined.
- Source attribution is defined.
- Error handling strategy is defined.
- Security boundaries are defined.
- MVP API surface is defined.
- Technology choices are documented.
- The implementation can proceed without major architectural ambiguity.

---

# 32. Relationship to Other Documents

`PRODUCT.md` defines **what Ask-NASA should accomplish**.

`ARCHITECTURE.md` defines **how the system should accomplish it**.

`DATA_SOURCES.md` defines **where the information comes from**.

`ROADMAP.md` defines **when and in what order capabilities are implemented**.

All four documents should remain consistent.

---

# 33. Architecture North Star

> **Keep the system simple enough to build, structured enough to scale, and grounded enough to trust.**
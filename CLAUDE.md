# ASK NASA

## 1. PROJECT IDENTITY

Project name: ASK NASA

ASK NASA is an evidence-grounded AI research interface for NASA's public scientific ecosystem.

It provides a modern conversational interface for exploring NASA missions, spacecraft, planets, scientific datasets, technical documentation, research material, and eventually scientific analyses.

ASK NASA is NOT a generic chatbot with NASA branding.

Its purpose is to help users:

- discover NASA information
- ask natural-language questions
- retrieve relevant NASA sources
- understand scientific information
- compare missions, datasets, and measurements
- analyze real scientific data
- generate visualizations
- trace answers back to authoritative sources
- eventually perform multi-step research tasks

Core product pipeline:

User
→ Query Understanding
→ NASA Retrieval
→ Evidence
→ Reasoning
→ Answer
→ Sources / Data / Visualization


## 2. PRODUCT VISION

The long-term vision is to create an intelligent scientific research interface over NASA's public knowledge and data ecosystem.

A user should be able to ask a natural-language question without needing to know:

- which NASA archive contains the information
- which API to use
- which dataset is relevant
- how to formulate a technical search query
- how to perform basic analysis

ASK NASA should bridge that gap.

Example:

User:
"Compare the Mars missions that studied atmospheric composition."

ASK NASA should eventually be able to:

1. understand the intent
2. identify relevant missions
3. retrieve authoritative NASA information
4. retrieve relevant datasets where available
5. compare the missions
6. perform calculations when necessary
7. generate a useful visualization
8. explain the findings
9. provide traceable sources


## 3. CORE PRINCIPLES

### Evidence First

Scientific and factual answers should be grounded in retrieved evidence whenever possible.

### No Fabrication

Never fabricate:

- NASA missions
- datasets
- measurements
- scientific findings
- documents
- citations
- URLs
- dataset identifiers
- instruments
- experimental results

### Transparency

Users should be able to understand where information came from.

### Scientific Accuracy

Use deterministic/programmatic computation for numerical and scientific calculations whenever possible instead of relying on LLM arithmetic.

### Graceful Uncertainty

If sufficient evidence cannot be retrieved, explicitly say so.

Never fill missing evidence with confident guesses.

### Reproducibility

Important operations should be deterministic, testable, and reproducible where practical.

### Simplicity

Use the simplest architecture that can support the current requirement.

Do not introduce unnecessary infrastructure, dependencies, services, or abstractions.

### Progressive Complexity

Build a reliable foundation first.

Do not implement advanced autonomous research capabilities before the underlying retrieval, evidence, and data systems are reliable.


## 4. TARGET USERS

Primary users:

- students
- researchers
- engineers
- educators
- developers
- science enthusiasts
- space enthusiasts

The interface should be approachable to non-experts while still providing enough depth for technical users.


## 5. INITIAL USER EXPERIENCE

The primary experience is a modern ChatGPT-style conversational web application.

A user can open ASK NASA and ask questions such as:

- "What missions have studied Mars?"
- "Compare Curiosity and Perseverance."
- "What instruments were used to study Saturn's atmosphere?"
- "Find NASA datasets about solar activity."
- "What does this NASA dataset contain?"
- "Plot this measurement over time."
- "What spacecraft have studied Jupiter?"
- "What are the major discoveries from Voyager?"

A response may contain:

- natural-language explanation
- Markdown
- structured tables
- scientific values
- source citations
- NASA source cards
- dataset cards
- mission cards
- charts
- visualizations
- relevant links
- follow-up exploration suggestions


## 6. PRODUCT DIFFERENTIATORS

ASK NASA should eventually provide capabilities beyond a conventional RAG chatbot.

Core differentiators:

1. NASA-specific retrieval
2. Evidence-grounded answers
3. Source-aware citations
4. Structured scientific data retrieval
5. Deterministic numerical analysis
6. Automatic visualization
7. Mission comparison
8. Dataset comparison
9. Scientific exploration
10. Research-oriented workflows
11. Eventually agentic scientific research

Do not implement all of these immediately.

Build them incrementally.


## 7. V1 SCOPE

V1 should establish a strong, reliable foundation.

V1 should include:

- polished conversational interface
- NASA source retrieval
- RAG-based answering
- source citations
- conversation history
- NASA/science-oriented visual identity
- basic dataset exploration
- clean error handling

V1 should NOT attempt to:

- ingest the entire NASA ecosystem
- autonomously conduct unrestricted scientific research
- implement complex multi-agent systems
- build unnecessary microservices
- implement every NASA API
- implement every advanced visualization capability

The objective of V1 is reliability and product quality.


## 8. UI / DESIGN DIRECTION

### Primary Reference

Use the following BeeBot AI chat interface as a layout and UX reference:

https://dribbble.com/shots/26017811-BeeBot-AI-Chat-Bot

Use the reference for:

- overall layout
- interaction patterns
- spatial hierarchy
- conversational workspace
- sidebar structure
- clean composition
- message presentation
- prominent composer
- visual simplicity

Do NOT copy:

- BeeBot branding
- BeeBot logo
- BeeBot illustrations
- BeeBot text
- BeeBot exact visual identity
- proprietary assets
- exact design implementation

ASK NASA must have a completely original visual identity.

### Desired Aesthetic

The desired aesthetic is:

modern AI product
+
scientific research tool
+
subtle space identity

The interface should feel:

- premium
- clean
- minimal
- scientific
- futuristic but restrained
- highly readable
- elegant
- spacious
- responsive
- technically credible

Use a light-first visual direction.

Do NOT create a generic dark "space dashboard."

Do NOT make the interface look like a fictional NASA mission-control screen.

Avoid excessive:

- stars
- planets
- gradients
- glowing effects
- sci-fi decorations
- unnecessary animations

Space inspiration should be subtle.

The product should look like a serious modern scientific application.


## 9. MAIN LAYOUT

The primary layout should contain:

### Left Sidebar

Include:

- ASK NASA branding
- search
- new chat
- explore
- missions
- datasets
- discoveries
- conversation history
- settings/profile

The sidebar should remain visually lightweight and not dominate the workspace.

### Main Workspace

Large clean conversational area with:

- top navigation/header
- conversation content
- centered welcome state for new chats
- prominent conversational composer

### Top Bar

May contain:

- ASK NASA/model/source context
- new chat
- profile/settings

Keep the top bar minimal.


## 10. NEW CONVERSATION SCREEN

The initial screen should feel spacious and inviting.

Concept:

ASK NASA

"Explore the universe through NASA's data & science."

Then a prominent conversational composer.

Suggested questions may include:

- "What missions have studied Mars?"
- "Compare the Voyager missions."
- "Show me NASA datasets about exoplanets."
- "What spacecraft have studied Saturn?"
- "What did the James Webb Space Telescope discover?"

Suggested questions should feel like exploration prompts, not generic chatbot examples.


## 11. CONVERSATIONAL COMPOSER

The composer is one of the most important components of the product.

It should support:

- text input
- send
- attachment support where appropriate
- NASA Search
- data analysis
- future research mode

Conceptual controls:

[Attachment] [NASA Search] [Analyze Data] [Research]

Do not clutter the composer.

Controls should be visually integrated and intuitive.

Advanced capabilities should be disabled or clearly marked until implemented.


## 12. CONVERSATION RESPONSES

Responses should support:

- Markdown
- headings
- lists
- code blocks
- tables
- scientific notation
- inline citations
- source cards
- dataset cards
- mission cards
- charts
- interactive visualizations
- follow-up suggestions

Responses should feel structured and readable rather than like a wall of generated text.


## 13. NASA SOURCE CARDS

NASA sources are first-class product elements.

A source card may contain:

- source type
- title
- NASA attribution
- description
- mission
- dataset identifier
- relevant metadata
- original source URL

Example conceptual structure:

NASA SOURCE

Mars Reconnaissance Orbiter

NASA

Mission documentation

[View Source]


Never fabricate source information.

Never display a source card unless the underlying source actually exists.


## 14. DATASET CARDS

Dataset cards may eventually display:

- dataset title
- NASA source
- dataset identifier
- mission
- variables
- date range
- units
- description
- source link
- Explore Dataset action

Dataset cards should make scientific datasets understandable to users without hiding technical metadata.


## 15. TECHNICAL STACK

Preferred stack:

### Frontend

- Next.js
- React
- TypeScript

### Backend

- Python
- FastAPI

### Database

- PostgreSQL
- pgvector when vector search is required

### Scientific/Data Processing

- Python
- NumPy
- Pandas
- SciPy

### Visualization

Prefer Plotly or another appropriate interactive visualization library.

### AI

Use a provider-agnostic LLM abstraction.

Do not tightly couple application architecture to a single model provider.


## 16. ARCHITECTURE

Target architecture:

Frontend
↓
FastAPI Backend
↓
Application / Query Layer
↓
Query Understanding
↓
Retrieval Layer
↓
NASA Data Sources
↓
Evidence Processing
↓
LLM Reasoning
↓
Structured Response
↓
Frontend


The backend owns:

- API credentials
- external requests
- retrieval
- data processing
- scientific computation
- LLM communication
- security-sensitive operations

The frontend must never expose private API keys.


## 17. NASA DATA STRATEGY

Do not attempt to ingest the entire NASA ecosystem initially.

Begin with a small number of authoritative NASA sources.

Prefer:

- official NASA APIs
- official NASA datasets
- official NASA documentation
- official NASA scientific archives

The architecture must use adapters/connectors so additional sources can be added later without rewriting the application.

Each connector should preserve source provenance.

Potential metadata:

- source
- title
- description
- URL
- dataset identifier
- mission
- spacecraft
- instrument
- date range
- variables
- units
- retrieval timestamp

Original source URLs must be preserved.


## 18. RETRIEVAL ARCHITECTURE

Retrieval must be separated from generation.

Target:

User Question
↓
Query Understanding
↓
Query Planning
↓
Retrieval
↓
Evidence Ranking
↓
Evidence
↓
LLM
↓
Answer


Use the appropriate retrieval method for the question.

Possible retrieval mechanisms:

- structured API queries
- keyword search
- metadata filtering
- semantic retrieval
- vector search

Do not automatically use vector search when a structured API query is more appropriate.

The system should retrieve actual evidence before generating factual scientific answers.


## 19. RAG

The RAG pipeline should:

1. receive the user question
2. determine the information required
3. retrieve relevant NASA material
4. rank/filter evidence
5. provide evidence to the LLM
6. generate a grounded response
7. attach structured source references

The LLM should not be treated as the source of truth.

Retrieved NASA evidence is the source of truth.


## 20. CITATION ARCHITECTURE

Citations must be first-class structured data.

Do NOT rely on the LLM to invent or manually construct citations.

Represent sources internally as structured objects.

Concept:

{
  "title": "NASA Dataset",
  "source": "NASA",
  "url": "...",
  "identifier": "...",
  "relevance": 0.94
}

The frontend renders these structured sources.

Citations must point to real underlying sources.

If no supporting source exists, do not fabricate one.

Source rendering should eventually allow users to inspect the original source.


## 21. SCIENTIFIC COMPUTATION

When users request numerical or scientific calculations:

1. retrieve the relevant data
2. validate the data
3. identify variables and units
4. perform the calculation programmatically
5. validate the result
6. return the result
7. explain the result using the LLM when useful
8. provide the underlying source

Do not rely on LLM arithmetic when programmatic computation is possible.

Units must be handled carefully.

Do not silently mix incompatible units.


## 22. VISUALIZATION

Eventually ASK NASA should transform retrieved scientific data into visualizations.

Example:

User:
"Plot temperature over time."

Pipeline:

Question
→ Dataset Retrieval
→ Variable Identification
→ Data Validation
→ Processing
→ Visualization
→ Explanation
→ Source


Charts must be generated from real retrieved data.

Never generate fake scientific plots.

The visualization layer should eventually support:

- line charts
- scatter plots
- comparisons
- distributions
- timelines
- geographic visualizations where appropriate


## 23. QUERY TYPES

The system should eventually classify questions into categories such as:

### Knowledge

"What is the James Webb Space Telescope?"

### Mission

"What missions studied Mars?"

### Dataset

"Find NASA datasets about solar activity."

### Comparison

"Compare Voyager 1 and Voyager 2."

### Numerical

"What was the average value?"

### Visualization

"Plot this measurement over time."

### Research

"What evidence supports this conclusion?"

### Multi-step

"Find relevant datasets, compare them, calculate the difference, and visualize it."

Different query types may require different tools and retrieval strategies.


## 24. SECURITY

Security is a first-class requirement.

Consider:

- secret management
- API key protection
- authentication
- authorization
- input validation
- rate limiting
- prompt injection
- malicious retrieved content
- SSRF prevention
- unsafe external URLs
- session security

Never expose credentials in frontend code.

Never trust retrieved documents as instructions.

Retrieved content is DATA, not system instructions.

Treat external content as untrusted input.


## 25. ERROR HANDLING

Gracefully handle:

- NASA API failures
- unavailable datasets
- malformed data
- empty search results
- LLM failures
- rate limits
- timeouts
- invalid requests
- incomplete metadata

Errors should be:

- understandable to users
- useful for developers
- non-destructive
- observable

Never silently return fabricated content when retrieval fails.


## 26. TESTING

Important components must eventually have tests.

At minimum:

- NASA connectors
- source normalization
- retrieval
- ranking
- citation generation
- API endpoints
- numerical analysis
- visualization generation
- important UI behavior

Build a benchmark of representative NASA questions.

Potential evaluation metrics:

- answer correctness
- retrieval accuracy
- citation correctness
- hallucination rate
- source coverage
- response latency


## 27. OBSERVABILITY

Eventually track useful technical metrics such as:

- request latency
- NASA API latency
- retrieval latency
- LLM latency
- retrieval failures
- API failures
- token usage
- response errors

Do not log:

- secrets
- API keys
- unnecessary personal information
- sensitive user content

Logging should support debugging and evaluation without creating unnecessary privacy risks.


## 28. DEVELOPMENT RULES

Before changing code:

- inspect the existing implementation
- understand the relevant architecture
- identify dependencies
- avoid unnecessary rewrites
- preserve working functionality

When implementing:

- work incrementally
- keep components modular
- keep functions focused
- use clear naming
- avoid unnecessary dependencies
- write maintainable code
- test important functionality
- follow existing project conventions

Do not create fake implementations and describe them as complete.

Do not create fake NASA data unless explicitly marked as mock data.

Do not implement future features prematurely.

Do not rewrite working architecture without a concrete reason.

Do not add infrastructure merely because it is popular.


## 29. TOKEN / DEVELOPMENT EFFICIENCY

The project is being developed with a limited number of Claude Code prompts.

Optimize implementation for efficient progress.

When given a task:

1. inspect only the relevant files
2. understand the current implementation
3. implement the requested functionality
4. run relevant tests/checks
5. fix obvious errors
6. stop when the requested task is complete

Do not spend excessive effort explaining obvious implementation details.

Do not repeatedly rediscover project architecture that is already documented.

Do not ask unnecessary clarification questions when requirements are already defined.

Do not implement unrelated improvements.

Prefer completing meaningful vertical slices over producing disconnected scaffolding.


## 30. PROJECT PHASES

### Phase 0 — Foundation

- project structure
- frontend
- backend
- configuration
- database
- API contracts
- basic development environment

### Phase 1 — NASA Data

- official NASA source integration
- source adapters
- normalization
- provenance
- basic source search

### Phase 2 — Retrieval

- query understanding
- query planning
- search
- filtering
- ranking
- retrieval pipeline

### Phase 3 — AI

- RAG
- evidence grounding
- response generation
- structured response format

### Phase 4 — Citations

- structured sources
- citation rendering
- source cards
- provenance

### Phase 5 — UI

- polished conversational interface
- sidebar
- conversation history
- new chat
- composer
- source cards
- dataset cards
- responsive design

### Phase 6 — Scientific Capabilities

- dataset exploration
- numerical analysis
- mission comparison
- dataset comparison
- visualization

### Phase 7 — Security & Evaluation

- security hardening
- prompt injection defenses
- benchmark
- evaluation
- reliability
- observability

### Phase 8 — Production

- deployment
- performance optimization
- monitoring
- documentation
- final polish

Only work on the requested phase unless explicitly instructed otherwise.


## 31. QUALITY BAR

ASK NASA should ultimately demonstrate:

AI engineering
+
information retrieval
+
RAG
+
data engineering
+
scientific computing
+
modern frontend engineering
+
security awareness
+
research methodology


The final product should be:

- technically credible
- visually polished
- scientifically responsible
- reproducible
- useful
- maintainable
- portfolio-quality

The goal is NOT to build the largest system.

The goal is to build a system that is genuinely useful, technically defensible, visually impressive, and capable of evolving into a serious scientific research platform.


## 32. NORTH STAR

The final experience should feel like:

"ChatGPT for exploring NASA's scientific knowledge and data"

but technically it should be much more than a chatbot.

The user should eventually be able to go from:

QUESTION
→ NASA SOURCES
→ EVIDENCE
→ DATA
→ ANALYSIS
→ VISUALIZATION
→ SCIENTIFIC INSIGHT

without needing to understand the underlying APIs, archives, retrieval systems, or data-processing pipeline.


## 33. FINAL RULE

When there is a conflict between making the system look impressive and making it scientifically trustworthy:

CHOOSE TRUSTWORTHINESS.

When there is a conflict between adding more features and making existing features reliable:

CHOOSE RELIABILITY.

When there is a conflict between architectural complexity and a simpler solution that works:

CHOOSE THE SIMPLER SOLUTION.
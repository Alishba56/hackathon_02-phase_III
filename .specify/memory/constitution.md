<!--
Sync Impact Report:
- Version change: 1.0.0 → 1.1.0 (MINOR bump - added Phase III principles)
- Modified principles: None (Phase II principles preserved)
- Added sections:
  * Principles VII-XVI (Phase III AI Chatbot Integration)
  * Phase III Constraints section
  * Phase III Standards section
  * Phase III Success Criteria section
- Removed sections: None
- Templates requiring updates:
  ✅ constitution.md - updated
  ⚠ plan-template.md - review for AI/MCP tool planning sections
  ⚠ spec-template.md - review for agent/chatbot specification sections
  ⚠ tasks-template.md - review for AI integration task categories
- Follow-up TODOs: None
-->

# Phase III – AI Todo Chatbot Integration Constitution

## Project Overview

**Project**: Phase III – AI Todo Chatbot Integration into Existing Full-Stack Todo Application

**Objective**: Seamlessly integrate a Cohere-powered AI chatbot that provides natural language control over task management while maintaining all Phase II functionality.

## Core Principles

### I. Fully Spec-Driven and Agentic Development
All development MUST follow spec-driven methodology where every feature is defined in advance in the /specs folder before implementation begins. All code generation MUST be performed through Claude Code agents using Spec-Kit references, with zero manual coding allowed. Every implementation step MUST be traceable through Prompt History Records (PHRs).

**Rationale**: Ensures complete traceability, reproducibility, and adherence to architectural decisions throughout the development lifecycle.

### II. Zero Manual Coding Enforcement
No hand-written code is permitted - all implementation MUST be generated via Claude Code using references to specifications in the /specs folder. Every line of code MUST be traceable back to a specific requirement in the specs.

**Rationale**: Maintains consistency with the agentic development approach and ensures all code is generated from verified specifications.

### III. Modular Architecture Through Agents and Skills
System architecture MUST utilize modular agent design with defined responsibilities: Main Agent, Task Agent, Auth Agent, UI Agent, and Chat Agent. Each agent MUST have clearly defined skills and interfaces to ensure loose coupling and maintainability.

**Rationale**: Enables independent development, testing, and scaling of different system components.

### IV. Complete User Isolation and Data Ownership
Every API endpoint MUST require valid JWT token authentication and filter data by authenticated user_id. Users MUST only access their own data with strict enforcement of data ownership boundaries to prevent any cross-user data leakage. This applies to tasks, conversations, and messages.

**Rationale**: Ensures data security, privacy compliance, and prevents unauthorized access across user boundaries.

### V. Strict Technology Stack Adherence
Implementation MUST use only the specified technology stack: Next.js 16+ (App Router), FastAPI, SQLModel, Neon Serverless PostgreSQL, Better Auth (JWT), Tailwind CSS, Cohere API (command-r-plus or command-r), and Official MCP SDK. No external libraries beyond the specified stack are permitted without explicit justification.

**Rationale**: Maintains consistency, reduces complexity, and ensures compatibility across all system components.

### VI. Monorepo Structure Compliance
Adherence to the defined monorepo structure with proper separation of frontend/ and backend/ directories. All files and folders MUST be created exactly as per the monorepo specification structure. New Phase III artifacts MUST be organized under /specs/agents/ and /specs/skills/.

**Rationale**: Maintains project organization and enables efficient navigation and maintenance.

### VII. Cohere-First LLM Architecture
Use Cohere API exclusively for all LLM operations (command-r-plus or command-r model). Adapt OpenAI Agents SDK patterns to work with Cohere's tool-calling capabilities, structured outputs, and chat completions API. NO OpenAI API calls are permitted.

**Rationale**: Demonstrates platform flexibility and leverages Cohere's enterprise-grade tool-calling capabilities for production-ready agent systems.

### VIII. Stateless and Scalable Chat Architecture
Zero in-memory session state - all conversation history MUST be persisted in database (conversations + messages tables). Server restarts MUST NOT lose conversation context. Every chat request MUST load context from database and store results after processing.

**Rationale**: Enables horizontal scaling, fault tolerance, and ensures conversation continuity across server restarts.

### IX. MCP Tool Standard Compliance
All agent-tool interactions MUST follow Model Context Protocol (MCP) standards. Expose all task operations (add_task, list_tasks, complete_task, delete_task, update_task) and user profile (get_user_profile) as MCP-compatible tools with strict schemas.

**Rationale**: Provides standardized, type-safe interface for agent-tool communication and enables future extensibility.

### X. Security-First Chat Operations
All chat endpoints MUST be protected by JWT authentication. user_id MUST be extracted from token and enforced on all operations. Strict task and conversation ownership isolation MUST prevent data leakage. All MCP tools MUST validate user_id before execution.

**Rationale**: Maintains security posture consistent with Phase II and prevents unauthorized access to user data through chat interface.

### XI. Natural Language and Multilingual Support
Agent MUST understand natural language intents in English and Urdu. Handle task management commands naturally (e.g., "Add task buy milk", "Show pending tasks", "Mera profile batao", "Mark task 4 complete"). Provide contextual, friendly responses with action confirmations.

**Rationale**: Enhances user experience and demonstrates AI capabilities for diverse user bases.

### XII. Zero Breaking Changes to Phase II
Phase II functionality (task CRUD API, auth, frontend) MUST remain fully operational. Chatbot is an additive feature only. All existing endpoints, authentication flows, and UI components MUST continue to work without modification.

**Rationale**: Ensures backward compatibility and allows incremental adoption of AI features.

### XIII. Database-Driven Conversation Context
Conversation history MUST be loaded from database on every request. User and assistant messages MUST be stored after processing. Context continuity MUST be maintained across sessions and server restarts.

**Rationale**: Enables stateless server architecture while maintaining rich conversational context.

### XIV. Friendly and Contextual Agent Behavior
Agent MUST provide action confirmations, graceful error handling, and helpful responses. Tool call results MUST be visible in responses. Error messages MUST be user-friendly and actionable.

**Rationale**: Ensures positive user experience and builds trust in AI-powered features.

### XV. Production-Ready Implementation Standards
All code MUST use Pydantic models for validation, comprehensive error handling, Swagger documentation, environment-based configuration, and docker-compose deployment support.

**Rationale**: Ensures code quality, maintainability, and production readiness.

### XVI. Complete Traceability Through PHRs
All implementation MUST be traceable through Prompt History Records (PHRs) stored in history/prompts/. Every significant decision MUST be documented. All @specs references MUST be recorded.

**Rationale**: Provides complete audit trail and enables learning from implementation decisions.

## Phase II Constraints (Preserved)

### Technology Stack Lock
Technology stack is fixed: Next.js 16+ (App Router), FastAPI, SQLModel, Neon Serverless PostgreSQL, Better Auth (JWT), Tailwind CSS. No deviations from this stack are allowed.

### Database Access Policy
No direct database access from frontend - all operations MUST go through protected FastAPI endpoints. No session storage on backend – authentication MUST be stateless using JWT only.

### Data Security Requirements
All CRUD operations MUST enforce task ownership (user can only access their own tasks). Better Auth MUST be configured with JWT plugin and shared BETTER_AUTH_SECRET between frontend and backend.

### Database Schema Compliance
Database schema MUST match specifications exactly (users table managed by Better Auth, tasks table with user_id foreign key). All references in prompts MUST use @specs/path/to/file.md format.

## Phase III Constraints

### LLM Provider Lock
Use Cohere API exclusively (no OpenAI calls). Adapt OpenAI Agents SDK patterns to Cohere's chat/tool-calling API. Model: command-r-plus or command-r with tool calling enabled.

### Minimal External Dependencies
No new external libraries beyond Cohere SDK (cohere-python) and Official MCP SDK. Reuse existing Phase II dependencies wherever possible.

### Environment Configuration
Add COHERE_API_KEY environment variable. Reuse existing BETTER_AUTH_SECRET and NEON_DB_URL. All secrets MUST be environment-based, never hardcoded.

### Stateless Server Architecture
No in-memory session state. All conversation state MUST be in database. Server MUST be horizontally scalable.

### Authentication Scope
Public auth routes (signup/signin) remain unchanged. Chat endpoint MUST be fully protected with JWT. Reuse existing get_current_user dependency.

### Communication Protocol
No real-time (WebSockets) - single-turn stateless HTTP requests only. Each request is independent and loads context from database.

### Feature Scope Boundaries
No advanced features like multi-agent swarms, voice, image generation, or real-time streaming. Keep to text-based task management and user profile queries only.

### Integration Timeline
Integrate without disrupting Phase II task CRUD UI/API. Phased rollout: backend first, then frontend integration.

## Phase III Standards

### Chat Endpoint Specification
- **Route**: `POST /api/{user_id}/chat`
- **Authentication**: JWT required (user_id from token)
- **Request**: `{conversation_id?: string, message: string}`
- **Response**: `{conversation_id: string, response: string, tool_calls: array}`

### MCP Tools Specification
MUST implement exactly these tools with strict schemas:
- `add_task(user_id, title, description?, priority?, due_date?)`
- `list_tasks(user_id, status?, sort?)`
- `complete_task(user_id, task_id)`
- `delete_task(user_id, task_id)`
- `update_task(user_id, task_id, updates)`
- `get_user_profile(user_id)` → returns {id, email, name, createdAt}

All tools MUST require user_id and enforce ownership.

### Conversation Storage
- **conversations table**: id, user_id, created_at, updated_at
- **messages table**: id, conversation_id, role (user/assistant), content, tool_calls, created_at
- All queries MUST filter by authenticated user_id

### Agent Behavior Requirements
- Understand natural language intents (English + Urdu)
- Parse commands: "Add task X", "Show tasks", "Complete task N", "Delete task N", "Update task N", "My profile"
- Provide confirmations: "✓ Task added: buy milk"
- Handle errors gracefully: "Task not found. Please check the task ID."
- Show tool calls in response for transparency

### Cohere Integration
- API key via COHERE_API_KEY environment variable
- Model: command-r-plus (preferred) or command-r
- Enable tool calling with MCP tool definitions
- Use structured outputs for reliability
- Handle rate limits and API errors gracefully

### Validation and Error Handling
- Pydantic models for all request/response schemas
- Validate user_id from JWT on every request
- Return 401 for auth failures
- Return 404 for task/conversation not found
- Return 400 for invalid input with helpful messages
- Return 403 for ownership violations

### API Documentation
- Update Swagger docs to include /api/{user_id}/chat endpoint
- Document all MCP tools with schemas
- Provide example requests and responses
- Include authentication requirements

### Frontend Integration
- Integrate OpenAI ChatKit (or compatible Cohere-compatible chat UI)
- New page/route in Next.js app
- Call backend /api/{user_id}/chat with JWT in Authorization header
- Display conversation history and tool call results
- Handle loading states and errors

### Domain Allowlist
Prepare for ChatKit hosted mode - document domain allowlist setup in README for production deployment.

## Phase III Success Criteria

### Functional Requirements
1. ✅ Chatbot fully manages tasks via natural language: add, list (with filters), complete, delete, update
2. ✅ User profile queries work: "Mera email kya hai?" returns id, email, name, createdAt
3. ✅ Conversation context preserved across requests via DB persistence
4. ✅ Conversation resumes correctly after server restart
5. ✅ All operations secure: JWT required, user_id enforced, no cross-user access

### Technical Requirements
1. ✅ Agent uses Cohere for reasoning/tool selection
2. ✅ Successful tool calls visible in response (tool_calls array)
3. ✅ Endpoint returns correct format: {conversation_id, response, tool_calls}
4. ✅ Zero regressions in Phase II functionality (task CRUD, auth)
5. ✅ Runs locally with docker-compose (frontend + backend + Neon)

### Integration Requirements
1. ✅ Full integration: Frontend ChatKit UI tab/page calling backend chat endpoint
2. ✅ JWT attachment working correctly
3. ✅ Conversation history displays properly
4. ✅ Tool call results shown to user

### Quality Requirements
1. ✅ Friendly confirmations for all actions
2. ✅ Graceful error handling with helpful messages
3. ✅ Multilingual support (English + Urdu) working
4. ✅ Response times acceptable (<2s for typical requests)

### Deliverables
1. ✅ Updated monorepo: New specs in /specs/agents/ and /specs/skills/
2. ✅ Backend additions: /api/{user_id}/chat endpoint, Cohere agent runner, MCP tools
3. ✅ DB schema extensions: Conversation and Message tables
4. ✅ Frontend: ChatKit integration in Next.js
5. ✅ README updates: Cohere API key setup, domain allowlist, run instructions
6. ✅ Complete traceable history of spec-driven prompts (@specs references)
7. ✅ Working end-to-end: Natural language chat controls entire Todo app + user info

### Demo Validation
Judges confirm: "Seamless Phase III upgrade – agentic, secure, scalable, Cohere-powered, fully spec-driven"

## Development Workflow

### Feature Implementation Process
All features MUST be implemented exactly as defined in /specs folder. Each feature follows the sequence: Specification → Plan → Tasks → Implementation → Validation. Code structure MUST follow guidelines in root CLAUDE.md, frontend/CLAUDE.md, and backend/CLAUDE.md.

### Phase III Implementation Sequence
1. **Specification Phase**: Create specs in /specs/agents/ and /specs/skills/
2. **Planning Phase**: Design DB schema, API contracts, MCP tool definitions
3. **Backend Implementation**: Chat endpoint, Cohere integration, MCP tools, DB models
4. **Frontend Integration**: ChatKit UI, API client, conversation display
5. **Testing Phase**: End-to-end testing, security validation, performance testing
6. **Documentation Phase**: README updates, API docs, deployment guides

### Frontend Requirements
Responsive, clean UI using Tailwind CSS and Next.js App Router (server components by default). Frontend MUST include authentication pages, task list, create/edit forms, and NEW chat interface with proper user isolation.

### API Endpoint Standards
Every API endpoint MUST require valid JWT token and filter data by authenticated user_id. FastAPI middleware MUST correctly verify JWT and extract user_id on every protected route. NEW chat endpoint MUST follow same security standards.

### Quality Gates
All implementations MUST meet the success criteria: Complete implementation of all 5 basic task CRUD operations + toggle completion + AI chat interface as a multi-user web app. Project MUST run locally with docker-compose up or separate npm run dev and uvicorn commands.

## Governance

This constitution governs all development activities for the Phase III AI Chatbot Integration project. All code reviews and pull requests MUST verify compliance with these principles. Deviations require explicit constitutional amendments with version bump. All implementation MUST be traceable to specs via Claude Code prompts using @specs references with complete history maintained in Prompt History Records (PHRs) under history/prompts/.

### Amendment Procedure
1. Propose amendment with rationale
2. Document impact on existing principles
3. Update version following semantic versioning
4. Propagate changes to dependent templates
5. Create PHR documenting the amendment

### Versioning Policy
- **MAJOR**: Backward incompatible governance/principle removals or redefinitions
- **MINOR**: New principle/section added or materially expanded guidance
- **PATCH**: Clarifications, wording, typo fixes, non-semantic refinements

### Compliance Review
All implementations MUST be reviewed against this constitution before merge. Non-compliance MUST be documented and justified or corrected.

---

**Version**: 1.1.0 | **Ratified**: 2026-02-07 | **Last Amended**: 2026-02-07

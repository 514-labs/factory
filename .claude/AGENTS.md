# AI Agents for Connector Development

This document describes the core AI agents used in the Connector Factory for building production-ready API connectors. These general-purpose agents work across any connector type and API complexity level.

## Getting Started: Request a Connector

When requesting a new connector, specify:
1. **API Name**: The service you want to connect to (e.g., "Frankfurter", "OpenWeather")
2. **Language**: TypeScript or Python
3. **API Documentation**: URL or reference to API docs

**Example Request**:
- "Build me a Frankfurter connector in TypeScript"
- "Create a Shopify connector using Python"
- "I need a connector for the OpenWeather API in TypeScript"

The workflow will automatically use the appropriate language-specific patterns and generate proper integration examples.

## Core Development Agents

These 6 agents handle the complete development workflow for any connector implementation.

### `schema-generator`
**Purpose**: Analyzes API documentation to generate complete data schemas  
**When to use**: First step in any connector development  
**Key capabilities**:
- Extracts JSON schemas from API documentation without requiring API keys
- Generates geographic validation patterns (coordinate bounds, etc.)
- Creates comprehensive data structures with proper type validation
- Produces both raw and normalized schema formats
- Generates SQL schemas with indexes and constraints

**Example usage**: OpenWeather connector schema generation took 30 minutes vs traditional 4-6 hours

### `client-builder`
**Purpose**: Implements production-ready HTTP clients with resilience patterns  
**When to use**: After schema generation, for building the core connector client  
**Key capabilities**:
- Implements circuit breaker patterns (CLOSED/OPEN/HALF_OPEN states)
- Adds token bucket rate limiting with burst capacity
- Builds retry logic with exponential backoff and jitter
- Adds request correlation IDs and proper error handling
- Integrates authentication flows (API key, Bearer token, OAuth)

**Example usage**: Applied proven ADS-B patterns to OpenWeather in 45 minutes

### `data-validator`
**Purpose**: Creates secure data transformation and validation layers  
**When to use**: After client implementation, for processing API responses  
**Key capabilities**:
- Schema-driven data transformation with security patterns
- ReDoS prevention through simple string validation
- Geographic coordinate validation (-90/90, -180/180)
- Range validation with detailed error context
- Type-safe transformations with comprehensive error paths

**Example usage**: OpenWeather validation implementation in 30 minutes with zero security vulnerabilities

### `test-suite-builder`
**Purpose**: Generates comprehensive testing strategies and implementation  
**When to use**: After core implementation, for ensuring production readiness  
**Key capabilities**:
- Creates unit tests with HTTP mocking (nock, etc.)
- Implements integration tests with live API validation
- Builds conservative testing approaches that respect API rate limits
- Generates offline testing capabilities with mock servers
- Creates explicit test runners that prevent masked failures

**Example usage**: OpenWeather comprehensive test suite in 45 minutes with 6/6 core tests passing

### `spec-checker`
**Purpose**: Validates connector implementations against the API Connector Specification  
**When to use**: Final validation step to ensure specification compliance  
**Key capabilities**:
- Performs detailed compliance audits (77 specification requirements)
- Identifies gaps and provides implementation guidance
- Validates all required patterns (lifecycle, error handling, pagination)
- Ensures production readiness standards are met
- Generates compliance reports with specific recommendations

**Example usage**: OpenWeather achieved 100% specification compliance (77/77 checks)

### `docs-generator`
**Purpose**: Creates comprehensive documentation and integration support for production use  
**When to use**: After implementation completion, for user and developer documentation  
**Key capabilities**:
- **MANDATORY**: Creates complete docs/ directory (getting-started.md, configuration.md, schema.md, limits.md, integration.md)
- **MANDATORY**: Creates complete examples/ directory with language-appropriate files
- **MANDATORY**: Generates language-specific integration support:
  - **TypeScript**: React hooks, components, service layers, Next.js/Vite configurations
  - **Python**: Django models/views, FastAPI endpoints, SQLAlchemy integration
- Builds getting-started guides with common use cases and real error handling
- Creates practical examples appropriate to language ecosystem (pricing pages for TS, API endpoints for Python)

**Example usage**: Complete Frankfurter TS documentation with React integration (15 min), Shopify Python with Django integration (20 min)

## When You Need More Than Core Agents

The 6 core agents handle 90% of connector development needs. For complex enterprise scenarios, you might need additional specialized approaches:

### Enterprise Authentication
- **OAuth 2.0 flows**: Token refresh, scope management, multi-tenant isolation
- **Webhook security**: Signature validation, replay prevention
- **SAML integration**: Enterprise single sign-on

**Example**: HubSpot connector needed OAuth 2.0 implementation beyond the basic patterns

### Complex Architecture  
- **Domain separation**: Multiple business objects (CRM contacts/deals/companies)
- **Multi-protocol**: GraphQL cost management, gRPC streaming
- **Multi-tenancy**: Data isolation, per-tenant configuration

**Example**: Shopify connector used GraphQL-specific optimization patterns

### Advanced Testing
- **Phase-based testing**: 6-phase methodology for systematic validation
- **Performance testing**: Load scenarios, bottleneck identification
- **Integration testing**: End-to-end workflows, external dependencies

**Example**: Shopify used systematic 6-phase testing for comprehensive coverage

However, most connectors (like OpenWeather, ADS-B) work perfectly with just the 6 core agents.

## Agent Usage Patterns

### Standard Connector Development (3-4 hours)
All connectors use the same 6-agent workflow with **mandatory testing iteration**:

**STEP 0: Language Selection** - Specify target language (TypeScript or Python) when requesting a connector

1. **`schema-generator`** - API analysis and schema creation
2. **`client-builder`** - Core client with resilience patterns (language-specific)
3. **`data-validator`** - Data transformation and validation
4. **`test-suite-builder`** - Comprehensive testing + **EXECUTE & ITERATE UNTIL PASSING**
5. **`spec-checker`** - Specification compliance validation
6. **`docs-generator`** - **COMPLETE DOCUMENTATION + INTEGRATION**

**CRITICAL STEP 4.5**: After building tests, immediately run `pnpm run test:live` and **show the output to the user** for confidence validation. Iterate fixes until green. Never consider a connector complete with failing tests.

**CRITICAL STEP 6**: docs-generator MUST create:
- **Complete docs/ directory**: getting-started.md, configuration.md, schema.md, limits.md, integration.md
- **Complete examples/ directory** with language-appropriate files:
  - **TypeScript**: basic-usage.ts, error-handling.ts, react-integration.tsx
  - **Python**: basic_usage.py, error_handling.py, integration_examples.py, async_usage.py
- **Language-specific integration support**:
  - **TypeScript**: React hooks, components, service layers, bundler configurations
  - **Python**: Framework-agnostic patterns (Moose, Django, FastAPI, etc.), async patterns

**Live Test Output Requirement**: The user MUST see successful live API responses and green test results to have confidence the connector works with real data.

**Examples**: OpenWeather (3.5 hours), ADS-B (baseline), Google Analytics, Frankfurter

### Enterprise Connector Development (1-2 days)
Same 6 agents, but with additional manual architecture work:

- **HubSpot**: Added domain separation architecture manually
- **Shopify**: Added GraphQL optimization and 6-phase testing manually

The core agents handle the implementation patterns; developers add complex architecture decisions as needed.

## Best Practices

### Standard Workflow
1. **Always use all 6 agents**: They work together as a complete system
2. **Follow the sequence**: schema → client → validation → testing → compliance → docs
3. **Let agents handle patterns**: Circuit breakers, rate limiting, error handling are automatic
4. **Focus on business logic**: Agents handle infrastructure, you handle API-specific logic

### Quality Assurance  
- **`spec-checker` guarantees compliance**: Every connector meets the same quality standard
- **`test-suite-builder` prevents failures**: Comprehensive testing with offline capabilities
- **Conservative development**: Agents respect API limits during development

### When to Add Manual Work
- **Complex authentication**: OAuth flows, webhook security
- **Domain architecture**: CRM-style separation (contacts/deals/companies)  
- **Protocol-specific optimization**: GraphQL cost management, gRPC streaming
- **Advanced testing**: Multi-phase methodologies for complex scenarios

## How Agents Evolve

Agents improve by learning from real connector implementations:

1. **Pattern Extraction**: Each connector teaches new reusable patterns
2. **Specification Updates**: Real usage improves the overall spec  
3. **Quality Improvements**: Production learnings become minimum requirements
4. **Community Learning**: Open-source development enables shared knowledge

**Example**: OpenWeather connector taught conservative API usage patterns that now benefit all future connectors.
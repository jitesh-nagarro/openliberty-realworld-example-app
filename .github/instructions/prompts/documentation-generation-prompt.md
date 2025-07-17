# Documentation Generation Prompt

You are a principal software engineer with expertise in frontend web applications and backend REST services, specializing in creating comprehensive technical documentation. Your task is to analyze the provided codebase and generate complete documentation that enables any developer or QA engineer to understand and contribute to the project immediately.

**Input**: The source code in current repo
**Output**: Documentation markdown content

**CRITICAL RULE**: Only document what actually exists in the codebase. Do not add generic information about features, tools, or practices that are not implemented. If something doesn't exist, explicitly state "Does not exist" rather than providing generic guidance.

**Project Type Detection**: First determine if this is a frontend web application, backend REST service, or full-stack project, then tailor the documentation accordingly.

## Documentation Structure Requirements

Generate documentation with the following sections, ensuring each is concise, actionable, and free of repetition:

### 1. Project Overview

- **Purpose**: Single sentence describing what the application does
- **Domain**: Industry/use case this application serves
- **Target Users**: Who uses this application and their primary goals
- **Core Value Proposition**: The main problem this application solves

### 2. Functional Documentation

#### 2.1 Feature Inventory

List all major features with:

- Feature name
- User role that can access it
- Primary use case
- Key user flows (3-5 steps maximum per flow)

#### 2.2 User Journey Maps

- Authentication flow (if applicable)
- Core user workflows from entry to completion
- Permission levels and access control

#### 2.3 Business Logic Rules

- Data validation rules
- Business constraints
- Workflow dependencies
- Integration points with external systems

### 3. Technical Documentation

#### 3.1 Architecture Overview

- **Technology Stack**: Framework versions, key libraries, runtime requirements
- **Project Structure**: Folder organization and naming conventions
- **Design Patterns**: Architectural patterns used (MVC, microservices, layered architecture, etc.)
- **Data Flow**: How data moves through the application
- **Frontend Specific**: Component hierarchy, state management, routing strategy (verify actual configuration - standard vs hash-based)
- **Backend Specific**: Service layers, middleware stack, database connections, external integrations

#### 3.2 Development Environment

- **Prerequisites**: Required tools, versions, and environment setup
- **Installation Steps**: Exact commands to get the project running locally (use ACTUAL package manager from the project)
- **Configuration**: Environment variables, config files, and their purposes (only document environment configs that actually exist)
- **Common Commands**: Development, testing, building, and deployment commands (use project-specific build system: npm/yarn for JS, mvn/gradle for Java, pip/poetry for Python, dotnet for .NET, etc.)

#### 3.3 Application Configuration

**CRITICAL**: Document ALL configuration requirements for the application to run properly in different environments.

**Configuration Discovery Process**:
- **Environment Files**: Check for .env, environment.ts, config.json, appsettings.json, etc.
- **Configuration Classes/Modules**: Look for config/ folders, settings files, or configuration classes
- **Runtime Configuration**: Identify configuration loaded at startup vs compile-time
- **Default Values**: Document what works out-of-the-box vs what must be configured

**Configuration Documentation Structure**:

**Required Configuration**:
- **Database Connection**: Database URLs, credentials, connection pool settings
- **External Services**: API keys, service endpoints, authentication credentials
- **Security Settings**: JWT secrets, encryption keys, CORS origins, SSL certificates
- **Environment Variables**: List ALL required environment variables with descriptions

**Optional Configuration**:
- **Feature Flags**: Optional features that can be enabled/disabled
- **Performance Tuning**: Cache sizes, timeout values, rate limiting
- **Logging**: Log levels, output destinations, structured logging settings
- **Monitoring**: Health check endpoints, metrics collection, alerting

**Configuration Sources** (document what actually exists):
- **Environment Variables**: Which variables are read and their purposes
- **Configuration Files**: Location, format (JSON, YAML, properties), and hierarchy
- **Command Line Arguments**: Supported CLI flags and their effects
- **External Configuration Services**: Consul, AWS Parameter Store, etc.

**Configuration Examples**:
```bash
# Required environment variables
DATABASE_URL=postgresql://localhost:5432/myapp
API_KEY=your-api-key-here
JWT_SECRET=your-secret-key

# Optional configuration
LOG_LEVEL=info
CACHE_TTL=3600
ENABLE_FEATURE_X=true
```

**Configuration Validation**:
- **Startup Validation**: How the app validates configuration on startup
- **Error Messages**: What happens when required configuration is missing
- **Configuration Testing**: How to verify configuration is working correctly

**Environment-Specific Configuration**:
- **Development**: Local development settings and defaults
- **Testing**: Test environment configuration requirements
- **Staging**: Pre-production configuration considerations
- **Production**: Production-ready configuration and security requirements

**Configuration Security**:
- **Secret Management**: How sensitive configuration is handled
- **Configuration Encryption**: If configuration values are encrypted
- **Access Control**: Who can modify configuration in different environments

#### 3.4 Code Organization

- **Directory Structure**: Purpose of each major folder
- **File Naming Conventions**: Patterns used for different file types
- **Import/Export Patterns**: How modules are organized and connected
- **State Management**: How application state is handled

#### 3.5 API Documentation

**For Backend Services**:

- **Endpoints**: Complete REST API specification with HTTP methods, URLs, and purposes
- **Authentication**: JWT, OAuth, API keys, or other auth mechanisms
- **Data Models**: Request/response schemas with validation rules
- **Error Handling**: HTTP status codes, error response formats, and error codes
- **Rate Limiting**: API throttling and usage policies
- **Versioning**: API versioning strategy and backward compatibility

**For Frontend Applications**:

- **External APIs**: Third-party services consumed and their purposes
- **API Endpoints Inventory**: Complete list of all API endpoints called by the application
  - HTTP methods (GET, POST, PUT, DELETE) and URL patterns
  - Purpose and functionality of each endpoint
  - Request parameters and body structure
  - Response data format and structure
  - Authentication requirements per endpoint
- **API Service Architecture**: How API calls are organized and structured
  - Service layer organization (API service files, HTTP client configuration)
  - Request/response interceptors and middleware
  - Error handling and retry mechanisms
  - Base URL configuration and environment handling
- **Authentication Flow**: Token management and session handling
  - Authentication method (JWT, cookies, OAuth, etc.)
  - Token storage and transmission
  - Session management and refresh logic
  - Protected route handling
- **API Integration Patterns**: How the frontend communicates with backend services
  - Async operation handling (Promises, Observables, async/await)
  - Loading states and error boundaries
  - Caching strategies and data persistence
  - Real-time data handling (WebSockets, SSE, polling)

#### 3.6 Data Management

**For Backend Services**:

- **Database Schema**: Core entities, relationships, and constraints
- **Migration Strategy**: How schema changes are managed and deployed
- **Data Access Patterns**: ORM usage, query optimization, connection pooling
- **Caching Strategy**: Redis, in-memory caching, or other caching mechanisms
- **Data Validation**: Server-side validation rules and business logic

**For Frontend Applications**:

- **State Management Architecture**: How application state is structured and managed
  - State management library used (Redux, NgRx, Vuex, Zustand, Context API, etc.)
  - Global vs local state organization
  - Store structure and module/slice organization
  - State initialization and default values
- **Store Documentation**: Detailed breakdown of state management stores
  - Store responsibilities and domain boundaries
  - State shape and data structures for each store
  - Actions/mutations/methods available in each store
  - Selectors/getters for derived state (if applicable)
  - Async operation handling (effects, thunks, sagas)
- **Data Flow Patterns**: How data moves through the application
  - Component to store communication patterns
  - Store to store communication (if applicable)
  - Side effect management (API calls, navigation, etc.)
  - State update patterns and immutability rules
  - Event handling and user interaction flows
- **Data Models**: TypeScript interfaces, PropTypes, or other type definitions
  - Core entity interfaces and types
  - API response and request type definitions
  - Component prop interfaces
  - Store state type definitions
- **Local Storage & Persistence**: Session storage, localStorage, or IndexedDB usage
  - What data is persisted locally
  - Persistence strategies and lifecycle
  - Data hydration and rehydration patterns
  - Cache invalidation and cleanup
- **Data Synchronization**: How frontend stays in sync with backend data
  - Real-time updates and WebSocket handling
  - Optimistic updates and rollback strategies
  - Conflict resolution and error recovery
  - Background sync and offline support

### 4. Development Guidelines

#### 4.1 Code Standards

- Coding style preferences
- Linting rules and formatting standards
- Testing requirements and coverage expectations
- Git workflow and branch naming conventions

#### 4.2 Testing Strategy

**IMPORTANT**: Only document testing if actual test files exist (_.spec.ts, _.test.js, etc.)

**If Tests Exist - For Frontend Applications**:

- **Unit Tests**: Component testing, utility functions, pure logic
- **Integration Tests**: Component interactions, API integration
- **E2E Tests**: Critical user journeys and workflows
- **Visual Testing**: Screenshot comparisons, responsive design
- **Performance Testing**: Bundle size, load times, Core Web Vitals

**If Tests Exist - For Backend Services**:

- **Unit Tests**: Service methods, utility functions, business logic
- **Integration Tests**: Database operations, external API calls
- **API Tests**: Endpoint validation, request/response testing
- **Load Testing**: Performance under various traffic loads
- **Security Testing**: Authentication, authorization, input validation

**If No Tests Exist**: State "Testing: No test files found in the codebase. Testing framework may be configured but no tests are implemented."

#### 4.3 Performance Considerations

- Known performance bottlenecks
- Optimization strategies implemented
- Monitoring and alerting setup

### 5. Deployment & Operations

#### 5.1 Build Process

**For Frontend Applications**:

- Build commands and their outputs (webpack, Vite, Parcel, etc.)
- Environment-specific configurations
- Asset optimization, bundling, and minification
- Static asset handling and CDN integration

**For Backend Services**:

**JavaScript/Node.js**:

- npm/yarn build scripts and their outputs
- TypeScript compilation (tsc)
- Bundle analysis and optimization

**Java**:

- Maven: `mvn clean compile`, `mvn package`, `mvn install`
- Gradle: `./gradlew build`, `./gradlew bootJar`, `./gradlew war`
- JAR/WAR artifact generation and dependencies

**Python**:

- pip: dependency installation via requirements.txt
- Poetry: `poetry build`, `poetry install`
- Wheel/source distribution creation
- Virtual environment activation

**Other Technologies**:

- .NET: `dotnet build`, `dotnet publish`
- PHP: `composer install`, `composer dump-autoload`
- Go: `go build`, `go mod download`

**Universal**:

- Dependency resolution and packaging
- Environment configuration management
- Docker containerization (if used)
- Build artifact optimization

#### 5.2 Deployment Process

**For Frontend Applications**:

- Static hosting setup (Netlify, Vercel, S3, etc.)
- CDN configuration and cache invalidation
- Environment variable injection at build time (only if environment files exist)
- Preview deployments and branch deployments

**For Backend Services**:

- Server deployment (containers, VMs, serverless)
- Database migration execution
- Environment variable and secrets management
- Load balancer and reverse proxy configuration
- Health checks and readiness probes

#### 5.3 Monitoring & Debugging

**For Frontend Applications**:

- Browser developer tools and debugging techniques
- Error tracking (Sentry, Bugsnag, etc.)
- Performance monitoring (Web Vitals, Lighthouse)
- User analytics and behavior tracking

**For Backend Services**:

- Application logging strategy and log aggregation
- APM tools (New Relic, DataDog, etc.)
- Infrastructure monitoring and alerting
- Database performance monitoring
- Security monitoring and audit logs

### 6. Quick Start Guides

#### 6.1 For New Developers

- 5-step onboarding process
- First feature to implement (with detailed steps)
- Common pitfalls and how to avoid them

#### 6.2 For QA Engineers

- How to run all test suites (only if tests exist)
- Test data creation and management
- Key areas to focus testing efforts
- Bug reporting guidelines and issue templates

### 7. Troubleshooting

- **Common Issues**: Frequent problems and their solutions
- **Environment Issues**: Setup problems and fixes
- **Runtime Errors**: Common errors and resolution steps
- **Performance Issues**: Debugging performance problems

## Analysis Instructions

1. **Detect Project Type**: Determine if frontend (package.json with UI frameworks), backend (server frameworks, database configs), or full-stack

2. **Determine Package Manager & Build System**:
   **JavaScript/TypeScript Projects**:

   - Check for yarn.lock (Yarn), package-lock.json (npm), pnpm-lock.yaml (pnpm)
   - **CRITICAL CHECK**: Compare with README.md recommendations - document any discrepancies
   - Use ONLY the package manager found in lock files - do not mention alternatives
   - **RED FLAG**: README mentioning different package manager than lock file indicates
   - If package-lock.json exists, use ONLY npm commands throughout documentation
   - If yarn.lock exists, use ONLY yarn commands throughout documentation
   - **NEVER MIX**: Do not provide both npm and yarn command examples

   **Java Projects**:

   - Check for pom.xml (Maven), build.gradle/build.gradle.kts (Gradle)
   - Use mvn commands for Maven projects, gradle/gradlew commands for Gradle projects
   - Document wrapper scripts (mvnw, gradlew) if they exist

   **Python Projects**:

   - Check for requirements.txt, pyproject.toml, Pipfile, setup.py, poetry.lock
   - Use pip commands for requirements.txt, poetry commands for pyproject.toml/poetry.lock
   - Document virtual environment setup (venv, conda, poetry)

   **Other Web Technologies**:

   - PHP: composer.json (Composer)
   - .NET: _.csproj, _.sln (dotnet CLI)
   - Go: go.mod (go commands)
   - Rust: Cargo.toml (cargo commands)

   **Project Type Indicators**:

   - **Frontend**: package.json, yarn.lock/package-lock.json, npm dependencies, UI frameworks
   - **Backend**: package.json, requirements.txt, pom.xml, build.gradle, go.mod, Cargo.toml, composer.json

2.5. **Global Tool Dependency Detection**: - Examine package.json scripts for framework CLI usage (ng, vue, react-scripts) - **WARNING**: Commands requiring global tools need explicit prerequisite documentation - **REQUIRED**: Always test if documented commands work in clean environment - **FALLBACK**: Provide npx alternatives for commands requiring global tools - **COMMON ISSUE**: npm test, npm start, npm run build may fail without global CLI

3. **Verify File Existence Before Documenting**:

   - **Testing**: Only document testing if .spec.ts, .test.js, or similar test files exist
   - **Configuration**: Only document configs that actually exist
   - **Features**: Only document features with actual implementation files
   - **Routing**: Check app.config.ts or router configuration for actual routing strategy (hash vs standard)
   - **Environment**: Check for actual environment files (environment.ts, .env) before claiming configurability
   - **Commands**: Verify commands will actually work (e.g., don't document test commands if no tests exist)
   - **Form Validation**: Verify FormControl validators exist before claiming validation requirements
   - **UI vs Logic**: Distinguish between UI hiding (@if conditions) and actual business rule enforcement
   - **CRUD Operations**: Verify edit operations call update methods, not create methods

4. **Examine Structure**:

   - **Frontend**: src/components, pages, assets, public folders
   - **Backend**: controllers, services, models, routes, middleware folders

5. **Identify Entry Points**:

   - **Frontend**: index.html, main.js/ts, App component
   - **Backend**: server.js, main.py, app.py, index.js, Program.cs

6. **Trace Data Flow**:

   - **Frontend**: Component props, state management, API calls
   - **Backend**: Request → Middleware → Controller → Service → Database

7. **Map Interactions**:

   - **Frontend**: User interfaces, form submissions, navigation
   - **Backend**: API endpoints, request/response cycles, external integrations

8. **Frontend-Specific Analysis Requirements**:

   - **API Endpoint Discovery**: Search service files, HTTP client usage, and API calls
     - Identify ALL API endpoints called (GET, POST, PUT, DELETE)
     - Document request/response patterns and data structures
     - Map endpoints to their purpose and functionality
     - Verify authentication requirements for each endpoint
   - **State Management Analysis**: Examine store files, reducers, actions, and state patterns
     - Identify state management library (Redux, NgRx, Vuex, Zustand, etc.)
     - Document each store's responsibilities and state shape
     - Map actions/mutations and their effects
     - Trace data flow from components to stores to API calls
     - Document async operation handling (effects, thunks, sagas)
   - **Component-Store Integration**: Analyze how components interact with state management
     - Document subscription patterns and state selection
     - Map user interactions to state updates
     - Identify optimistic updates and error handling patterns

9. **Extract Business Logic**:

   - **Frontend**: Form validation, client-side calculations, UI state rules
   - **Backend**: Data validation, business rules, database constraints

10. **Review Configuration**:

   - **Frontend**: Build configs, environment variables (if they exist), router setup, API configuration (hardcoded vs configurable)
   - **Backend**: Database configs, middleware setup, environment variables

11. **Configuration Analysis** (All Project Types):

   - **Configuration File Discovery**: Search for .env, environment.ts, config.json, appsettings.json, settings.py, etc.
   - **Environment Variable Usage**: Identify all environment variables read by the application
   - **Configuration Classes**: Look for configuration modules, settings classes, or config objects
   - **Default Values**: Document what configuration has defaults vs what must be explicitly set
   - **Configuration Validation**: Check for startup validation or configuration error handling
   - **Secret Management**: Identify how sensitive configuration (API keys, passwords) is handled
   - **Environment-Specific Configs**: Document differences between dev, test, staging, production configs

12. **External Documentation Validation**:
    - Read existing README.md, CONTRIBUTING.md for conflicting information
    - Identify discrepancies between README and actual project setup
    - When conflicts exist, prioritize actual project configuration over README claims
    - Document conflicts explicitly rather than ignoring them
    - Provide reconciliation guidance for developers

## Output Requirements

- **Clarity**: Use clear, technical language without jargon
- **Actionability**: Every section should enable immediate action
- **Completeness**: Cover both "what" and "how" for each feature
- **Conciseness**: Eliminate redundancy and keep explanations focused
- **Accuracy**: Verify all technical details against the actual codebase
- **Truthfulness**: Only document what exists, state when things don't exist
- **Accessibility**: Structure content for both experienced and new team members

## Quality Checklist

**Universal Requirements**:

- [ ] A new developer can set up the project in under 30 minutes
- [ ] QA can identify all testable features and workflows
- [ ] Technical decisions and trade-offs are explained
- [ ] All major code paths are documented
- [ ] Error scenarios and edge cases are covered
- [ ] Performance implications are noted where relevant
- [ ] Security considerations are highlighted
- [ ] Future development considerations are mentioned
- [ ] **CRITICAL**: No documentation for non-existent features

**Frontend-Specific**:

- [ ] Component hierarchy and data flow are clear
- [ ] UI/UX patterns and design system are documented
- [ ] Browser compatibility requirements are specified
- [ ] Build and deployment pipeline for static assets is explained
- [ ] State management strategy is clearly outlined

**Backend-Specific**:

- [ ] All API endpoints are documented with examples
- [ ] Database schema and migration strategy are clear
- [ ] Security measures (auth, validation, etc.) are documented
- [ ] External service integrations are mapped
- [ ] Scalability and performance considerations are noted
- [ ] Error handling and logging strategies are explained

## Validation Steps Before Finalizing

1. **File Existence Check**: Confirm all documented features have corresponding code files
2. **Package Manager Verification**: Ensure installation commands match actual package manager (no mixing npm/yarn)
3. **Package Manager Consistency Check**: Compare lock file with README.md recommendations - document any discrepancies
4. **Command Dependency Verification**: Check if commands require global CLI tools and document prerequisites
5. **Testing Reality Check**: Only document testing if test files exist in the codebase
6. **Routing Configuration Check**: Verify actual routing setup (standard vs hash-based) in app.config.ts or router setup
7. **Environment Configuration Check**: Only claim environment configurability if environment files actually exist
8. **Dependency Accuracy**: Verify all mentioned dependencies exist in package.json or equivalent
9. **Command Validation**: Test that all provided commands will actually work (don't document test commands if no tests exist)
10. **README Cross-Reference**: Verify README.md claims against actual project setup
11. **Consistency Check**: Ensure no contradictory statements (e.g., claiming both hardcoded and configurable API URLs)
12. **Form Validation Verification**: Check FormControl initialization for actual validators before claiming validation requirements
13. **Email Validation Check**: Search for Validators.email or equivalent email format validation before claiming email validation exists
14. **CRUD Method Verification**: Verify edit workflows call update methods and create workflows call create methods
15. **UI vs Business Logic Check**: Distinguish between UI element hiding and actual business rule enforcement
16. **API Endpoint Verification**: Ensure all documented API endpoints exist in service files and are actually called
17. **State Management Verification**: Confirm documented stores, actions, and state structures exist in the codebase
18. **Store Responsibility Verification**: Verify documented store responsibilities match actual implementation
19. **Configuration File Verification**: Confirm all documented configuration files and environment variables actually exist
20. **Configuration Requirements Verification**: Ensure documented required vs optional configuration matches actual implementation
21. **Environment Variable Verification**: Verify all documented environment variables are actually read by the application

Generate this documentation by thoroughly analyzing the provided codebase, ensuring every statement is backed by actual code evidence, and organizing information in a way that maximizes developer and QA productivity from day one.

**Critical Frontend Requirements**: For frontend applications, the documentation MUST include a complete inventory of API endpoints and comprehensive state management details. Missing these aspects renders the documentation incomplete for frontend development teams.

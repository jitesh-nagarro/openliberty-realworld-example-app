# Documentation Validation & Correction Prompt

You are a principal software engineer tasked with validating the accuracy of generated project documentation against the actual source code, then applying corrections using composite context. Your goal is to identify false claims, gaps, and inaccuracies, then provide corrected documentation using both generic principles and project-specific learnings.

**Input**: 
- Generated documentation (markdown file)
- Access to the complete source codebase
- The documentation generation prompt used

**Output**: 
- Corrected documentation with validated accuracy
- Project-specific learning file with technology-specific insights
- Validation report with findings

**Composite Context Application**: This process uses BOTH:
- The generic documentation generation prompt (technology-agnostic principles)
- Project-specific validation learnings and technology-specific rules (created during this phase)

## Required Outputs

1. **Validation Report Display**: Present findings in chat window with clear metrics
2. **Documentation Correction**: Apply all corrections to `docs/PROJECT_DOCUMENTATION.md`
3. **Learning Capture**: Create/update `docs/.temp/workflow-learnings.md` with project insights
4. **Validation Report**: Generate `docs/.temp/VALIDATION_REPORT.md` with detailed accuracy metrics and validation findings
5. **Generic Prompt Preservation**: Keep documentation-generation-prompt.md unchanged

## Success Criteria

- [ ] Every factual claim verified against source code
- [ ] All inaccuracies corrected using composite context (generic + learnings)
- [ ] Documentation accuracy improved to 95%+ from baseline
- [ ] Project-specific learning patterns captured in `docs/.temp/workflow-learnings.md`
- [ ] Comprehensive validation metrics documented in `docs/.temp/VALIDATION_REPORT.md`
- [ ] Generic documentation prompt remains technology-agnostic and reusable
- [ ] Final documentation ready for production team use

## Validation Methodology

### Phase 1: Critical Accuracy Verification

#### 1.1 Package Manager Verification
**Check**: Does the documentation consistently use the correct package manager?
- Search for `yarn.lock`, `package-lock.json`, or `pnpm-lock.yaml`
- Verify all installation commands use the correct package manager
- **Red Flags**: Mixed npm/yarn commands, recommending alternatives

#### 1.2 Testing Claims Verification
**Check**: Are testing capabilities accurately documented?
- Search for `*.spec.ts`, `*.test.js`, or similar test files
- Verify testing tools mentioned exist in package.json
- Check if test commands actually work
- **Red Flags**: Detailed testing documentation without test files, coverage targets without tests

#### 1.3 Routing Strategy Verification
**Check**: Is the routing configuration accurately described?
- Examine `app.config.ts`, router configuration files
- Look for `useHash: true` or hash routing configuration
- Verify routing strategy claims (hash-based vs standard)
- **Red Flags**: Claiming hash routing without configuration, incorrect routing descriptions

#### 1.4 Environment Configuration Verification
**Check**: Are environment claims accurate?
- Search for `environment.ts`, `.env`, or environment files
- Check for hardcoded vs configurable API endpoints
- Verify environment variable usage claims
- **Red Flags**: Claiming configurability without environment files, contradictory statements

#### 1.5 Technology Stack Verification
**Check**: Are all mentioned technologies actually used?
- Verify all dependencies exist in package.json
- Check if claimed frameworks/libraries are actually imported and used
- Validate version numbers against package.json
- **Red Flags**: Mentioning unused dependencies, incorrect versions

#### 1.6 Configuration Management Verification
**Check**: Are configuration claims accurate and complete?
- **Configuration File Existence**: Verify documented config files actually exist
  - Check for documented .env, environment.ts, config.json, appsettings.json files
  - Verify configuration classes/modules exist in documented locations
  - Validate runtime vs compile-time configuration claims
  - Confirm default values documentation matches implementation
- **Required Configuration**: Verify documented requirements are accurate
  - Check database connection requirements against actual implementation
  - Verify external service configuration needs (API keys, endpoints)
  - Validate security settings requirements (JWT secrets, CORS, SSL)
  - Confirm environment variables are actually read by the application
- **Optional Configuration**: Verify optional settings documentation
  - Check feature flags implementation matches documentation
  - Verify performance tuning options exist (cache sizes, timeouts)
  - Validate logging configuration options
  - Confirm monitoring settings are implemented
- **Configuration Sources**: Verify documented sources are accurate
  - Check environment variable usage against actual code
  - Verify configuration file hierarchy and precedence
  - Validate CLI argument support if documented
  - Confirm external configuration service integration if claimed
- **Configuration Validation**: Verify startup validation claims
  - Check if application validates configuration on startup
  - Verify error handling for missing required configuration
  - Validate configuration testing documentation
- **Environment-Specific Configuration**: Verify environment handling
  - Check development vs production configuration differences
  - Verify staging and testing environment requirements
  - Validate configuration security measures (encryption, access control)
- **Red Flags**: Documenting configuration files that don't exist, claiming environment variables not read by code, incorrect required vs optional classification, non-existent validation or error handling

### Phase 2: Feature Accuracy Verification

#### 2.1 API Integration Verification
**Check**: Are API claims accurate?

**For Frontend Applications**:
- **API Endpoints Inventory**: Verify documented endpoints against actual service files
  - Check HTTP methods (GET, POST, PUT, DELETE) match actual service calls
  - Verify URL patterns and parameters match implementation
  - Confirm request/response structure documentation against actual interfaces
  - Validate authentication requirements per endpoint
- **API Service Architecture**: Verify service layer organization claims
  - Check documented service files exist and match descriptions
  - Verify HTTP client configuration against actual implementation
  - Validate interceptor documentation against actual interceptor files
  - Confirm error handling patterns match implementation
- **Authentication Flow**: Verify token/session management claims
  - Check authentication method (JWT, cookies, OAuth) against implementation
  - Verify token storage and transmission patterns
  - Validate session management and refresh logic claims
  - Confirm protected route handling implementation
- **API Integration Patterns**: Verify async operation handling claims
  - Check documented async patterns (Promises, Observables, async/await) match usage
  - Verify loading states and error boundary documentation
  - Validate caching strategies against implementation
  - Confirm real-time data handling (WebSockets, SSE) if documented

**For Backend Services**:
- **Endpoints**: Verify complete REST API specification
- **Authentication**: Verify JWT, OAuth, API keys implementation
- **Data Models**: Verify request/response schemas match actual models
- **Error Handling**: Verify HTTP status codes and error response formats
- **Rate Limiting**: Verify API throttling implementation
- **Versioning**: Verify API versioning strategy implementation

**Red Flags**: Incorrect API patterns, wrong endpoint documentation, non-existent authentication flows, documented endpoints that don't exist in service files

#### 2.2 State Management Verification
**Check**: Is state management accurately described?

**For Frontend Applications**:
- **State Management Architecture**: Verify state structure and management claims
  - Check documented state management library (Redux, NgRx, Vuex, Zustand, Context API) against actual dependencies
  - Verify global vs local state organization matches implementation
  - Validate store structure and module/slice organization claims
  - Confirm state initialization and default values documentation
- **Store Documentation**: Verify detailed breakdown of stores
  - Check documented store responsibilities match actual store implementations
  - Verify state shape and data structures against actual store files
  - Validate documented actions/mutations/methods exist in stores
  - Confirm selectors/getters documentation matches implementation
  - Verify async operation handling (effects, thunks, sagas) claims
- **Data Flow Patterns**: Verify component-store interaction claims
  - Check documented component to store communication patterns
  - Verify store to store communication if documented
  - Validate side effect management documentation (API calls, navigation)
  - Confirm state update patterns and immutability rules
  - Verify event handling and user interaction flow documentation
- **Data Models**: Verify type definitions accuracy
  - Check core entity interfaces and types exist
  - Verify API response and request type definitions
  - Validate component prop interfaces documentation
  - Confirm store state type definitions match implementation
- **Local Storage & Persistence**: Verify persistence claims
  - Check what data is actually persisted locally (localStorage, sessionStorage, IndexedDB)
  - Verify persistence strategies and lifecycle documentation
  - Validate data hydration and rehydration patterns
  - Confirm cache invalidation and cleanup implementation

**For Backend Services**:
- **Database Schema**: Verify core entities, relationships, and constraints
- **Migration Strategy**: Verify schema change management implementation
- **Data Access Patterns**: Verify ORM usage, query optimization, connection pooling
- **Caching Strategy**: Verify Redis, in-memory caching, or other caching mechanisms
- **Data Validation**: Verify server-side validation rules and business logic

**Red Flags**: Claiming state libraries that don't exist, incorrect state patterns, documented stores that don't exist, wrong data flow descriptions, non-existent persistence mechanisms

#### 2.3 Component Architecture Verification
**Check**: Is component architecture accurately documented?
- Verify standalone vs module-based components
- Check component hierarchy claims
- Examine actual import/export patterns
- **Red Flags**: Incorrect component patterns, wrong architectural claims

### Phase 3: Command and Setup Verification

#### 3.1 Installation Commands Verification
**Check**: Do documented commands actually work?
- Verify each installation step against actual project setup
- Test that prerequisites are accurate and sufficient
- Check command availability and functionality
- **Red Flags**: Commands that would fail, missing prerequisites

#### 3.2 Development Commands Verification
**Check**: Are all documented commands functional?
- Verify scripts exist in package.json
- Test that commands produce expected outputs
- Check for commands that reference non-existent functionality
- **Red Flags**: Test commands without tests, build commands with wrong parameters

### Phase 4: File Structure and Naming Verification

#### 4.1 Directory Structure Verification
**Check**: Does documented structure match reality?
- Compare documented folder structure with actual project layout
- Verify claimed file organization patterns
- Check for missing or incorrectly described directories
- **Red Flags**: Non-existent directories, incorrect structure descriptions

#### 4.2 File Naming Convention Verification
**Check**: Are naming conventions accurately documented?
- Verify actual file naming patterns used in the project
- Check for claimed file types that don't exist
- Validate naming convention examples against real files
- **Red Flags**: Documenting conventions not followed, non-existent file types

## Validation Execution Instructions

### Step 1: Systematic Code Analysis
1. **Read the generated documentation completely**
2. **Create a checklist of all factual claims made**
3. **For each claim, verify against the actual codebase**
4. **Document discrepancies with evidence**
5. **Specific Enhanced Validation**:
   - **API Claims**: Verify every documented endpoint exists in service files
   - **State Management Claims**: Verify every documented store, action, and selector exists
   - **Configuration Claims**: Verify every documented config file and environment variable exists and is used
   - **Authentication Claims**: Verify documented auth flow matches actual implementation
   - **Data Flow Claims**: Verify documented patterns match actual component-store interactions

### Step 2: Evidence Collection
For each inaccuracy found:
```markdown
**Claim**: [Quote from documentation]
**Reality**: [What actually exists in code]
**Evidence**: [File path and relevant code snippet]
**Impact**: [How this affects developer experience]
```

### Step 3: Pattern Identification
Identify recurring types of inaccuracies:
- Generic assumptions vs. actual implementation
- Best practice documentation vs. what's implemented
- Technology claims vs. actual dependencies
- Command documentation vs. actual functionality
- **API Documentation Patterns**:
  - Documenting generic REST patterns vs. actual endpoint implementations
  - Claiming authentication mechanisms not actually used
  - Documenting API endpoints that don't exist in service files
  - Incorrect HTTP method or URL pattern documentation
- **State Management Patterns**:
  - Claiming state management libraries not in dependencies
  - Documenting stores or actions that don't exist
  - Incorrect data flow pattern descriptions
  - Wrong state structure or type definitions
- **Configuration Documentation Patterns**:
  - Documenting configuration files that don't exist
  - Claiming environment variables not read by the application
  - Incorrect required vs optional configuration classification
  - Missing configuration validation or error handling documentation

### Step 4: Correction Generation & Composite Context Application

#### 4.1 Project-Specific Learning Creation
Create `docs/.temp/workflow-learnings.md` with:
- **Technology-Specific Validation Rules**: Rules discovered for this project type
- **Common Inaccuracy Patterns**: Patterns specific to this technology stack
- **Validation Insights**: Technology-specific validation approaches that worked
- **Execution Metrics**: Accuracy rates, common issues, successful patterns

#### 4.2 Documentation Corrections Using Composite Context
For each inaccuracy, provide corrected documentation using:
- **Generic Principles**: From the documentation generation prompt
- **Project-Specific Learnings**: From the validation insights just created
- **Evidence-Based Corrections**: Supported by actual code evidence

#### 4.3 Final Documentation Update
Generate corrected `docs/PROJECT_DOCUMENTATION.md` that:
- **Incorporates All Corrections**: Based on validation findings
- **Maintains Generic Structure**: Follows the original documentation format
- **Includes Project-Specific Accuracy**: Uses composite context for precision
- **Preserves Reusability**: Keeps technology-agnostic approach where possible

#### 4.4 Learning File Enhancement
Update `docs/.temp/workflow-learnings.md` with:
- **Execution Results**: Final accuracy metrics and success patterns
- **Technology-Specific Insights**: Validation rules for this project type
- **Future Validation Guidance**: Patterns to watch for in similar projects

## Output Format

### Validation Report
```markdown
# Documentation Validation Report

## Summary
- Total claims verified: [number]
- Inaccuracies found: [number]
- Severity: [Critical/Major/Minor]
- Final accuracy achieved: [percentage]

## Critical Issues
[List issues that would prevent successful project setup]

## Major Issues  
[List issues that would cause confusion or incorrect understanding]

## Minor Issues
[List cosmetic or clarity issues]

## Pattern Analysis
[Common types of inaccuracies and their root causes]

## Corrections Applied
[Summary of corrections made using composite context]
```

### Corrected Documentation
```markdown
# Corrected Documentation

[Complete corrected PROJECT_DOCUMENTATION.md with all inaccuracies fixed using composite context]
```

### Project-Specific Learning File
```markdown
# Project-Specific Workflow Learnings

## Technology Stack
[Technology-specific validation rules for this project type]

## Validation Insights
[What worked well for validating this type of project]

## Common Inaccuracy Patterns
[Patterns specific to this technology stack]

## Execution Metrics
[Accuracy rates, validation success patterns, timing]

## Future Validation Guidance
[Recommendations for similar projects]
```

### Generation Prompt Improvements
```markdown
# Recommended Prompt Improvements

## Additional Validation Steps
[Specific checks to add to prevent found issues]

## Enhanced Warning Language
[Stronger language about verification requirements]

## New Validation Rules
[Specific rules to prevent identified patterns]
```

## Quality Assurance Checklist

Before completing validation:
- [ ] Every factual claim in documentation has been verified against code
- [ ] Package manager usage is consistent throughout
- [ ] No testing documentation exists without actual test files
- [ ] Routing strategy matches actual implementation
- [ ] Environment configuration claims match reality
- [ ] Configuration management documentation is complete and accurate
  - [ ] All documented configuration files exist
  - [ ] Required vs optional configuration classification is correct
  - [ ] Environment variables are actually read by the application
  - [ ] Configuration validation and error handling claims are accurate
- [ ] API integration documentation is comprehensive and accurate
  - [ ] API endpoints inventory matches actual service implementations
  - [ ] Service architecture descriptions match actual file organization
  - [ ] Authentication flow documentation matches implementation
  - [ ] API integration patterns match actual async operation handling
- [ ] State management documentation is detailed and accurate
  - [ ] State management library claims match actual dependencies
  - [ ] Store documentation matches actual store implementations
  - [ ] Data flow patterns match actual component-store interactions
  - [ ] Data models and type definitions exist as documented
  - [ ] Persistence mechanisms match actual implementation
- [ ] All commands have been verified as functional
- [ ] Technology stack claims match package.json
- [ ] File structure documentation matches actual project
- [ ] No contradictory statements exist
- [ ] All corrections include supporting evidence

## Validation Execution

Execute this integrated validation and correction process thoroughly:

1. **Validate** every factual claim in the documentation against the source code
2. **Create** project-specific learning file with technology-specific insights
3. **Apply corrections** using composite context (generic + project-specific learnings)
4. **Generate** corrected documentation with validated accuracy
5. **Enhance** learning file with execution results and patterns

Focus on accuracy that impacts developer productivity and onboarding success while building reusable technology-specific validation insights for future projects. 
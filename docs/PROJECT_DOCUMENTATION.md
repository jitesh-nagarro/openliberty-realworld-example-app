# Open Liberty RealWorld Example App - Project Documentation

## 1. Project Overview

**Purpose**: A backend REST API implementation of the RealWorld specification using Open Liberty and Jakarta EE technologies, providing a conduit-style blogging platform with user authentication, article management, and social features.

**Domain**: Social blogging platform that enables users to create, read, update, and delete articles, follow other users, and favorite articles.

**Target Users**: Frontend developers building RealWorld frontend implementations and backend developers learning Open Liberty/Jakarta EE patterns.

**Core Value Proposition**: Demonstrates production-ready Java enterprise patterns and Open Liberty microservices architecture for building scalable REST APIs with JWT authentication, JPA persistence, and CORS support.

## 2. Functional Documentation

### 2.1 Feature Inventory

**User Management Features**:
- User Registration - Anonymous users can create accounts with email, username, and password
- User Authentication - Registered users can login with email/password to receive JWT tokens
- User Profile Management - Authenticated users can view and update their profile information
- User Profile Viewing - Users can view other users' public profiles

**Article Management Features**:
- Article Creation - Authenticated users can create articles with title, description, body, and tags
- Article Listing - All users can browse articles with filtering by tag, author, and favorited status
- Article Viewing - All users can read individual articles and view article details
- Article Editing - Authors can update their own articles
- Article Deletion - Authors can delete their own articles
- Article Feed - Authenticated users can view articles from followed authors

**Social Features**:
- Article Favoriting - Authenticated users can favorite/unfavorite articles
- User Following - Authenticated users can follow/unfollow other users
- Article Comments - Authenticated users can add, view, and delete comments on articles

**Tag System**:
- Tag Listing - All users can retrieve list of all article tags
- Tag Filtering - Articles can be filtered by tags

### 2.2 User Journey Maps

**Authentication Flow**:
1. User submits registration with email, username, password → System validates and creates account → JWT token returned
2. User submits login with email/password → System validates credentials → JWT token returned
3. User includes JWT token in Authorization header for protected endpoints

**Article Creation Flow**:
1. Authenticated user submits article data → System validates user token → Article created with auto-generated slug → Article returned with metadata
2. System automatically associates article with authenticated user as author

**Social Interaction Flow**:
1. User follows another user → System creates follower relationship → Updated profile returned
2. User favorites article → System creates favorite relationship → Updated article with favorite status returned
3. User views personalized feed → System queries articles from followed users → Filtered article list returned

### 2.3 Business Logic Rules

**Data Validation Rules**:
- Email addresses must be unique across all users
- Usernames must be unique across all users
- Article slugs are auto-generated from titles and must be unique
- Required fields: user email/username/password, article title/description/body
- Comment body is required for comment creation

**Business Constraints**:
- Users can only edit/delete their own articles and comments
- Article favoriting requires authentication
- User following requires authentication
- Feed access requires authentication
- Public endpoints: article listing, article viewing, tag listing, user profile viewing

**Workflow Dependencies**:
- Article operations require valid JWT authentication (except listing/viewing)
- Comment operations require both article existence and user authentication
- User profile updates require matching authenticated user ID

**Integration Points**:
- CORS configured for frontend applications running on localhost:4100
- JWT token validation integrated with all protected endpoints
- Database transactions managed automatically via JTA

## 3. Technical Documentation

### 3.1 Architecture Overview

**Technology Stack**:
- Java 8 runtime environment
- Open Liberty 20.0.0.4 application server
- MicroProfile 3.0 specifications
- Jakarta EE features (JAX-RS, JPA, CDI, JWT)
- Apache Derby embedded database
- Maven 2.7 build system
- JUnit 4.12 for testing

**Project Structure**:
```
src/main/java/
├── application/
│   ├── rest/           # JAX-RS REST endpoint controllers
│   └── errors/         # Validation error message handling
├── core/               # Domain entities and business objects
│   ├── article/        # Article domain models
│   ├── comments/       # Comment domain models
│   └── user/           # User domain models
├── dao/                # Data Access Objects for database operations
└── security/          # JWT token generation utilities

src/main/liberty/config/  # Open Liberty server configuration
src/main/resources/       # JPA persistence configuration
src/test/java/it/         # Integration tests
```

**Design Patterns**:
- **REST API Architecture**: JAX-RS resources following RESTful conventions
- **Layered Architecture**: Clear separation between REST layer, business logic, and data access
- **Dependency Injection**: CDI-based injection throughout application layers
- **JPA Entity Inheritance**: AbstractUser base class extended by User and Profile entities
- **Builder Pattern**: Used in CreateUser and CreateArticle DTOs for request handling

**Data Flow**:
1. HTTP Request → JAX-RS Resource → DAO Layer → JPA Entity → Database
2. Database Response → JPA Entity → DAO Layer → JSON Response → HTTP Response
3. JWT tokens validated at resource level before business logic execution

### 3.2 Development Environment

**Prerequisites**:
- Java 8 JDK or higher
- Maven 3.x build tool
- Git for source code management

**Installation Steps**:
```bash
# Clone the repository
git clone [repository-url]
cd openliberty-realworld-example-app

# Build the application
mvn clean install

# Start the server (background process)
mvn liberty:start

# Start the server (foreground process)
mvn liberty:run
```

**Configuration**:
- Server runs on HTTP port 9080 and HTTPS port 9443 by default
- Derby database automatically created in target/liberty/wlp/usr/shared/resources/
- Application context root: `/` (root path)
- API endpoints available under `/api` path

**Common Commands**:
```bash
# Build application
mvn clean compile

# Run unit tests
mvn test

# Run integration tests
mvn verify

# Package WAR file
mvn package

# Start Liberty server (background)
mvn liberty:start

# Stop Liberty server
mvn liberty:stop

# Run server in development mode with hot reload (recommended for development)
mvn liberty:dev

# Run server in foreground
mvn liberty:run
```

### 3.3 Application Configuration

**Required Configuration**: The application works out-of-the-box with embedded Derby database and default ports.

**Database Configuration** (server.xml):
- **Connection**: Embedded Derby database with auto-creation
- **JNDI Name**: `jdbc/RealWorldSource`
- **Database Name**: `UserDB`
- **Auto-Creation**: Database and tables created automatically on first startup

**Security Configuration**:
- **JWT Issuer**: `https://192.168.1.15:9443/jwt/defaultJWT` (development configuration - update for production)
- **JWT Scheme**: `Token` (Authorization header format: `Token jwt-token-here`)
- **CORS Origins**: `http://localhost:4100` (configurable for frontend integration)
- **Protected Role**: `users` (all authenticated endpoints require this role)

**Server Configuration** (server.xml):
```xml
<httpEndpoint httpPort="9080" httpsPort="9443" host="*"/>
<application context-root="/" location="realworld-liberty.war"/>
<cors domain="/api" allowedOrigins="http://localhost:4100" allowCredentials="true"/>
<mpJwt issuer="https://192.168.1.15:9443/jwt/defaultJWT" authorizationHeaderScheme="Token"/>
```

**Optional Configuration**:
- **Port Customization**: Modify `testServerHttpPort` and `testServerHttpsPort` properties in pom.xml
  - HTTP Port: Default 9080 (configurable via Maven property)
  - HTTPS Port: Default 9443 (configurable via Maven property)
- **Database Location**: Configurable via Derby properties in server.xml
- **CORS Origins**: Update `allowedOrigins` in server.xml for different frontend URLs
- **JWT Issuer**: Modify `mpJwt` configuration for different token issuers (required for production)

**Environment Variables**: No environment variables required for basic operation. Maven properties can be overridden via system properties or environment variables if needed:
- `testServerHttpPort`: HTTP port (default 9080)
- `testServerHttpsPort`: HTTPS port (default 9443)

**Configuration Validation**:
- Application validates JWT tokens on startup and for each protected request
- Database connection tested during server startup
- Invalid JWT configuration results in 401 Unauthorized responses

### 3.4 Code Organization

**Directory Structure**:
- `application/rest/`: JAX-RS resource classes handling HTTP requests and responses
- `core/`: Domain entities representing business objects (User, Article, Comment)
- `dao/`: Data Access Objects providing database operations and business logic
- `security/`: JWT token generation and security utilities
- `application/errors/`: Error handling and validation message utilities

**File Naming Conventions**:
- `*API.java`: JAX-RS resource classes (UsersAPI, ArticlesAPI, ProfilesAPI)
- `*.java`: Domain entities (User.java, Article.java, Comment.java)
- `*Dao.java`: Data access objects (UserDao.java, ArticleDao.java)
- `Create*.java`: DTO classes for request payloads (CreateUser.java, CreateArticle.java)
- `*IT.java`: Integration test classes

**Import/Export Patterns**:
- CDI `@Inject` annotation for dependency injection
- JAX-RS annotations for REST endpoint definitions
- JPA annotations for entity mapping
- Standard Java package-based imports

**State Management**:
- Request-scoped CDI beans for resource classes
- JPA entities managed by persistence context
- JWT claims injected via MicroProfile JWT for user context
- Transaction boundaries managed via `@Transactional` annotations

### 3.5 API Documentation

**Authentication**: JWT token required for protected endpoints, sent via `Authorization: Token <jwt-token>` header.

**Base URL**: `http://localhost:9080/api`

**User Management Endpoints**:

**POST /api/users** (Register)
- Purpose: Create new user account
- Authentication: None required
- Request Body:
```json
{
  "user": {
    "username": "string",
    "email": "string",
    "password": "string"
  }
}
```
- Response: User object with JWT token
- Status Codes: 200 (success), 422 (validation error)

**POST /api/users/login** (Login)
- Purpose: Authenticate existing user
- Authentication: None required
- Request Body:
```json
{
  "user": {
    "email": "string",
    "password": "string"
  }
}
```
- Response: User object with JWT token
- Status Codes: 200 (success), 422 (invalid credentials), 404 (user not found)

**GET /api/user** (Current User)
- Purpose: Get current authenticated user details
- Authentication: JWT token required
- Response: User object with current details
- Status Codes: 200 (success), 404 (user not found), 401 (unauthorized)

**PUT /api/user** (Update User)
- Purpose: Update current user profile
- Authentication: JWT token required
- Request Body: User object with fields to update
- Response: Updated user object
- Status Codes: 200 (success), 404 (user not found), 401 (unauthorized)

**Article Management Endpoints**:

**GET /api/articles** (List Articles)
- Purpose: List articles with optional filtering
- Authentication: None required (JWT optional for favorite status)
- Query Parameters: `tag`, `author`, `favorited`, `limit` (default 20), `offset` (default 0)
- Response: Articles array with metadata
- Status Codes: 200 (success)

**GET /api/articles/feed** (User Feed)
- Purpose: Get articles from followed authors
- Authentication: JWT token required
- Query Parameters: `limit` (default 20), `offset` (default 0)
- Response: Articles array from followed users
- Status Codes: 200 (success), 401 (unauthorized)

**GET /api/articles/{slug}** (Get Article)
- Purpose: Retrieve specific article by slug
- Authentication: None required (JWT optional for favorite status)
- Response: Single article object
- Status Codes: 200 (success), 404 (article not found)

**POST /api/articles** (Create Article)
- Purpose: Create new article
- Authentication: JWT token required
- Request Body:
```json
{
  "article": {
    "title": "string",
    "description": "string",
    "body": "string",
    "tagList": ["string"]
  }
}
```
- Response: Created article object
- Status Codes: 201 (created), 422 (validation error), 404 (user not found), 401 (unauthorized)

**PUT /api/articles/{slug}** (Update Article)
- Purpose: Update existing article (author only)
- Authentication: JWT token required
- Request Body: Article object with fields to update
- Response: Updated article object
- Status Codes: 200 (success), 404 (not found), 401 (unauthorized), 403 (not author)

**DELETE /api/articles/{slug}** (Delete Article)
- Purpose: Delete article (author only)
- Authentication: JWT token required
- Response: No content
- Status Codes: 204 (deleted), 404 (not found), 401 (unauthorized), 403 (not author)

**Social Feature Endpoints**:

**POST /api/articles/{slug}/favorite** (Favorite Article)
- Purpose: Add article to user's favorites
- Authentication: JWT token required
- Response: Article object with updated favorite status
- Status Codes: 200 (success), 404 (article not found), 401 (unauthorized)

**DELETE /api/articles/{slug}/favorite** (Unfavorite Article)
- Purpose: Remove article from user's favorites
- Authentication: JWT token required
- Response: Article object with updated favorite status
- Status Codes: 200 (success), 404 (article not found), 401 (unauthorized)

**GET /api/profiles/{username}** (Get Profile)
- Purpose: Get user profile by username
- Authentication: None required (JWT optional for following status)
- Response: Profile object with following status
- Status Codes: 200 (success), 404 (user not found)

**POST /api/profiles/{username}/follow** (Follow User)
- Purpose: Follow another user
- Authentication: JWT token required
- Response: Profile object with following status
- Status Codes: 200 (success), 404 (user not found), 401 (unauthorized)

**DELETE /api/profiles/{username}/follow** (Unfollow User)
- Purpose: Unfollow another user
- Authentication: JWT token required
- Response: Profile object with following status
- Status Codes: 200 (success), 404 (user not found), 401 (unauthorized)

**Comment Endpoints**:

**GET /api/articles/{slug}/comments** (Get Comments)
- Purpose: Get comments for an article
- Authentication: None required
- Response: Comments array
- Status Codes: 200 (success), 404 (article not found)

**POST /api/articles/{slug}/comments** (Add Comment)
- Purpose: Add comment to article
- Authentication: JWT token required
- Request Body:
```json
{
  "comment": {
    "body": "string"
  }
}
```
- Response: Created comment object
- Status Codes: 201 (created), 404 (article or user not found), 401 (unauthorized), 400 (validation error)

**DELETE /api/articles/{slug}/comments/{id}** (Delete Comment)
- Purpose: Delete comment (author only)
- Authentication: JWT token required
- Response: No content
- Status Codes: 204 (deleted), 404 (not found), 401 (unauthorized), 403 (not author)

**Tag Endpoints**:

**GET /api/tags** (List Tags)
- Purpose: Get list of all article tags
- Authentication: None required
- Response: Tags array
- Status Codes: 200 (success)

**Health Check**:

**GET /health** (Health Check)
- Purpose: Application health status
- Authentication: None required
- Response: Health status object
- Status Codes: 200 (healthy)

**Error Handling**:
- **422 Unprocessable Entity**: Validation errors with detailed error messages
- **401 Unauthorized**: Missing or invalid JWT token
- **403 Forbidden**: Valid token but insufficient permissions
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server-side errors

**Rate Limiting**: No rate limiting implemented.

**Versioning**: No versioning implemented - single API version.

### 3.6 Data Management

**Database Schema**:
- **USER_TABLE**: Stores user accounts with email, username, password, bio, image
- **Article**: Stores articles with title, description, body, slug, timestamps
- **Comment**: Stores comments linked to articles and users
- **Tag relationships**: Many-to-many between articles and tags
- **Follow relationships**: Many-to-many self-referencing users table
- **Favorite relationships**: Many-to-many between users and articles

**Migration Strategy**:
- **Auto-Creation**: EclipseLink automatically creates tables on first startup
- **DDL Generation**: Set to "create-tables" mode in persistence.xml
- **Schema Updates**: Manual schema changes require updating JPA entity annotations

**Data Access Patterns**:
- **JPA Repository Pattern**: DAO classes encapsulate all database operations
- **Connection Pooling**: Managed by Open Liberty runtime
- **Transaction Management**: JTA transactions with `@Transactional` annotations
- **Query Optimization**: Named queries and JPQL for complex operations

**Caching Strategy**: No explicit caching implemented - relies on JPA L1 cache.

**Data Validation**:
- **Entity Validation**: JPA constraints (unique, nullable, length)
- **Business Validation**: Custom validation in DAO and API layers
- **Input Sanitization**: JAX-RS automatic JSON binding with validation

## 4. Development Guidelines

### 4.1 Code Standards

**Coding Style**:
- 4-space indentation for Java code
- Camel case for variables and methods (`createUser`, `findByEmail`)
- Pascal case for class names (`UsersAPI`, `ArticleDao`)
- ALL_CAPS for constants and column names
- Double quotes for string literals

**CDI and Dependency Injection**:
- Use `@Inject` for dependency injection
- `@RequestScoped` for REST resource classes
- Avoid field injection, prefer constructor injection where possible

**JAX-RS Conventions**:
- Use appropriate HTTP methods (GET, POST, PUT, DELETE)
- Include `@Consumes(MediaType.APPLICATION_JSON)` for request bodies
- Include `@Produces(MediaType.APPLICATION_JSON)` for JSON responses
- Use `@PermitAll` for public endpoints, `@RolesAllowed("users")` for protected endpoints

**Error Handling**:
- Return appropriate HTTP status codes
- Use ValidationMessages utility for consistent error responses
- Include meaningful error messages in response bodies
- Handle JWT exceptions consistently across endpoints

### 4.2 Testing Strategy

**Integration Tests**: Limited integration testing implemented in `EndpointIT.java`

**Existing Tests**:
- **Health Check Test**: Verifies `/health` endpoint returns UP status
- **Basic Article Listing**: Verifies `/api/articles` returns empty list initially

**Test Coverage**: Currently minimal - only 2 basic integration tests exist

**Test Framework**: JUnit 4.12 with JAX-RS client testing

**Test Database**: Uses same Derby embedded database as development

**Missing Test Areas**:
- Unit tests for business logic
- Comprehensive API endpoint testing
- Authentication and authorization testing
- Database operations testing
- Error handling scenarios

### 4.3 Performance Considerations

**Known Optimizations**:
- Derby embedded database for development simplicity
- JPA lazy loading configured for related entities
- Connection pooling managed by Open Liberty

**Potential Bottlenecks**:
- Derby database not suitable for production load
- No explicit caching layer implemented
- N+1 query issues possible with article/comment relationships

**Monitoring Setup**: Basic health endpoint available, no advanced monitoring configured.

## 5. Deployment & Operations

### 5.1 Build Process

**Maven Build System**:
- `mvn clean compile`: Compiles Java sources
- `mvn package`: Creates WAR file for deployment
- `mvn install`: Installs artifacts to local repository
- Maven Surefire plugin configured for unit tests
- Maven Failsafe plugin configured for integration tests

**Build Artifacts**:
- WAR file: `target/realworld-liberty.war`
- Liberty server package: Generated in `target/liberty/`
- Derby database dependencies copied to `target/liberty/wlp/usr/shared/resources/`

**Dependency Management**:
- MicroProfile 3.0 provided scope (runtime dependency)
- Jersey JAX-RS implementation for JSON processing
- Derby database driver included for embedded database
- Test dependencies (JUnit) excluded from production build

### 5.2 Deployment Process

**Open Liberty Deployment**:
- Application packaged as WAR file
- Deployed to Liberty runtime via Maven plugin
- Server configuration in `src/main/liberty/config/server.xml`
- Context root configured as `/` (application root)

**Database Setup**:
- Derby database auto-created on first application startup
- No manual database setup required
- Database files stored in Liberty shared resources directory

**Environment Configuration**:
- Server ports configurable via Maven properties
- CORS origins configurable in server.xml
- JWT issuer configurable in server.xml

**Production Considerations**:
- **CRITICAL**: Replace Derby with production database (PostgreSQL, MySQL, etc.) - Derby is unsuitable for production use
- Configure external JWT token issuer (current configuration uses development IP address)
- Update CORS configuration for production frontend URLs
- Implement proper logging configuration for production monitoring

### 5.3 Monitoring & Debugging

**Application Logging**:
- Standard Java logging available
- Liberty server logs in `target/liberty/wlp/usr/servers/defaultServer/logs/`
- No structured logging configuration

**Health Monitoring**:
- Health check endpoint: `/health` (implemented via MicroProfile Health)
- Returns JSON status with "UP" indicator using HealthCheck interface
- Accessible without authentication for load balancer health checks
- No detailed health metrics implemented

**Error Tracking**:
- Basic exception handling in REST endpoints
- Validation errors returned as JSON responses
- No external error tracking service integration

**Performance Monitoring**: No APM tools configured - rely on basic Liberty metrics.

## 6. Quick Start Guides

### 6.1 For New Developers

**5-Step Onboarding**:
1. **Environment Setup**: Install Java 8+ and Maven 3.x
2. **Clone and Build**: `git clone [repo] && cd [repo] && mvn clean install`
3. **Start Development Server**: `mvn liberty:dev` (recommended) or `mvn liberty:run` and verify http://localhost:9080/health
4. **Test API**: Use Postman collection in `utility/` folder to test endpoints
5. **First Feature**: Add a new validation rule to user registration in `UsersAPI.java`

**First Feature Implementation** (Add username length validation):
1. Open `src/main/java/application/rest/UsersAPI.java`
2. In `createUser` method, add username length check after null validation
3. Use `ValidationMessages.throwError()` for consistent error response
4. Test with Postman to verify validation works
5. Consider adding similar validation to update user endpoint

**Common Pitfalls**:
- **Database Lock Issues**: Stop server (`mvn liberty:stop`) before rebuilding if using background mode
- **Port Conflicts**: Ensure ports 9080/9443 are available before starting
- **JWT Token Format**: Use `Token` scheme, not `Bearer` in Authorization header
- **CORS Issues**: Frontend must run on localhost:4100 or update server.xml configuration
- **Development Mode**: Use `mvn liberty:dev` for hot reload during development

### 6.2 For QA Engineers

**Test Data Creation**:
- Register test users via POST `/api/users` endpoint
- Create test articles via POST `/api/articles` (requires authentication)
- Use provided Postman collection for consistent test data setup

**Testing Focus Areas**:
- **Authentication Flow**: Registration, login, token validation
- **Authorization Rules**: Verify users can only edit their own content
- **Data Validation**: Test required fields, uniqueness constraints, email formats
- **CRUD Operations**: Create, read, update, delete for all entities
- **Social Features**: Following, favoriting, commenting workflows
- **Error Scenarios**: Invalid tokens, missing data, duplicate entries

**Bug Reporting**: Include request/response details, JWT token (if applicable), and server logs from Liberty console output.

## 7. Troubleshooting

### Common Issues

**"Port 9080 already in use"**
- **Solution**: Stop existing Liberty server with `mvn liberty:stop` or change port in pom.xml

**"Database locked" errors**
- **Solution**: Ensure server is stopped before rebuilding. Delete `target/liberty/` folder if needed

**"JWT token invalid" (401 responses)**
- **Solution**: Verify token format is `Authorization: Token <jwt-token>`, not `Bearer`
- **Check**: Token may have expired, re-login to get fresh token

**"User not found" errors**
- **Solution**: Verify JWT token contains valid user ID claim
- **Debug**: Check server logs for JWT parsing errors

### Environment Issues

**Maven build failures**
- **Check**: Java 8+ is installed and JAVA_HOME is set correctly
- **Solution**: Run `mvn clean` before `mvn install` to clear build artifacts

**Server startup failures**
- **Check**: Verify `src/main/liberty/config/server.xml` exists and is valid XML
- **Solution**: Review server logs in `target/liberty/wlp/usr/servers/defaultServer/logs/`

### Runtime Errors

**500 Internal Server Error responses**
- **Debug**: Check Liberty server console output for stack traces
- **Common Cause**: Database connection issues or JPA mapping errors

**422 Validation Error responses**
- **Expected**: These are business validation errors (duplicate email, missing fields)
- **Debug**: Response body contains specific validation error messages

### Performance Issues

**Slow response times**
- **Check**: Database performance with embedded Derby (consider external database)
- **Monitor**: Network latency if testing from remote clients
- **Debug**: Enable JPA SQL logging to identify slow queries

**Memory issues**
- **Solution**: Increase JVM heap size in Maven plugin configuration
- **Monitor**: Liberty server memory usage in production environments

---

*This documentation covers the Open Liberty RealWorld Example App implementation. For frontend integration, refer to the [RealWorld frontend specifications](https://github.com/gothinkster/realworld) and use the provided API endpoints.*

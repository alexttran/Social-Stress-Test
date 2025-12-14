# System Architecture

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                          Client Layer                            │
│  ┌────────────────────────────────────────────────────────┐     │
│  │  React + TypeScript Frontend (Port 5173)               │     │
│  │  - Auth0 React SDK                                     │     │
│  │  - React Router                                        │     │
│  │  - Axios HTTP Client                                   │     │
│  └────────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTPS + JWT Bearer Token
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Application Layer                           │
│  ┌────────────────────────────────────────────────────────┐     │
│  │  Spring Boot Backend (Port 8080)                       │     │
│  │  ┌──────────────────────────────────────────────┐     │     │
│  │  │  Controllers (REST API)                      │     │     │
│  │  └──────────────────────────────────────────────┘     │     │
│  │  ┌──────────────────────────────────────────────┐     │     │
│  │  │  Spring Security (OAuth2 Resource Server)    │     │     │
│  │  │  - JWT Validation                            │     │     │
│  │  │  - Route Protection                          │     │     │
│  │  └──────────────────────────────────────────────┘     │     │
│  │  ┌──────────────────────────────────────────────┐     │     │
│  │  │  Business Services                           │     │     │
│  │  │  - IssueService                              │     │     │
│  │  │  - SolutionService                           │     │     │
│  │  │  - EvaluationService                         │     │     │
│  │  │  - UserHistoryService                        │     │     │
│  │  └──────────────────────────────────────────────┘     │     │
│  └────────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────┘
           │                    │                    │
           │                    │                    │
           ▼                    ▼                    ▼
┌─────────────────┐  ┌──────────────────┐  ┌─────────────────┐
│   Auth0         │  │   OpenAI API     │  │   AWS S3        │
│                 │  │                  │  │                 │
│ - JWT Issuer    │  │ - GPT-4/3.5      │  │ - Solutions     │
│ - JWKS Endpoint │  │ - Evaluations    │  │ - Evaluations   │
│ - User Mgmt     │  │ - JSON Mode      │  │ - History       │
└─────────────────┘  └──────────────────┘  └─────────────────┘
```

## Component Architecture

### Frontend (React + TypeScript)

```
src/
├── components/
│   ├── layout/
│   │   ├── Header.tsx              # Top navigation with auth
│   │   ├── Footer.tsx
│   │   └── Layout.tsx              # Main layout wrapper
│   ├── auth/
│   │   ├── LoginButton.tsx
│   │   ├── LogoutButton.tsx
│   │   └── ProtectedRoute.tsx     # Route guard component
│   ├── issues/
│   │   ├── IssueCard.tsx           # Issue preview card
│   │   ├── IssueList.tsx           # List of all issues
│   │   ├── IssueDetail.tsx         # Full issue breakdown
│   │   └── IssueSection.tsx        # Reusable section component
│   ├── solutions/
│   │   ├── SolutionForm.tsx        # Solution submission form
│   │   ├── SolutionCard.tsx        # Solution preview
│   │   ├── SolutionList.tsx        # User's solutions list
│   │   └── SolutionDetail.tsx      # Full solution view
│   ├── evaluations/
│   │   ├── EvaluationPanel.tsx     # Full evaluation display
│   │   ├── FeasibilitySection.tsx  # Feasibility breakdown
│   │   ├── ImpactSection.tsx       # Impact analysis
│   │   ├── EquitySection.tsx       # Equity considerations
│   │   └── ImprovementList.tsx     # Suggested improvements
│   └── common/
│       ├── LoadingSpinner.tsx
│       ├── ErrorMessage.tsx
│       └── Button.tsx
│
├── pages/
│   ├── Home.tsx                    # Landing page
│   ├── IssuesPage.tsx              # Browse issues
│   ├── IssueDetailPage.tsx         # Issue detail view
│   ├── SubmitSolutionPage.tsx      # Solution submission
│   ├── MySolutionsPage.tsx         # User's solutions
│   ├── EvaluationPage.tsx          # Evaluation results
│   ├── HistoryPage.tsx             # User activity history
│   └── NotFound.tsx
│
├── services/
│   ├── api.ts                      # Axios instance config
│   ├── issueService.ts             # Issue API calls
│   ├── solutionService.ts          # Solution API calls
│   ├── evaluationService.ts        # Evaluation API calls
│   └── historyService.ts           # History API calls
│
├── hooks/
│   ├── useAuth.ts                  # Auth0 wrapper hook
│   ├── useIssues.ts                # Issues data fetching
│   ├── useSolutions.ts             # Solutions data fetching
│   └── useEvaluation.ts            # Evaluation request logic
│
├── types/
│   ├── issue.ts                    # Issue interfaces
│   ├── solution.ts                 # Solution interfaces
│   ├── evaluation.ts               # Evaluation interfaces
│   ├── user.ts                     # User interfaces
│   └── api.ts                      # API response types
│
├── context/
│   └── AppContext.tsx              # Global app state (optional)
│
├── utils/
│   ├── formatters.ts               # Date, text formatting
│   ├── validators.ts               # Form validation helpers
│   └── constants.ts                # App constants
│
├── App.tsx                         # Main app component
├── main.tsx                        # Entry point
└── router.tsx                      # Route configuration
```

### Backend (Spring Boot + Java)

```
src/main/java/com/socialstresstest/
├── SocialStressTestApplication.java    # Main application class
│
├── controller/
│   ├── IssueController.java            # GET /api/issues/*
│   ├── SolutionController.java         # POST/GET /api/solutions/*
│   ├── EvaluationController.java       # POST/GET /api/evaluations/*
│   └── UserController.java             # GET /api/users/me/*
│
├── service/
│   ├── IssueService.java               # Issue business logic
│   ├── SolutionService.java            # Solution management
│   ├── EvaluationService.java          # Evaluation orchestration
│   ├── OpenAIService.java              # OpenAI API integration
│   ├── S3Service.java                  # AWS S3 operations
│   └── UserHistoryService.java         # User history aggregation
│
├── model/
│   ├── domain/
│   │   ├── Issue.java                  # Issue entity
│   │   ├── Solution.java               # Solution entity
│   │   ├── Evaluation.java             # Evaluation entity
│   │   └── UserHistory.java            # User history entity
│   ├── dto/
│   │   ├── request/
│   │   │   ├── SolutionRequest.java
│   │   │   └── EvaluationRequest.java
│   │   └── response/
│   │       ├── IssueResponse.java
│   │       ├── SolutionResponse.java
│   │       ├── EvaluationResponse.java
│   │       └── ErrorResponse.java
│   └── openai/
│       ├── OpenAIRequest.java
│       └── OpenAIResponse.java
│
├── config/
│   ├── SecurityConfig.java             # Spring Security setup
│   ├── CorsConfig.java                 # CORS configuration
│   ├── AwsConfig.java                  # AWS S3 client config
│   └── OpenAIConfig.java               # OpenAI client config
│
├── security/
│   ├── JwtAuthenticationFilter.java    # JWT extraction/validation
│   └── SecurityUtils.java              # Security helper methods
│
├── integration/
│   ├── openai/
│   │   ├── OpenAIClient.java           # OpenAI REST client
│   │   └── PromptBuilder.java          # Prompt construction
│   └── aws/
│       └── S3Client.java               # S3 operations wrapper
│
└── exception/
    ├── GlobalExceptionHandler.java     # @ControllerAdvice
    ├── ResourceNotFoundException.java
    ├── UnauthorizedException.java
    └── ExternalServiceException.java

src/main/resources/
├── application.yml                     # Main configuration
├── application-dev.yml                 # Dev environment config
├── application-prod.yml                # Production config
└── data/
    └── issues.json                     # Sample issues data
```

## Data Flow Diagrams

### Authentication Flow

```
User                 Frontend              Auth0              Backend
 │                      │                    │                   │
 │──Click Login────────>│                    │                   │
 │                      │──Redirect──────────>│                   │
 │<─────Auth0 Login Page─────────────────────│                   │
 │                      │                    │                   │
 │──Enter Credentials──>│                    │                   │
 │                      │                    │                   │
 │<─────JWT Token───────────────────────────│                   │
 │                      │                    │                   │
 │                      │──Store Token───────│                   │
 │                      │                    │                   │
 │──API Request─────────>│                    │                   │
 │                      │──Request + JWT─────────────────────────>│
 │                      │                    │<──Validate JWT────│
 │                      │                    │──JWKS Response────>│
 │                      │<──Response─────────────────────────────│
 │<─────Data────────────│                    │                   │
```

### Solution Evaluation Flow

```
User          Frontend       Backend         S3          OpenAI
 │                │              │            │             │
 │──Submit────────>│              │            │             │
 │   Solution     │──POST────────>│            │             │
 │                │  /solutions  │            │             │
 │                │              │──Save──────>│             │
 │                │              │  Solution  │             │
 │                │<─Solution ID─│            │             │
 │                │              │            │             │
 │──Request───────>│              │            │             │
 │   Evaluation   │──POST────────>│            │             │
 │                │  /evaluations│            │             │
 │                │              │──Build─────│             │
 │                │              │  Prompt    │             │
 │                │              │            │             │
 │                │              │──API Call──────────────>│
 │                │              │            │  (Evaluate) │
 │                │              │<─Evaluation─────────────│
 │                │              │  JSON      │             │
 │                │              │            │             │
 │                │              │──Save──────>│             │
 │                │              │  Evaluation│             │
 │                │<─Evaluation──│            │             │
 │<───Display─────│  Response    │            │             │
```

### User History Retrieval Flow

```
User          Frontend       Backend            S3
 │                │              │               │
 │──View──────────>│              │               │
 │   History      │──GET─────────>│               │
 │                │  /users/me/  │               │
 │                │  history     │               │
 │                │              │──List─────────>│
 │                │              │  Objects      │
 │                │              │  (user_sub/*) │
 │                │              │               │
 │                │              │<─Object List──│
 │                │              │               │
 │                │              │──Get Objects──>│
 │                │              │               │
 │                │              │<─JSON Data────│
 │                │              │               │
 │                │              │──Aggregate────│
 │                │              │  & Sort       │
 │                │              │               │
 │                │<─History─────│               │
 │<───Display─────│  Timeline    │               │
```

## Security Architecture

### JWT Validation Flow

```
1. Frontend obtains JWT from Auth0
2. Frontend includes JWT in Authorization header: "Bearer {token}"
3. Spring Security intercepts request
4. JwtDecoder validates:
   - Signature (using Auth0 JWKS public keys)
   - Issuer (matches Auth0 domain)
   - Audience (matches API identifier)
   - Expiration (not expired)
5. If valid, extract user claims (sub, email, etc.)
6. Set SecurityContext with authenticated user
7. Controller method executes with @AuthenticationPrincipal
```

### Authorization Rules

| Endpoint | Auth Required | Additional Check |
|----------|---------------|------------------|
| GET /api/issues | No | - |
| GET /api/issues/{id} | No | - |
| POST /api/solutions | Yes | - |
| GET /api/solutions/{id} | Yes | Owner or public |
| GET /api/users/me/solutions | Yes | Current user only |
| POST /api/evaluations | Yes | Solution owner only |
| GET /api/users/me/history | Yes | Current user only |

### Data Isolation

- All S3 objects prefixed with user's Auth0 `sub` claim
- Controllers extract `sub` from JWT, never from request body
- Service layer filters all queries by authenticated user ID
- No cross-user data access permitted

## Deployment Architecture (Future)

```
┌─────────────────────────────────────────────────────────┐
│                      AWS Cloud                           │
│                                                          │
│  ┌────────────────┐         ┌──────────────────┐       │
│  │  CloudFront    │────────>│  S3 Static       │       │
│  │  (CDN)         │         │  Website Hosting │       │
│  │                │         │  (React Build)   │       │
│  └────────────────┘         └──────────────────┘       │
│         │                                                │
│         │                                                │
│  ┌────────────────┐         ┌──────────────────┐       │
│  │  Application   │────────>│  S3 Bucket       │       │
│  │  Load Balancer │         │  (User Data)     │       │
│  └────────────────┘         └──────────────────┘       │
│         │                                                │
│         ▼                                                │
│  ┌────────────────┐                                     │
│  │  ECS/EC2       │                                     │
│  │  Spring Boot   │─────────> OpenAI API               │
│  │  Instances     │           (External)                │
│  │                │                                     │
│  │  - Auto Scaling│─────────> Auth0                    │
│  │  - Health Check│           (External)                │
│  └────────────────┘                                     │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

## Technology Decisions & Rationale

### Why React + TypeScript?
- **Type Safety**: Catch errors at compile time
- **Developer Experience**: Excellent tooling, autocomplete
- **Maintainability**: Self-documenting code
- **Industry Standard**: Large ecosystem, easy to hire

### Why Spring Boot?
- **Mature Security**: Spring Security is battle-tested
- **OAuth2 Support**: First-class JWT validation
- **Dependency Injection**: Clean, testable architecture
- **AWS Integration**: Official SDK support
- **Java Ecosystem**: Robust libraries for enterprise features

### Why Auth0?
- **Security**: Don't build auth from scratch
- **Compliance**: GDPR, SOC2, etc. handled
- **Features**: MFA, social login, passwordless ready
- **JWT Standards**: Standard OAuth2/OIDC implementation

### Why S3 for Storage?
- **Cost**: Very cheap for append-only data
- **Scalability**: Handles unlimited objects
- **Durability**: 99.999999999% durability
- **Simplicity**: No schema management for MVP
- **Future-proof**: Easy to migrate to DB later

### Why OpenAI API?
- **Best-in-class reasoning**: GPT-4 excels at analysis
- **Structured outputs**: JSON mode for reliable parsing
- **Flexible prompts**: Easy to iterate on evaluation criteria
- **Cost-effective**: Pay per use, no model hosting

## API Versioning Strategy

Current: `/api/solutions`
Future: `/api/v1/solutions`, `/api/v2/solutions`

- Version API when breaking changes needed
- Maintain old versions for 6 months minimum
- Use content negotiation for minor changes

## Performance Considerations

### Frontend
- Lazy load routes with React.lazy()
- Memoize expensive components
- Debounce form inputs
- Cache API responses (SWR or React Query)
- Optimize bundle size (<200kb initial)

### Backend
- Connection pooling for HTTP clients
- Async processing for OpenAI calls (CompletableFuture)
- Cache issue data (in-memory or Redis)
- Rate limiting per user (bucket4j)
- Gzip compression for responses

### OpenAI
- Use gpt-3.5-turbo for dev/testing (cheaper)
- Use gpt-4 for production evaluations
- Implement timeout (30s max)
- Cache identical requests (rare but possible)

## Monitoring & Observability

### Metrics to Track
- Authentication success/failure rate
- API response times (p50, p95, p99)
- OpenAI API latency and costs
- S3 read/write operations
- Error rates by endpoint
- User engagement (solutions submitted, evaluations requested)

### Logging
- Structured logging (JSON format)
- Log levels: ERROR, WARN, INFO, DEBUG
- Include correlation IDs for request tracing
- Log all authentication events
- Log OpenAI API calls (prompts + responses)
- Never log sensitive data (tokens, PII)

### Tools (Future)
- CloudWatch for AWS metrics
- Sentry for error tracking
- DataDog or New Relic for APM
- Auth0 logs for authentication events

## Disaster Recovery

### Backup Strategy
- S3 versioning enabled
- Cross-region replication for critical buckets
- Regular exports to Glacier (cost optimization)

### Incident Response
1. Monitor error rates
2. Check Auth0 status
3. Check OpenAI status
4. Review CloudWatch logs
5. Rollback deployment if needed

## Cost Estimation (Monthly)

### MVP Phase
- Auth0: Free tier (7,000 active users)
- AWS S3: ~$5 (assuming <100GB, minimal requests)
- OpenAI API: ~$50-200 (depends on usage, GPT-4 expensive)
- AWS EC2/ECS: ~$20-50 (t3.small instance)
- **Total: ~$75-255/month**

### Scaling (1,000 active users)
- Auth0: ~$25
- AWS S3: ~$15
- OpenAI API: ~$500-1,000 (rate limiting critical)
- AWS Compute: ~$100-200 (auto-scaling)
- **Total: ~$640-1,240/month**

---

## Next Steps

1. Set up development environment
2. Initialize projects (frontend & backend)
3. Configure Auth0 tenant
4. Create AWS S3 bucket
5. Obtain OpenAI API key
6. Implement Phase 1 (Foundation Setup)

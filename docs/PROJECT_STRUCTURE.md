# Project Structure

## Complete Directory Tree

```
Social-Stress-Test/
│
├── README.md                           # Project overview and vision
├── CLAUDE.md                           # Implementation guide for AI assistance
├── .gitignore                          # Root gitignore
│
├── docs/                               # Documentation
│   ├── ARCHITECTURE.md                 # System architecture and design
│   ├── GETTING_STARTED.md              # Setup and development guide
│   └── PROJECT_STRUCTURE.md            # This file
│
├── frontend/                           # React + TypeScript application
│   ├── public/                         # Static assets
│   ├── src/
│   │   ├── components/                 # React components
│   │   │   ├── layout/                 # Layout components (Header, Footer, etc.)
│   │   │   ├── auth/                   # Authentication components
│   │   │   ├── issues/                 # Issue-related components
│   │   │   ├── solutions/              # Solution-related components
│   │   │   ├── evaluations/            # Evaluation display components
│   │   │   └── common/                 # Shared UI components
│   │   ├── pages/                      # Page-level components (routes)
│   │   ├── services/                   # API clients and services
│   │   ├── hooks/                      # Custom React hooks
│   │   ├── types/                      # TypeScript type definitions
│   │   ├── context/                    # React context providers
│   │   ├── utils/                      # Utility functions
│   │   ├── App.tsx                     # Main app component
│   │   └── main.tsx                    # Application entry point
│   ├── index.html                      # HTML template
│   ├── package.json                    # NPM dependencies
│   ├── tsconfig.json                   # TypeScript configuration
│   ├── tsconfig.node.json              # TypeScript config for Vite
│   ├── vite.config.ts                  # Vite bundler configuration
│   ├── tailwind.config.js              # Tailwind CSS configuration
│   ├── postcss.config.js               # PostCSS configuration
│   ├── .env.example                    # Example environment variables
│   └── .gitignore                      # Frontend gitignore
│
└── backend/                            # Spring Boot application
    ├── src/
    │   ├── main/
    │   │   ├── java/com/socialstresstest/
    │   │   │   ├── SocialStressTestApplication.java   # Main application class
    │   │   │   ├── controller/                        # REST API controllers
    │   │   │   ├── service/                           # Business logic services
    │   │   │   ├── model/                             # Data models
    │   │   │   │   ├── domain/                        # Domain entities
    │   │   │   │   ├── dto/                           # Data transfer objects
    │   │   │   │   │   ├── request/                   # Request DTOs
    │   │   │   │   │   └── response/                  # Response DTOs
    │   │   │   │   └── openai/                        # OpenAI-specific models
    │   │   │   ├── config/                            # Spring configuration
    │   │   │   ├── security/                          # Security configuration
    │   │   │   ├── integration/                       # External API integrations
    │   │   │   │   ├── openai/                        # OpenAI client
    │   │   │   │   └── aws/                           # AWS S3 client
    │   │   │   └── exception/                         # Exception handlers
    │   │   └── resources/
    │   │       ├── application.yml                    # Main configuration
    │   │       ├── application-dev.yml                # Dev environment config
    │   │       └── data/
    │   │           └── issues.json                    # Sample issues data
    │   └── test/
    │       └── java/com/socialstresstest/            # Test classes
    ├── pom.xml                         # Maven dependencies
    ├── .env.example                    # Example environment variables
    └── .gitignore                      # Backend gitignore
```

## Key Files Explained

### Root Level

| File | Purpose |
|------|---------|
| `README.md` | Project vision, features, architecture overview |
| `CLAUDE.md` | Detailed implementation guide with data models, API specs, phases |
| `.gitignore` | Prevents committing sensitive files (env vars, IDE configs) |

### Documentation (`/docs`)

| File | Purpose |
|------|---------|
| `ARCHITECTURE.md` | Complete system architecture, data flows, component design |
| `GETTING_STARTED.md` | Step-by-step setup instructions for developers |
| `PROJECT_STRUCTURE.md` | This file - explains directory organization |

### Frontend (`/frontend`)

#### Configuration Files

| File | Purpose |
|------|---------|
| `package.json` | NPM dependencies and scripts |
| `tsconfig.json` | TypeScript compiler options |
| `vite.config.ts` | Vite dev server and build configuration |
| `tailwind.config.js` | Tailwind CSS theme and plugins |
| `.env.example` | Template for environment variables |

#### Source Code (`/src`)

| Directory | Purpose |
|-----------|---------|
| `components/layout/` | Header, Footer, Layout wrapper |
| `components/auth/` | Login/Logout buttons, ProtectedRoute |
| `components/issues/` | IssueCard, IssueList, IssueDetail |
| `components/solutions/` | SolutionForm, SolutionCard, SolutionList |
| `components/evaluations/` | EvaluationPanel, sections for each dimension |
| `components/common/` | Reusable UI components (Button, Spinner, etc.) |
| `pages/` | Top-level route components |
| `services/` | API client functions (Axios wrappers) |
| `hooks/` | Custom React hooks (useAuth, useIssues, etc.) |
| `types/` | TypeScript interfaces and types |
| `context/` | React Context providers for global state |
| `utils/` | Helper functions (formatters, validators, constants) |

### Backend (`/backend`)

#### Configuration Files

| File | Purpose |
|------|---------|
| `pom.xml` | Maven dependencies and build configuration |
| `application.yml` | Spring Boot configuration (security, AWS, OpenAI) |
| `application-dev.yml` | Development-specific overrides |
| `.env.example` | Template for environment variables |

#### Source Code (`/src/main/java/com/socialstresstest`)

| Directory | Purpose |
|-----------|---------|
| `controller/` | REST API endpoints (IssueController, SolutionController, etc.) |
| `service/` | Business logic (IssueService, EvaluationService, OpenAIService, S3Service) |
| `model/domain/` | Core domain entities (Issue, Solution, Evaluation) |
| `model/dto/request/` | Request DTOs for API endpoints |
| `model/dto/response/` | Response DTOs for API endpoints |
| `model/openai/` | Models for OpenAI API requests/responses |
| `config/` | Spring configuration classes (Security, CORS, AWS, OpenAI) |
| `security/` | Security filters and utilities |
| `integration/openai/` | OpenAI REST client and prompt builder |
| `integration/aws/` | AWS S3 client wrapper |
| `exception/` | Custom exceptions and global exception handler |

#### Resources (`/src/main/resources`)

| File/Directory | Purpose |
|----------------|---------|
| `application.yml` | Main Spring Boot configuration |
| `application-dev.yml` | Development profile overrides |
| `data/issues.json` | Sample issues for development/testing |

## Development Workflow

### Adding a New Feature

1. **Plan**: Review CLAUDE.md for implementation phases
2. **Backend**:
   - Create domain model in `model/domain/`
   - Create DTOs in `model/dto/`
   - Implement service in `service/`
   - Create controller in `controller/`
   - Write tests in `src/test/`
3. **Frontend**:
   - Define types in `types/`
   - Create API service in `services/`
   - Build components in `components/`
   - Create page in `pages/`
   - Add route to router
4. **Test**: Test end-to-end with both servers running

### Where to Put New Code

| Type of Code | Location |
|--------------|----------|
| New REST endpoint | `backend/controller/` |
| Business logic | `backend/service/` |
| Database/S3 operations | `backend/service/` or `integration/` |
| API request/response shapes | `backend/model/dto/` |
| External API client | `backend/integration/` |
| React page component | `frontend/src/pages/` |
| Reusable UI component | `frontend/src/components/` |
| API call function | `frontend/src/services/` |
| Custom hook | `frontend/src/hooks/` |
| TypeScript types | `frontend/src/types/` |
| Utility function | `frontend/src/utils/` or `backend/util/` |

## File Naming Conventions

### Frontend (TypeScript/React)

- **Components**: PascalCase, e.g., `IssueCard.tsx`
- **Hooks**: camelCase with `use` prefix, e.g., `useAuth.ts`
- **Services**: camelCase with `Service` suffix, e.g., `issueService.ts`
- **Types**: camelCase, e.g., `issue.ts`, `solution.ts`
- **Utils**: camelCase, e.g., `formatters.ts`

### Backend (Java/Spring Boot)

- **Classes**: PascalCase, e.g., `IssueController.java`
- **Interfaces**: PascalCase, often with `I` prefix or descriptive suffix
- **Services**: PascalCase with `Service` suffix, e.g., `IssueService.java`
- **Controllers**: PascalCase with `Controller` suffix
- **DTOs**: PascalCase with `Request`/`Response` suffix
- **Config**: PascalCase with `Config` suffix

## Configuration Management

### Environment Variables

**Never commit these to Git!**

| Environment | File | Purpose |
|-------------|------|---------|
| Frontend | `.env` | Auth0 config, API base URL |
| Backend | `.env` | Auth0, OpenAI, AWS credentials |

**Safe to commit:**

| File | Purpose |
|------|---------|
| `.env.example` | Template showing required variables (no actual secrets) |

### Spring Profiles

| Profile | File | When Used |
|---------|------|-----------|
| Default | `application.yml` | Always loaded |
| Development | `application-dev.yml` | Local development (set `SPRING_PROFILES_ACTIVE=dev`) |
| Production | `application-prod.yml` | Production deployment |

Activate profile: `export SPRING_PROFILES_ACTIVE=dev` or set in `.env`

## Build Artifacts

### Frontend

| Directory | Contents | Committed? |
|-----------|----------|------------|
| `node_modules/` | NPM dependencies | No (.gitignore) |
| `dist/` | Production build output | No (.gitignore) |

### Backend

| Directory | Contents | Committed? |
|-----------|----------|------------|
| `target/` | Maven build output | No (.gitignore) |
| `.mvn/` | Maven wrapper | Yes (except wrapper JAR) |

## Testing Structure

### Frontend Tests (Future)

```
src/
  components/
    __tests__/
      IssueCard.test.tsx
  services/
    __tests__/
      issueService.test.ts
```

### Backend Tests

```
src/test/java/com/socialstresstest/
  controller/
    IssueControllerTest.java
  service/
    EvaluationServiceTest.java
```

## Documentation Organization

| Document | Audience | Content |
|----------|----------|---------|
| `README.md` | Everyone | High-level vision, features, purpose |
| `CLAUDE.md` | Developers, AI assistants | Implementation details, data models, API specs |
| `docs/ARCHITECTURE.md` | Developers, architects | System design, component diagrams, flows |
| `docs/GETTING_STARTED.md` | New developers | Setup instructions, troubleshooting |
| `docs/PROJECT_STRUCTURE.md` | New developers | File organization, conventions |

## Next Steps

1. Read `docs/GETTING_STARTED.md` to set up your environment
2. Review `CLAUDE.md` to understand implementation phases
3. Explore `docs/ARCHITECTURE.md` for system design details
4. Start implementing Phase 1: Foundation Setup

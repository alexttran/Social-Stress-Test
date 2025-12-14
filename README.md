# Social-Stress-Test
## Overview

This project is a web-based platform that helps users **think more rigorously about social issues and policy solutions**. Instead of passively consuming content, users actively propose solutions to real-world problems and receive **structured, critical evaluation** designed to stress-test and strengthen their arguments.

The platform is built around two core ideas:

1. **Structured issue breakdowns** that clearly map problems, existing solutions, tradeoffs, and constraints.
2. **Automated evaluation of user-proposed solutions**, focusing on feasibility, impact, assumptions, and weaknesses.

The goal is not to tell users what to think, but to force better thinking.

---

## Purpose

Most discussions around social issues suffer from one or more of the following:

* Oversimplification
* Ignoring tradeoffs
* Weak assumptions
* No exposure to serious counterarguments

This platform addresses that by:

* Making tradeoffs explicit
* Evaluating ideas the way a policy analyst or decision-maker would
* Helping users refine and strengthen their final arguments

It is designed as a **policy-thinking simulator**, not a debate forum or opinion feed.

---

## Core Features

### 1. Issue Argument Maps

Each social issue is presented as a structured breakdown:

* Problem definition
* Root causes
* Commonly proposed solutions
* Strengths and weaknesses of each solution
* Historical attempts and outcomes
* Ethical and economic tradeoffs
* Known constraints (budgetary, political, logistical)

This ensures users understand the problem space before proposing solutions.

---

### 2. User-Proposed Solutions

Authenticated users can submit their own solutions to an issue. A solution typically includes:

* The proposed policy or action
* Target population
* Time horizon
* Implementation assumptions

Submissions are stored and associated with the user account.

---

### 3. Automated Solution Evaluation

Each submitted solution is analyzed and critiqued by the system across multiple dimensions:

* **Feasibility** (cost, infrastructure, execution complexity)
* **Impact** (intended effects, secondary effects)
* **Equity** (who benefits, who bears the cost)
* **Assumptions** (implicit or unsupported premises)
* **Risks and unintended consequences**
* **Logical consistency**

The output is a structured critique highlighting:

* Strongest elements of the proposal
* Weakest or least-supported claims
* Missing considerations
* Suggested ways to strengthen the argument

The emphasis is on *improvement*, not scoring for popularity.

---

### 4. User History Tracking

User interactions (solution submissions, evaluations, revisions) are logged and stored as usage history. This enables:

* Reviewing past thinking
* Tracking improvement over time
* Supporting future analytics and personalization

---

## High-Level Architecture

```
React Frontend
     |
     |  (HTTPS + JWT)
     v
Spring Boot Backend (API)
     |
     |-- Auth validation (Auth0 JWTs)
     |-- Business logic (evaluation engine)
     |
     v
AWS Services
  - S3 (user history storage)
  - Optional DB (structured data)
```

---

## Authentication & Authorization

Authentication is handled by **Auth0** using OAuth 2.0 and OpenID Connect.

### Flow

1. User logs in via Auth0 Universal Login (React frontend).
2. Auth0 issues a signed JWT access token.
3. React attaches the token to API requests.
4. Spring Boot validates the token and authorizes access.

### Why Auth0

* No password handling in the backend
* Secure, battle-tested identity management
* Easy integration with React
* JWT-based authorization fits cleanly with Spring Security

The backend operates strictly as a **resource server** and does not manage sessions or credentials.

---

## Backend (Spring Boot)

### Responsibilities

* Expose REST APIs for:

  * Issue content
  * Solution submission
  * Solution evaluation
  * User history access
* Validate Auth0-issued JWTs
* Orchestrate calls to the OpenAI API for solution evaluation
* Enforce structured prompts and response schemas
* Write and read user history from AWS S3

### OpenAI Integration (Core Logic Engine)

The platform uses the **OpenAI API** to perform structured analysis and critique of user-proposed solutions.

Rather than generating free-form opinions, the model is used as a **reasoning engine** constrained by carefully designed prompts and output schemas.

The OpenAI-powered evaluation focuses on:

* Identifying assumptions
* Stress-testing feasibility
* Surfacing tradeoffs and risks
* Highlighting logical gaps
* Suggesting concrete improvements

All model outputs are treated as **inputs to the application logic**, not authoritative answers.

### Security

* Spring Security configured as an OAuth2 Resource Server
* JWT validation using Auth0 JWKS endpoint
* Route-level protection for authenticated endpoints

---

## Frontend (React)

### Responsibilities

* User authentication via Auth0
* Issue exploration UI
* Solution submission forms
* Display of structured evaluations
* User history and revisions

### Auth Handling

* Uses Auth0 React SDK
* Authorization Code Flow with PKCE
* Tokens are not manually stored or managed

---

## Data Storage

### AWS S3 (User History)

* Stores user interaction history as JSON objects
* Organized by Auth0 `sub` (stable user identifier)

Example structure:

```
s3://bucket-name/
  history/
    {user_sub}/
      {timestamp}.json
```

This design allows cheap, scalable storage of append-only usage data.

### Optional Database (Future)

A relational database (e.g., PostgreSQL) may be added for:

* Issues
* Solutions
* Evaluations
* Aggregated analytics

S3 is not ideal for querying structured data at scale.

---

## Tech Stack

**Frontend**

* JavaScript
* React
* Auth0 React SDK

**Backend**

* Java
* Spring Boot
* Spring Security (OAuth2 Resource Server)

**AI / Reasoning Engine**

* OpenAI API (LLM-based analysis and critique)

**Authentication**

* Auth0
* OAuth 2.0 / OpenID Connect
* JWT

**Cloud / Infrastructure**

* AWS S3 (user history storage)

---

## Design Philosophy

* No opinion feeds
* No engagement farming
* No popularity metrics

The platform prioritizes:

* Clarity over volume
* Tradeoffs over slogans
* Reasoning over conclusions

Users are rewarded by producing **better arguments**, not louder ones.

---

## AI Usage Philosophy

The OpenAI API is used to **augment human reasoning**, not replace it.

Key principles:

* The model critiques *arguments*, not people
* Outputs are structured and deterministic in format
* Prompts are designed to minimize ideological bias
* The system emphasizes tradeoffs and constraints over moral judgments

Model responses are logged and versioned to allow future prompt iteration and evaluation quality tracking.

---

## Future Enhancements

* Argument revision tracking
* Comparative evaluation between solutions
* Counterargument simulations
* Personal reasoning diagnostics
* Expert-reviewed benchmark solutions
* Model comparison and prompt versioning

---


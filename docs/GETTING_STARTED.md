# Getting Started Guide

This guide will help you set up the Social Stress Test platform for local development.

## Prerequisites

### Required Software
- **Node.js** 18+ and npm (for frontend)
- **Java** 17+ and Maven (for backend)
- **Git** for version control
- **Auth0 Account** (free tier works for development)
- **AWS Account** (for S3)
- **OpenAI API Key** (requires paid account)

### Recommended Tools
- **VS Code** or **IntelliJ IDEA** for development
- **Postman** or **Insomnia** for API testing
- **AWS CLI** for managing S3

## Setup Steps

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/Social-Stress-Test.git
cd Social-Stress-Test
```

### 2. Auth0 Configuration

#### Create Auth0 Application

1. Go to [Auth0 Dashboard](https://manage.auth0.com/)
2. Create a new **Single Page Application**
3. Note your:
   - Domain (e.g., `your-tenant.us.auth0.com`)
   - Client ID
4. Configure Application Settings:
   - **Allowed Callback URLs**: `http://localhost:5173/callback`
   - **Allowed Logout URLs**: `http://localhost:5173`
   - **Allowed Web Origins**: `http://localhost:5173`
   - **Allowed Origins (CORS)**: `http://localhost:5173`

#### Create Auth0 API

1. In Auth0 Dashboard, go to **Applications** > **APIs**
2. Create a new API:
   - **Name**: Social Stress Test API
   - **Identifier**: `https://api.socialstresstest.com` (can be any URL format)
3. Note the **Identifier** (this is your audience)

### 3. AWS S3 Setup

#### Create S3 Bucket

```bash
aws s3 mb s3://social-stress-test-data-dev --region us-east-1
```

Or use AWS Console:
1. Go to S3
2. Create bucket: `social-stress-test-data-dev`
3. Keep default settings (block public access)

#### Create IAM User (or use existing credentials)

1. Go to IAM > Users > Create User
2. Attach policy: `AmazonS3FullAccess` (or create custom policy with S3 read/write for your bucket)
3. Create access key and save:
   - Access Key ID
   - Secret Access Key

### 4. OpenAI API Key

1. Go to [OpenAI Platform](https://platform.openai.com/)
2. Navigate to API Keys
3. Create new secret key
4. Save it securely (you won't see it again)

### 5. Backend Setup

#### Configure Environment Variables

```bash
cd backend
cp .env.example .env
```

Edit `.env` with your actual values:

```bash
# Auth0
AUTH0_ISSUER_URI=https://your-tenant.us.auth0.com/
AUTH0_AUDIENCE=https://api.socialstresstest.com

# OpenAI
OPENAI_API_KEY=sk-your-actual-openai-key

# AWS
AWS_S3_BUCKET_NAME=social-stress-test-data-dev
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your-access-key-id
AWS_SECRET_ACCESS_KEY=your-secret-access-key

# CORS
CORS_ALLOWED_ORIGINS=http://localhost:5173

# Profile
SPRING_PROFILES_ACTIVE=dev
```

#### Install Dependencies and Run

```bash
# Install dependencies
./mvnw clean install

# Run the application
./mvnw spring-boot:run
```

The backend will start on `http://localhost:8080`

#### Verify Backend is Running

```bash
curl http://localhost:8080/actuator/health
```

Should return: `{"status":"UP"}`

### 6. Frontend Setup

#### Configure Environment Variables

```bash
cd frontend
cp .env.example .env
```

Edit `.env` with your actual values:

```bash
VITE_AUTH0_DOMAIN=your-tenant.us.auth0.com
VITE_AUTH0_CLIENT_ID=your-client-id
VITE_AUTH0_AUDIENCE=https://api.socialstresstest.com
VITE_API_BASE_URL=http://localhost:8080
```

#### Install Dependencies and Run

```bash
# Install dependencies
npm install

# Run development server
npm run dev
```

The frontend will start on `http://localhost:5173`

### 7. Test the Full Stack

1. Open browser to `http://localhost:5173`
2. Click "Login" (you'll be redirected to Auth0)
3. Create an account or login
4. You should be redirected back to the app
5. Try browsing issues (should work without auth)
6. Try submitting a solution (requires auth)

## Troubleshooting

### Backend Issues

#### JWT Validation Errors

**Error**: `An error occurred while attempting to decode the Jwt`

**Solution**:
- Verify `AUTH0_ISSUER_URI` ends with trailing slash
- Verify `AUTH0_AUDIENCE` matches your Auth0 API identifier exactly
- Check Auth0 API settings

#### S3 Access Denied

**Error**: `Access Denied` when writing to S3

**Solution**:
- Verify AWS credentials are correct
- Check IAM user has S3 permissions
- Verify bucket name matches configuration
- Try `aws s3 ls s3://your-bucket-name` to test credentials

#### OpenAI API Errors

**Error**: `401 Unauthorized` from OpenAI

**Solution**:
- Verify API key is correct (starts with `sk-`)
- Check you have sufficient credits in your OpenAI account
- Verify no extra whitespace in the key

### Frontend Issues

#### CORS Errors

**Error**: `CORS policy: No 'Access-Control-Allow-Origin' header`

**Solution**:
- Verify backend CORS configuration includes `http://localhost:5173`
- Check backend is running
- Try clearing browser cache

#### Auth0 Redirect Loop

**Error**: Continuously redirected to Auth0

**Solution**:
- Verify Allowed Callback URLs in Auth0 includes `http://localhost:5173/callback`
- Check Auth0 domain and client ID in `.env`
- Clear browser cookies for localhost

#### Module Not Found Errors

**Error**: `Cannot find module '@/...'`

**Solution**:
- Run `npm install` again
- Delete `node_modules` and `package-lock.json`, then `npm install`
- Verify Vite config has correct path alias

## Development Workflow

### Running Both Frontend and Backend

**Terminal 1 (Backend):**
```bash
cd backend
./mvnw spring-boot:run
```

**Terminal 2 (Frontend):**
```bash
cd frontend
npm run dev
```

### Making Changes

- Frontend changes auto-reload (Vite HMR)
- Backend changes auto-reload (Spring Boot DevTools)
- Configuration changes may require restart

### Testing Auth Flow

1. Logout (if logged in)
2. Open browser DevTools > Network tab
3. Click Login
4. Observe redirect to Auth0
5. After login, observe callback with `code` parameter
6. Observe token exchange
7. Check localStorage for Auth0 tokens

### Testing API Calls

**With Postman:**
1. Login to get JWT token
2. Copy token from browser localStorage: `@@auth0spajs@@::CLIENT_ID::AUDIENCE::openid profile email`
3. Add to Postman: `Authorization: Bearer YOUR_JWT_TOKEN`
4. Make requests to `http://localhost:8080/api/*`

## Next Steps

Now that your environment is set up, you can:

1. **Explore the codebase**: Read through `CLAUDE.md` and `docs/ARCHITECTURE.md`
2. **Implement Phase 1**: Start with authentication and basic API endpoints
3. **Create your first component**: Build the issue list page
4. **Test OpenAI integration**: Create an evaluation service
5. **Review the roadmap**: Check the implementation phases in CLAUDE.md

## Useful Commands

### Backend

```bash
# Clean build
./mvnw clean install

# Run tests
./mvnw test

# Run with specific profile
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev

# Package for deployment
./mvnw clean package
```

### Frontend

```bash
# Install dependencies
npm install

# Run dev server
npm run dev

# Type check
npm run type-check

# Build for production
npm run build

# Preview production build
npm run preview

# Lint code
npm run lint
```

### AWS CLI

```bash
# List S3 buckets
aws s3 ls

# List bucket contents
aws s3 ls s3://your-bucket-name

# Upload file
aws s3 cp file.json s3://your-bucket-name/path/

# Download file
aws s3 cp s3://your-bucket-name/path/file.json ./
```

## Additional Resources

- [Spring Security OAuth2 Resource Server](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/index.html)
- [Auth0 React Quickstart](https://auth0.com/docs/quickstart/spa/react)
- [Auth0 API Authorization](https://auth0.com/docs/get-started/apis)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [AWS S3 Java SDK](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/examples-s3.html)
- [Vite Documentation](https://vitejs.dev/)
- [React Router](https://reactrouter.com/)

## Getting Help

If you encounter issues:

1. Check this guide's troubleshooting section
2. Review error messages carefully
3. Check browser console and network tab
4. Check backend logs
5. Verify all environment variables are set correctly
6. Try restarting both frontend and backend

## Security Reminders

- **Never commit** `.env` files
- **Never commit** AWS credentials or API keys
- **Use `.env.example`** as template for others
- **Rotate keys** if accidentally exposed
- **Use Auth0 test tenant** for development (not production tenant)
- **Enable MFA** on your AWS and Auth0 accounts

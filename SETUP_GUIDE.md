# Setup Guide - Spring Boot + PostgreSQL DevOps Pipeline

This guide provides step-by-step instructions to set up and deploy your DevOps project.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [GitHub Repository Setup](#github-repository-setup)
3. [Adding Collaborators](#adding-collaborators)
4. [Docker Hub Setup](#docker-hub-setup)
5. [GitHub Secrets Configuration](#github-secrets-configuration)
6. [Local Development Setup](#local-development-setup)
7. [Running the Application](#running-the-application)
8. [Triggering CI/CD Pipeline](#triggering-cicd-pipeline)
9. [Verifying Deployment](#verifying-deployment)
10. [Troubleshooting](#troubleshooting)

---

## Prerequisites

Before you begin, ensure you have:

- ✅ Git installed and configured
- ✅ Docker Desktop installed and running
- ✅ GitHub account created
- ✅ Docker Hub account created
- ✅ Basic command line knowledge

### Version Requirements

```bash
git --version        # 2.x or higher
docker --version     # 20.10 or higher
docker-compose --version  # 2.0 or higher
```

---

## GitHub Repository Setup

### Step 1: Create a New Repository on GitHub

1. **Go to GitHub**: https://github.com/Mwaqarulmulk
2. **Click** the "+" icon (top right) → "New repository"
3. **Fill in details**:
   - Repository name: `spring-boot-postgres-devops`
   - Description: `Production-ready Spring Boot REST API with PostgreSQL, Docker, and CI/CD pipeline`
   - Visibility: **Public** (for free GitHub Actions)
   - **Do NOT** initialize with README (we have one already)
4. **Click** "Create repository"

### Step 2: Link Local Repository to GitHub

```bash
# Navigate to your project directory
cd H:\movies\docker-compose-spring-boot-postgres-master

# Rename branch to main (if needed)
git branch -M main

# Add GitHub remote
git remote add origin https://github.com/Mwaqarulmulk/spring-boot-postgres-devops.git

# Verify remote
git remote -v

# Push to GitHub
git push -u origin main
```

### Step 3: Verify Upload

1. Refresh your GitHub repository page
2. You should see all files uploaded:
   - ✅ README.md
   - ✅ docker-compose.yml
   - ✅ .github/workflows/cicd-pipeline.yml
   - ✅ devops_report.md
   - ✅ LICENSE
   - ✅ And more...

---

## Adding Collaborators

### Add Ghulam Mujtaba as Collaborator

1. **Navigate** to your repository on GitHub
2. **Click** "Settings" tab (top menu)
3. **Click** "Collaborators and teams" (left sidebar)
4. **Click** "Add people" button
5. **Enter** username: `ghulam-mujtaba5`
6. **Select** the user from dropdown
7. **Choose** permission level: `Write` or `Maintain`
8. **Click** "Add [username] to this repository"

### Collaborator Acceptance

The collaborator will receive an email invitation and must:
1. Click the invitation link
2. Accept the collaboration request
3. Now they can push, create branches, and contribute

---

## Docker Hub Setup

### Step 1: Create Docker Hub Account (if not already done)

1. **Go to**: https://hub.docker.com/signup
2. **Fill in**:
   - Docker ID: `waqarulmulk`
   - Email: Your email
   - Password: `Ee123321@` (or your secure password)
3. **Verify** your email address

### Step 2: Create Access Token (Recommended for Security)

**Note**: Using access tokens is more secure than using your password.

1. **Log in** to Docker Hub
2. **Click** your username (top right) → "Account Settings"
3. **Click** "Security" (left sidebar)
4. **Click** "New Access Token"
5. **Fill in**:
   - Description: `GitHub Actions CI/CD`
   - Access permissions: `Read, Write, Delete`
6. **Click** "Generate"
7. **Copy** the token immediately (you won't see it again!)
   - Example: `dckr_pat_abcdef123456...`

**Important**: Save this token securely! You'll use it in GitHub Secrets.

### Step 3: Create Docker Hub Repository (Optional)

Docker Hub will automatically create the repository when you first push, but you can create it manually:

1. **Click** "Repositories" → "Create Repository"
2. **Enter** name: `spring-boot-postgres-app`
3. **Choose** visibility: Public
4. **Click** "Create"

---

## GitHub Secrets Configuration

Secrets keep your credentials secure and out of your code.

### Step 1: Navigate to Secrets

1. **Go to** your GitHub repository
2. **Click** "Settings" tab
3. **Click** "Secrets and variables" → "Actions" (left sidebar)

### Step 2: Add Docker Hub Username

1. **Click** "New repository secret"
2. **Name**: `DOCKER_USERNAME`
3. **Secret**: `waqarulmulk`
4. **Click** "Add secret"

### Step 3: Add Docker Hub Password/Token

1. **Click** "New repository secret" again
2. **Name**: `DOCKER_PASSWORD`
3. **Secret**: 
   - If using access token: Paste the token (e.g., `dckr_pat_...`)
   - If using password: `Ee123321@`
4. **Click** "Add secret"

### Step 4: Verify Secrets

You should now see:
- ✅ `DOCKER_USERNAME`
- ✅ `DOCKER_PASSWORD`

**Note**: You can't view secret values after creation (security feature).

---

## Local Development Setup

### Step 1: Clone Repository (if on different machine)

```bash
git clone https://github.com/Mwaqarulmulk/spring-boot-postgres-devops.git
cd spring-boot-postgres-devops
```

### Step 2: Create Environment File

```bash
# Copy example file
cp .env.example .env

# Edit the file with your preferred editor
notepad .env
# OR
nano .env
# OR
vim .env
```

### Step 3: Configure Environment Variables

Edit `.env` file:

```env
# Database Configuration
POSTGRESDB_USER=admin
POSTGRESDB_ROOT_PASSWORD=admin123
POSTGRESDB_DATABASE=bezkoder_db

# PostgreSQL Ports
POSTGRESDB_LOCAL_PORT=5432
POSTGRESDB_DOCKER_PORT=5432

# Spring Boot Ports
SPRING_LOCAL_PORT=8080
SPRING_DOCKER_PORT=8080
```

**Save and close** the file.

---

## Running the Application

### Method 1: Using Docker Compose (Recommended)

```bash
# Ensure Docker is running
docker ps

# Build and start all services
docker-compose up -d

# This will:
# 1. Pull PostgreSQL image
# 2. Build Spring Boot image
# 3. Create network and volumes
# 4. Start both containers

# Wait 30-40 seconds for services to start

# View logs
docker-compose logs -f

# Check running containers
docker ps
```

**Expected Output**:
```
CONTAINER ID   IMAGE                    STATUS         PORTS
abc123...      bezkoder-spring-app      Up (healthy)   0.0.0.0:8080->8080/tcp
def456...      postgres:15-alpine       Up (healthy)   0.0.0.0:5432->5432/tcp
```

### Method 2: Using Docker Commands

```bash
# Create network
docker network create spring-postgres-network

# Start PostgreSQL
docker run -d \
  --name bezkoder-postgres \
  --network spring-postgres-network \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=admin123 \
  -e POSTGRES_DB=bezkoder_db \
  -p 5432:5432 \
  postgres:15-alpine

# Build Spring Boot image
cd bezkoder-app
docker build -t spring-app:latest .

# Run Spring Boot
docker run -d \
  --name bezkoder-spring-app \
  --network spring-postgres-network \
  -e SPRING_DATASOURCE_URL=jdbc:postgresql://bezkoder-postgres:5432/bezkoder_db \
  -e SPRING_DATASOURCE_USERNAME=admin \
  -e SPRING_DATASOURCE_PASSWORD=admin123 \
  -p 8080:8080 \
  spring-app:latest
```

### Verify Application is Running

```bash
# Test API endpoint
curl http://localhost:8080/api/tutorials

# Expected response: []
# (Empty array since no tutorials exist yet)

# If using Windows and curl doesn't work, open in browser:
# http://localhost:8080/api/tutorials
```

---

## Triggering CI/CD Pipeline

The CI/CD pipeline runs automatically, but here's how to trigger it:

### Automatic Triggers

The pipeline runs automatically when you:

1. **Push to main branch**
   ```bash
   git add .
   git commit -m "feat: add new feature"
   git push origin main
   ```

2. **Push to develop branch**
   ```bash
   git checkout -b develop
   git push origin develop
   ```

3. **Create Pull Request**
   - Create a branch
   - Push changes
   - Open PR on GitHub

### Manual Trigger

1. **Go to** GitHub repository
2. **Click** "Actions" tab
3. **Click** "DevOps CI/CD Pipeline" (left sidebar)
4. **Click** "Run workflow" (right side)
5. **Select** branch (e.g., main)
6. **Click** "Run workflow" button

### Monitoring Pipeline

1. **Go to** "Actions" tab
2. **Click** on the running workflow
3. **Watch** each stage execute:
   - 🏗️ Build & Install
   - 🔍 Lint & Security Scan
   - 🧪 Test with Database
   - 🐳 Build & Push Docker Image
   - 🚀 Deploy (main branch only)

### Pipeline Duration

- **Total time**: ~8-10 minutes
- **Build**: ~2 minutes
- **Security scan**: ~2 minutes
- **Tests**: ~3 minutes
- **Docker build**: ~3 minutes
- **Deploy**: ~1 minute

---

## Verifying Deployment

### Check GitHub Actions

1. ✅ All stages should show green checkmarks
2. ✅ "Pipeline Summary" should show success
3. ✅ Deployment report should be visible

### Check Docker Hub

1. **Go to**: https://hub.docker.com/r/waqarulmulk/spring-boot-postgres-app
2. **Verify**:
   - ✅ Repository exists
   - ✅ Latest tag is present
   - ✅ Image was pushed recently
   - ✅ Image size is ~250 MB

### Pull and Run Deployed Image

```bash
# Pull the image from Docker Hub
docker pull waqarulmulk/spring-boot-postgres-app:latest

# Verify image
docker images | grep spring-boot-postgres-app

# Run the deployed image
docker run -d \
  -e SPRING_DATASOURCE_URL=jdbc:postgresql://host.docker.internal:5432/bezkoder_db \
  -e SPRING_DATASOURCE_USERNAME=admin \
  -e SPRING_DATASOURCE_PASSWORD=admin123 \
  -p 8080:8080 \
  waqarulmulk/spring-boot-postgres-app:latest

# Test
curl http://localhost:8080/api/tutorials
```

---

## Troubleshooting

### Issue 1: "Connection refused" to database

**Symptoms**: Application can't connect to PostgreSQL

**Solutions**:

```bash
# Check if PostgreSQL is running
docker ps | grep postgres

# Check PostgreSQL logs
docker logs bezkoder-postgres

# Verify database is ready
docker exec bezkoder-postgres pg_isready -U admin

# Restart services
docker-compose restart
```

### Issue 2: Port already in use

**Symptoms**: `Error: bind: address already in use`

**Solutions**:

```bash
# Windows - Find process using port 8080
netstat -ano | findstr :8080

# Kill the process (replace PID)
taskkill /PID <PID> /F

# Or change port in .env file
SPRING_LOCAL_PORT=8081
```

### Issue 3: GitHub Actions pipeline fails

**Symptoms**: Red X on pipeline stages

**Solutions**:

1. **Check logs**:
   - Click on the failed stage
   - Read error messages
   - Common issues: Missing secrets, Docker Hub login failure

2. **Verify secrets**:
   - Go to Settings → Secrets
   - Ensure DOCKER_USERNAME and DOCKER_PASSWORD exist
   - Re-add if necessary

3. **Check Docker Hub credentials**:
   - Log in manually: `docker login`
   - Use username: `waqarulmulk`
   - Use password/token

### Issue 4: Docker build fails

**Symptoms**: `Error: failed to build`

**Solutions**:

```bash
# Clear Docker cache
docker builder prune -a

# Rebuild without cache
docker-compose build --no-cache

# Check Dockerfile syntax
docker build --no-cache -t test-build ./bezkoder-app
```

### Issue 5: Tests fail in pipeline

**Symptoms**: Test stage shows red X

**Solutions**:

```bash
# Run tests locally to reproduce
cd bezkoder-app
mvn test

# Check test database connection
# Verify PostgreSQL service is running in Actions

# Review test logs in GitHub Actions
```

### Issue 6: Cannot push to GitHub

**Symptoms**: `Permission denied` or `Authentication failed`

**Solutions**:

```bash
# Configure Git credentials
git config --global user.name "Mwaqarulmulk"
git config --global user.email "your-email@example.com"

# Use Personal Access Token (PAT) instead of password
# Generate PAT: GitHub → Settings → Developer settings → Personal access tokens

# Or use SSH
ssh-keygen -t ed25519 -C "your-email@example.com"
# Add SSH key to GitHub: Settings → SSH and GPG keys
```

### Issue 7: Application runs but endpoints return errors

**Symptoms**: 500 Internal Server Error or connection errors

**Solutions**:

```bash
# Check application logs
docker logs bezkoder-spring-app

# Check database connection
docker exec bezkoder-spring-app env | grep SPRING_DATASOURCE

# Verify database exists
docker exec -it bezkoder-postgres psql -U admin -d bezkoder_db

# Check if tables are created
docker exec -it bezkoder-postgres psql -U admin -d bezkoder_db -c "\dt"
```

### Getting Help

If you're still stuck:

1. **Check documentation**: README.md and devops_report.md
2. **Search Issues**: Someone may have had the same problem
3. **Create an Issue**: Provide detailed error logs
4. **Ask collaborators**: Contact team members

---

## Next Steps

Once everything is working:

1. ✅ **Test the API**:
   ```bash
   # Create tutorial
   curl -X POST http://localhost:8080/api/tutorials \
     -H "Content-Type: application/json" \
     -d '{"title":"My First Tutorial","description":"Learning DevOps","published":false}'
   
   # Get all tutorials
   curl http://localhost:8080/api/tutorials
   ```

2. ✅ **Take screenshots** for your report:
   - ✅ GitHub Actions pipeline (passing)
   - ✅ Docker Hub repository
   - ✅ Running containers (`docker ps`)
   - ✅ API response

3. ✅ **Document your work**:
   - Update devops_report.md with your experience
   - Add notes about challenges faced
   - Include screenshots in a `/screenshots` folder

4. ✅ **Submit your project**:
   - Public GitHub repository link
   - Docker Hub repository link
   - Screenshots
   - Documentation (README.md + devops_report.md)

---

## Deliverables Checklist

For your lab exam, ensure you have:

- ✅ Public GitHub repository
- ✅ Collaborator added (ghulam-mujtaba5)
- ✅ Working Dockerfile
- ✅ Working docker-compose.yml
- ✅ GitHub Actions YAML file (.github/workflows/cicd-pipeline.yml)
- ✅ All 5 pipeline stages working
- ✅ Docker Hub image published
- ✅ README.md (technical documentation)
- ✅ devops_report.md (detailed report)
- ✅ Screenshots folder with:
  - Passing pipeline
  - Running containers
  - Docker Hub deployment proof
  - API testing proof

---

## Quick Reference Commands

### Docker Commands
```bash
# Start
docker-compose up -d

# Stop
docker-compose down

# Logs
docker-compose logs -f

# Restart
docker-compose restart

# Clean everything
docker-compose down -v --rmi all
```

### Git Commands
```bash
# Status
git status

# Add changes
git add .

# Commit
git commit -m "your message"

# Push
git push origin main

# Pull latest
git pull origin main
```

### Testing Commands
```bash
# Test API
curl http://localhost:8080/api/tutorials

# Check containers
docker ps

# Check images
docker images

# Check Docker Hub login
docker login
```

---

**Congratulations! You've successfully set up your DevOps pipeline! 🎉**

If you encounter any issues, refer to the troubleshooting section or create an issue on GitHub.

**Good luck with your lab exam!** 🚀
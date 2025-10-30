# 🚀 Spring Boot + PostgreSQL DevOps Pipeline

![CI/CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?logo=github-actions&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.1.0-6DB33F?logo=spring-boot&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?logo=postgresql&logoColor=white)
![Java](https://img.shields.io/badge/Java-17-007396?logo=java&logoColor=white)

A production-ready Spring Boot REST API with PostgreSQL database, fully containerized with Docker Compose and automated CI/CD pipeline using GitHub Actions.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Tech Stack](#-tech-stack)
- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [API Documentation](#-api-documentation)
- [Docker Commands](#-docker-commands)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Environment Variables](#-environment-variables)
- [Testing](#-testing)
- [Security](#-security)
- [Contributing](#-contributing)
- [Contributors](#-contributors)
- [License](#-license)

---

## 🎯 Project Overview

This project demonstrates a **production-grade DevOps workflow** for a Spring Boot application with PostgreSQL database. It includes:

- **Full containerization** using Docker and Docker Compose
- **Multi-stage Docker builds** for optimized image size
- **5-stage CI/CD pipeline** with GitHub Actions
- **Automated testing** with database integration
- **Security scanning** with Trivy and OWASP
- **Secrets management** using GitHub Secrets
- **Automated deployment** to Docker Hub

### Purpose
This project was created as part of a DevOps lab exam to demonstrate:
- Containerization best practices
- CI/CD pipeline design and implementation
- Security-first development approach
- Infrastructure as Code (IaC)
- Automated testing and deployment

---

## 🛠️ Tech Stack

### Backend
- **Java 17** - Programming language
- **Spring Boot 3.1.0** - Application framework
- **Spring Data JPA** - Data persistence
- **Maven 3.8.5** - Build tool

### Database
- **PostgreSQL 15** - Relational database
- **Alpine Linux** - Minimal base image

### DevOps & Tools
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration
- **GitHub Actions** - CI/CD automation
- **Trivy** - Security vulnerability scanning
- **OWASP Dependency Check** - Dependency vulnerability analysis

---

## ✨ Features

### Application Features
- ✅ RESTful API for Tutorial management (CRUD operations)
- ✅ PostgreSQL database integration
- ✅ JPA/Hibernate ORM
- ✅ Automatic schema generation
- ✅ Health check endpoints
- ✅ Production-ready configuration

### DevOps Features
- ✅ **Multi-stage Docker builds** - Reduced image size (~70% smaller)
- ✅ **Non-root container user** - Enhanced security
- ✅ **Health checks** - Automatic container monitoring
- ✅ **Persistent volumes** - Data preservation
- ✅ **Internal networking** - Secure container communication
- ✅ **Automated CI/CD** - 5-stage pipeline
- ✅ **Automated testing** - Unit & integration tests
- ✅ **Security scanning** - Vulnerability detection
- ✅ **Docker Hub integration** - Automated image publishing

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    GitHub Actions CI/CD                      │
│  ┌──────────┐  ┌──────────┐  ┌──────┐  ┌────────┐  ┌──────┐│
│  │  Build   │→ │Lint/Scan │→ │ Test │→ │ Docker │→ │Deploy││
│  └──────────┘  └──────────┘  └──────┘  └────────┘  └──────┘│
└─────────────────────────────────────────────────────────────┘
                              ↓
                    ┌─────────────────────┐
                    │    Docker Hub       │
                    └─────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                  Docker Compose Network                      │
│                                                               │
│  ┌──────────────────────┐       ┌─────────────────────────┐│
│  │   Spring Boot App    │←─────→│   PostgreSQL Database   ││
│  │   (Port 8080)        │       │   (Port 5432)           ││
│  │                      │       │                         ││
│  │  - REST API          │       │  - Persistent Volume    ││
│  │  - Business Logic    │       │  - Health Checks        ││
│  │  - Health Checks     │       │  - Alpine Image         ││
│  └──────────────────────┘       └─────────────────────────┘│
│           ↓                                ↓                 │
│  ┌──────────────────────────────────────────────────────┐  │
│  │           Spring-Postgres Network (Bridge)            │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Docker** (v20.10 or higher)
  ```bash
  docker --version
  ```

- **Docker Compose** (v2.0 or higher)
  ```bash
  docker-compose --version
  ```

- **Git** (for cloning the repository)
  ```bash
  git --version
  ```

- **Java 17** (optional, for local development)
  ```bash
  java -version
  ```

- **Maven 3.8+** (optional, for local development)
  ```bash
  mvn -version
  ```

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Mwaqarulmulk/spring-boot-postgres-devops.git
cd spring-boot-postgres-devops
```

### 2. Configure Environment Variables

Create a `.env` file in the root directory:

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

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

### 3. Build and Run with Docker Compose

```bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Check running containers
docker ps
```

### 4. Access the Application

- **API Base URL**: http://localhost:8080
- **Health Check**: http://localhost:8080/api/tutorials
- **PostgreSQL**: localhost:5432

### 5. Test the API

```bash
# Create a new tutorial
curl -X POST http://localhost:8080/api/tutorials \
  -H "Content-Type: application/json" \
  -d '{"title":"Docker Tutorial","description":"Learn Docker basics","published":false}'

# Get all tutorials
curl http://localhost:8080/api/tutorials

# Get tutorial by ID
curl http://localhost:8080/api/tutorials/1

# Update tutorial
curl -X PUT http://localhost:8080/api/tutorials/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Docker Advanced","description":"Advanced Docker concepts","published":true}'

# Delete tutorial
curl -X DELETE http://localhost:8080/api/tutorials/1
```

---

## 📚 API Documentation

### Base URL
```
http://localhost:8080/api
```

### Endpoints

#### 1. Get All Tutorials
```http
GET /api/tutorials
```

**Response:**
```json
[
  {
    "id": 1,
    "title": "Spring Boot Tutorial",
    "description": "Learn Spring Boot basics",
    "published": true
  }
]
```

#### 2. Get Tutorial by ID
```http
GET /api/tutorials/{id}
```

**Response:**
```json
{
  "id": 1,
  "title": "Spring Boot Tutorial",
  "description": "Learn Spring Boot basics",
  "published": true
}
```

#### 3. Create New Tutorial
```http
POST /api/tutorials
Content-Type: application/json

{
  "title": "Docker Tutorial",
  "description": "Learn containerization",
  "published": false
}
```

**Response:**
```json
{
  "id": 2,
  "title": "Docker Tutorial",
  "description": "Learn containerization",
  "published": false
}
```

#### 4. Update Tutorial
```http
PUT /api/tutorials/{id}
Content-Type: application/json

{
  "title": "Docker Advanced Tutorial",
  "description": "Advanced Docker concepts",
  "published": true
}
```

#### 5. Delete Tutorial
```http
DELETE /api/tutorials/{id}
```

#### 6. Get Published Tutorials
```http
GET /api/tutorials/published
```

#### 7. Find Tutorials by Title
```http
GET /api/tutorials?title=Docker
```

#### 8. Delete All Tutorials
```http
DELETE /api/tutorials
```

---

## 🐳 Docker Commands

### Basic Operations

```bash
# Build images
docker-compose build

# Start services in detached mode
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs -f app
docker-compose logs -f postgresdb

# Restart a specific service
docker-compose restart app

# Remove all containers, networks, volumes
docker-compose down -v --rmi all
```

### Debugging

```bash
# Access Spring Boot container shell
docker exec -it bezkoder-spring-app sh

# Access PostgreSQL container
docker exec -it bezkoder-postgres psql -U admin -d bezkoder_db

# View container resource usage
docker stats

# Inspect container
docker inspect bezkoder-spring-app
```

### Image Management

```bash
# List all images
docker images

# Remove unused images
docker image prune -a

# Pull from Docker Hub
docker pull waqarulmulk/spring-boot-postgres-app:latest

# Run the pulled image
docker run -p 8080:8080 waqarulmulk/spring-boot-postgres-app:latest
```

---

## 🔄 CI/CD Pipeline

### Pipeline Stages

The GitHub Actions pipeline consists of **5 main stages**:

#### 1️⃣ **Build & Install**
- Checkout repository
- Set up JDK 17
- Cache Maven dependencies
- Build application with Maven
- Upload build artifacts

#### 2️⃣ **Lint & Security Scan**
- Run Maven Checkstyle validation
- OWASP dependency vulnerability check
- Trivy filesystem security scan
- Generate security reports

#### 3️⃣ **Test with Database**
- Spin up PostgreSQL service container
- Run unit and integration tests
- Verify database connectivity
- Publish test results

#### 4️⃣ **Build Docker Image**
- Build multi-stage Docker image
- Push to Docker Hub with multiple tags
- Scan Docker image with Trivy
- Generate build reports

#### 5️⃣ **Deploy (Conditional)**
- Only runs on `main` branch
- Verify Docker image on Docker Hub
- Deploy to production
- Generate deployment reports

### Pipeline Triggers

```yaml
# Automatically runs on:
- Push to main or develop branch
- Pull requests to main branch
- Manual workflow dispatch
```

### Pipeline Status

Check pipeline status at:
```
https://github.com/Mwaqarulmulk/spring-boot-postgres-devops/actions
```

### Setting Up GitHub Secrets

For the pipeline to work, configure these secrets in your GitHub repository:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Add the following secrets:

| Secret Name | Description | Example Value |
|-------------|-------------|---------------|
| `DOCKER_USERNAME` | Docker Hub username | `waqarulmulk` |
| `DOCKER_PASSWORD` | Docker Hub password/token | `your_docker_password` |

---

## 🔐 Environment Variables

### Application Configuration

Create a `.env` file with the following variables:

```env
# Database Configuration
POSTGRESDB_USER=admin
POSTGRESDB_ROOT_PASSWORD=securePassword123
POSTGRESDB_DATABASE=bezkoder_db

# Port Configuration
POSTGRESDB_LOCAL_PORT=5432
POSTGRESDB_DOCKER_PORT=5432
SPRING_LOCAL_PORT=8080
SPRING_DOCKER_PORT=8080
```

### Production Best Practices

⚠️ **Security Note**: Never commit `.env` files to version control!

- Use strong passwords for database
- Change default credentials before production
- Use environment-specific configurations
- Store secrets in GitHub Secrets for CI/CD

---

## 🧪 Testing

### Run Tests Locally

```bash
cd bezkoder-app

# Run all tests
mvn test

# Run tests with coverage
mvn test jacoco:report

# Run specific test class
mvn test -Dtest=TutorialControllerTests

# Skip tests during build
mvn clean install -DskipTests
```

### Test with Docker

```bash
# Run tests in container
docker-compose run app mvn test
```

### Automated Testing in CI/CD

Tests run automatically on every push:
- Unit tests with JUnit
- Integration tests with PostgreSQL
- Database connectivity tests
- API endpoint tests

---

## 🔒 Security

### Security Measures Implemented

1. **Multi-stage Docker Build**
   - Separate build and runtime stages
   - Minimal runtime image (JDK Slim)
   - Reduced attack surface

2. **Non-root User**
   - Application runs as non-root user `spring`
   - Enhanced container security

3. **Secrets Management**
   - Environment variables for sensitive data
   - GitHub Secrets for CI/CD credentials
   - No hardcoded passwords

4. **Security Scanning**
   - **Trivy**: Scans for vulnerabilities in:
     - Docker images
     - Filesystem dependencies
   - **OWASP Dependency Check**: Analyzes Java dependencies

5. **Network Isolation**
   - Internal Docker network
   - Services communicate only within network
   - No direct external exposure

6. **Health Checks**
   - Automatic container health monitoring
   - Restart on failure

### Security Scan Reports

View security scan results in GitHub Actions:
```
Actions → CI/CD Pipeline → Lint & Security Scan
```

---

## 🤝 Contributing

### Branching Strategy

```
main (production)
  ↑
develop (development)
  ↑
feature/* (features)
bugfix/* (bug fixes)
hotfix/* (urgent fixes)
```

### Contribution Workflow

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Make your changes**
4. **Commit with meaningful messages**
   ```bash
   git commit -m "feat: add amazing feature"
   ```
5. **Push to your fork**
   ```bash
   git push origin feature/amazing-feature
   ```
6. **Create a Pull Request**

### Commit Message Convention

```
feat: Add new feature
fix: Fix a bug
docs: Update documentation
style: Code style changes
refactor: Code refactoring
test: Add tests
chore: Maintenance tasks
```

---

## 👥 Contributors

### Project Team

- **[Mwaqarulmulk](https://github.com/Mwaqarulmulk)** - Project Lead & DevOps Engineer
  - Docker containerization
  - CI/CD pipeline implementation
  - Infrastructure setup

- **[Ghulam Mujtaba](https://github.com/ghulam-mujtaba5/)** - Collaborator
  - Testing and quality assurance
  - Documentation
  - Code review

### Acknowledgments

- Original Spring Boot + PostgreSQL tutorial by [BezKoder](https://www.bezkoder.com/)
- DevOps best practices from industry standards
- Community contributions and feedback

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 📞 Contact & Support

### Issues & Questions

- **GitHub Issues**: [Create an issue](https://github.com/Mwaqarulmulk/spring-boot-postgres-devops/issues)
- **Discussions**: [Join discussions](https://github.com/Mwaqarulmulk/spring-boot-postgres-devops/discussions)

### Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Docker Documentation](https://docs.docker.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---

## 🎓 Lab Exam Information

### Course Details
- **Course**: DevOps Practices
- **CLO**: Apply DevOps pipeline automation techniques for code deployment
- **Marks**: 25
- **Bloom Taxonomy Level**: Applying

### Deliverables Completed
- ✅ Public GitHub repository
- ✅ Working Dockerfile and docker-compose.yml
- ✅ GitHub Actions CI/CD pipeline (5 stages)
- ✅ Docker Hub integration
- ✅ Comprehensive documentation
- ✅ Security scanning implementation
- ✅ Automated testing with database
- ✅ Secrets management
- ✅ Production-ready deployment

---

## 🌟 Project Highlights

### What Makes This Project Stand Out

1. **Production-Ready Architecture**
   - Multi-stage Docker builds
   - Health checks and monitoring
   - Proper error handling

2. **Advanced CI/CD**
   - 5-stage comprehensive pipeline
   - Automated security scanning
   - Conditional deployment

3. **Security-First Approach**
   - Multiple security tools
   - Non-root containers
   - Secrets management

4. **Comprehensive Documentation**
   - Detailed README
   - DevOps report
   - Inline code comments

5. **Best Practices**
   - Clean code structure
   - Proper versioning
   - Logging and monitoring

---

<div align="center">

**⭐ If you find this project helpful, please give it a star! ⭐**

Made with ❤️ by [Mwaqarulmulk](https://github.com/Mwaqarulmulk)

</div>
# Contributing to Spring Boot + PostgreSQL DevOps Pipeline

Thank you for your interest in contributing to this project! We welcome contributions from everyone.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Reporting Bugs](#reporting-bugs)
- [Suggesting Enhancements](#suggesting-enhancements)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for all. Please be respectful and constructive in your interactions.

### Our Standards

**Positive behavior includes:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

**Unacceptable behavior includes:**
- Harassment, trolling, or discriminatory comments
- Publishing others' private information without permission
- Other conduct which could be considered inappropriate in a professional setting

---

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **Git** (v2.x or higher)
- **Docker** (v20.10 or higher)
- **Docker Compose** (v2.0 or higher)
- **Java 17** (for local development)
- **Maven 3.8+** (for local development)

### Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/spring-boot-postgres-devops.git
   cd spring-boot-postgres-devops
   ```

3. Add the upstream repository:
   ```bash
   git remote add upstream https://github.com/Mwaqarulmulk/spring-boot-postgres-devops.git
   ```

4. Keep your fork in sync:
   ```bash
   git fetch upstream
   git merge upstream/main
   ```

---

## Development Setup

### 1. Environment Configuration

Create a `.env` file from the example:

```bash
cp .env.example .env
```

Edit `.env` with your local configuration:

```env
POSTGRESDB_USER=admin
POSTGRESDB_ROOT_PASSWORD=admin123
POSTGRESDB_DATABASE=bezkoder_db
POSTGRESDB_LOCAL_PORT=5432
POSTGRESDB_DOCKER_PORT=5432
SPRING_LOCAL_PORT=8080
SPRING_DOCKER_PORT=8080
```

### 2. Build and Run

```bash
# Build and start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### 3. Local Development (Without Docker)

```bash
cd bezkoder-app

# Install dependencies
mvn clean install

# Run application
mvn spring-boot:run

# Run tests
mvn test
```

---

## How to Contribute

### Types of Contributions

We welcome various types of contributions:

1. **Bug Fixes** - Found a bug? Submit a fix!
2. **Features** - Have an idea? Implement it!
3. **Documentation** - Improve README, guides, or code comments
4. **Tests** - Add test coverage
5. **Performance** - Optimize code or Docker images
6. **Security** - Improve security measures

### Contribution Workflow

1. **Check existing issues** - See if someone is already working on it
2. **Create an issue** - Describe what you want to work on
3. **Get feedback** - Wait for maintainer approval
4. **Create a branch** - Start your work
5. **Make changes** - Write code following our standards
6. **Test thoroughly** - Ensure all tests pass
7. **Submit PR** - Create a pull request

---

## Pull Request Process

### Before Submitting

- ✅ Code builds successfully
- ✅ All tests pass
- ✅ New tests added for new features
- ✅ Documentation updated
- ✅ Commit messages follow conventions
- ✅ No merge conflicts with main branch

### Branch Naming Convention

Use descriptive branch names:

```
feature/add-user-authentication
bugfix/fix-database-connection
docs/update-readme
refactor/optimize-docker-build
test/add-controller-tests
```

### Pull Request Template

When creating a PR, include:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Performance improvement
- [ ] Refactoring

## Testing
Describe how you tested your changes

## Screenshots (if applicable)
Add screenshots showing the changes

## Checklist
- [ ] Code builds successfully
- [ ] All tests pass
- [ ] Documentation updated
- [ ] Follows coding standards
```

### Review Process

1. **Automated checks** run on your PR
2. **Maintainer review** - Usually within 48 hours
3. **Address feedback** - Make requested changes
4. **Approval** - PR is approved
5. **Merge** - Maintainer merges your PR

---

## Coding Standards

### Java/Spring Boot Standards

#### Code Style

```java
// Good: Descriptive names
public class TutorialService {
    private final TutorialRepository tutorialRepository;
    
    public Tutorial createTutorial(Tutorial tutorial) {
        return tutorialRepository.save(tutorial);
    }
}

// Bad: Vague names
public class TS {
    private TR tr;
    
    public T create(T t) {
        return tr.save(t);
    }
}
```

#### Best Practices

- **Use dependency injection** - Prefer constructor injection
- **Follow SOLID principles** - Single responsibility, Open/closed, etc.
- **Handle exceptions properly** - Use try-catch or @ControllerAdvice
- **Add JavaDoc comments** - Document public methods
- **Use Optional** - For methods that may return null

### Docker Standards

#### Dockerfile

```dockerfile
# Good: Multi-stage build with comments
FROM maven:3.8.5-openjdk-17-slim AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Bad: Single stage, no optimization
FROM maven:3.8.5-openjdk-17
COPY . .
RUN mvn clean package
```

#### docker-compose.yml

```yaml
# Good: Health checks and proper configuration
services:
  app:
    build: ./app
    depends_on:
      db:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080"]
      interval: 30s

# Bad: No health checks or dependencies
services:
  app:
    build: ./app
```

---

## Testing Guidelines

### Test Structure

Follow the **AAA pattern** (Arrange, Act, Assert):

```java
@Test
void shouldCreateTutorialSuccessfully() {
    // Arrange
    Tutorial tutorial = new Tutorial("Title", "Description", false);
    when(repository.save(any())).thenReturn(tutorial);
    
    // Act
    Tutorial result = service.createTutorial(tutorial);
    
    // Assert
    assertNotNull(result);
    assertEquals("Title", result.getTitle());
    verify(repository, times(1)).save(any());
}
```

### Test Coverage

- **Aim for 80%+ coverage**
- **Test happy paths** - Normal execution
- **Test edge cases** - Boundaries, null values
- **Test error handling** - Exceptions, validation

### Running Tests

```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=TutorialControllerTest

# Run with coverage
mvn test jacoco:report

# View coverage report
open target/site/jacoco/index.html
```

---

## Commit Message Guidelines

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, no code change)
- **refactor**: Code refactoring
- **test**: Adding or updating tests
- **chore**: Maintenance tasks

### Examples

```bash
# Good commit messages
feat(api): add endpoint for tutorial search
fix(docker): resolve container startup timing issue
docs(readme): update installation instructions
test(controller): add tests for tutorial deletion

# Bad commit messages
updated stuff
fix bug
changes
wip
```

### Best Practices

- Use imperative mood: "add" not "added"
- Keep subject line under 50 characters
- Capitalize subject line
- No period at the end of subject
- Separate subject from body with blank line
- Wrap body at 72 characters
- Explain **what** and **why**, not **how**

---

## Reporting Bugs

### Before Reporting

1. **Search existing issues** - Someone may have reported it
2. **Try latest version** - Bug might be fixed
3. **Check documentation** - Might be expected behavior
4. **Isolate the problem** - Minimal reproduction steps

### Bug Report Template

```markdown
**Bug Description**
Clear and concise description

**Steps to Reproduce**
1. Go to '...'
2. Click on '...'
3. See error

**Expected Behavior**
What you expected to happen

**Actual Behavior**
What actually happened

**Screenshots**
If applicable

**Environment**
- OS: [e.g., Windows 10]
- Docker: [e.g., 20.10.17]
- Java: [e.g., 17.0.2]
- Branch: [e.g., main]

**Additional Context**
Any other relevant information
```

---

## Suggesting Enhancements

### Enhancement Request Template

```markdown
**Feature Description**
Clear description of the feature

**Problem it Solves**
Why is this feature needed?

**Proposed Solution**
How would you implement it?

**Alternatives Considered**
Other approaches you thought about

**Additional Context**
Mockups, examples, references
```

---

## Questions?

If you have questions:

1. **Check documentation** - README.md and devops_report.md
2. **Search issues** - Someone may have asked already
3. **Open a discussion** - Use GitHub Discussions
4. **Ask maintainers** - Create an issue with the question label

---

## Recognition

Contributors will be:

- Listed in the project README
- Credited in release notes
- Given a shoutout on social media (if applicable)

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for contributing! 🎉**

Your efforts help make this project better for everyone.
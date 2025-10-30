# Stage 1: Build stage
FROM maven:3.8.5-openjdk-17-slim AS build

WORKDIR /app

# Copy only pom.xml first to leverage Docker cache
COPY pom.xml .

# Download dependencies (this layer will be cached if pom.xml doesn't change)
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build the application
RUN mvn clean package -DskipTests

# Stage 2: Runtime stage
FROM openjdk:17-jdk-slim

WORKDIR /app

# Create a non-root user for security
RUN groupadd -r spring && useradd -r -g spring spring

# Copy the jar from build stage
COPY --from=build /app/target/*.jar app.jar

# Change ownership to non-root user
RUN chown -R spring:spring /app

# Switch to non-root user
USER spring

# Expose the application port
EXPOSE 8080

# Health check
# Use the application endpoint available in this project. The original Dockerfile used
# /actuator/health but the project does not include the actuator dependency by default.
# We change the healthcheck to hit the public API endpoint so the container reports healthy
# when the application responds. If you prefer actuator, add `spring-boot-starter-actuator`
# to `pom.xml` and restore the actuator health endpoint.
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:8080/api/tutorials || exit 1

# Run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]

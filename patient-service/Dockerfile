# === Stage 1: Build the Application ===
FROM maven:3.9.9-eclipse-temurin-21-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy only the pom.xml first to leverage Docker layer caching for dependencies
COPY pom.xml .

# Download and cache the dependencies
RUN mvn dependency:go-offline -B

# Copy the actual source code
COPY src ./src

# Compile and package the application (skipping tests for build speed)
RUN mvn clean package -DskipTests

# === Stage 2: Run the Application ===
# Note: 'openjdk' image is deprecated. Eclipse Temurin is the modern standard.
FROM eclipse-temurin:21-jre-alpine AS runner

WORKDIR /app

# Copy the compiled JAR file from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose the internal container port (Spring Boot default)
EXPOSE 8080

# Execute the application
ENTRYPOINT ["java", "-jar", "app.jar"]
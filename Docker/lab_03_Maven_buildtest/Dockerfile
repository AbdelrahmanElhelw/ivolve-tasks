# 1. Use Maven base image with Java 17 (Alpine version for smaller size)
FROM maven:3.9-eclipse-temurin-17-alpine

# 2. Create a non-root user for security
RUN adduser -D appuser

# 3. Create work directory inside the container
WORKDIR /app

# 4. Cache dependencies (Docker Layer Caching Best Practice)
COPY pom.xml .
RUN mvn dependency:go-offline

# 5. Copy the source code and build the application
COPY src ./src
RUN mvn clean package -DskipTests

# 6. Grant the non-root user ownership of the app directory
RUN chown -R appuser:appuser /app

# 7. Switch to the non-root user (Everything below this runs securely)
USER appuser

# 8. Expose port 8080 so we can access it from our browser
EXPOSE 8080

# 9. Run the application
CMD ["java", "-jar", "target/demo-0.0.1-SNAPSHOT.jar"]

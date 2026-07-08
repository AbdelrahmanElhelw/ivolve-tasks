# 1. Use Maven base image with Java 17
FROM maven:3.9-eclipse-temurin-17-alpine

# 2. Create work directory inside the container
WORKDIR /app

RUN adduser -D appuser

# 3. Copy the application source code into the container
# (The first '.' is your local folder, the second '.' is the /app folder in the container)
COPY pom.xml .
RUN mvn dependency:go-offline

COPY src ./src

# 4. Build the app using mvn package
# (We add -DskipTests just to make the build faster during container creation, but 'RUN mvn package' works too)
RUN mvn clean package -DskipTests

# 5. Expose port 8080 so we can access it from our browser
EXPOSE 8080

USER appuser

# 6. Run the app on jar file
CMD ["java", "-jar", "target/demo-0.0.1-SNAPSHOT.jar"]

# 🐳 Advanced Docker Optimization: Multi-Stage Builds (Lab 05)

This project demonstrates the industry-standard DevOps pattern for containerizing compiled applications: **Multi-Stage Builds**. 

By utilizing multiple `FROM` statements in a single Dockerfile, we automate the entire Maven build process inside Docker, but only package the final compiled artifact into the lightweight runtime image. This eliminates the need for developers to have Maven installed locally while keeping the production image extremely small and secure.

---

## 🏗️ Architecture & Best Practices Implemented

* **Stage 1 (The Builder):** Uses a heavy `maven` Alpine image to download dependencies and compile the source code.
* **Stage 2 (The Runtime):** Uses a lightweight `amazoncorretto` (Java 17) Alpine image. It copies *only* the compiled `.jar` file from Stage 1, leaving all Maven tools, source code, and cached downloads behind.
* **Zero Local Dependencies:** Because the build happens inside Docker, this project can be built on any machine that has Docker installed, ensuring 100% environment consistency.

---

## Step 1: The Multi-Stage Dockerfile

Create a `Dockerfile` in the root directory. Notice the `AS build` alias and the `--from=build` flag that connects the two stages:

```dockerfile
# === STAGE 1: Build the Application ===
FROM maven:3.9.16-eclipse-temurin-17-alpine AS build
WORKDIR /app1
# Copy source code and build the artifact
COPY . .
RUN mvn package

# === STAGE 2: Lightweight Runtime ===
FROM amazoncorretto:17-alpine3.21-full
WORKDIR /app
# Extract ONLY the compiled .jar from the "build" stage above
COPY --from=build /app1/target/demo-0.0.1-SNAPSHOT.jar .

# Define startup command and expose port
CMD ["java", "-jar", "demo-0.0.1-SNAPSHOT.jar"]
EXPOSE 8080

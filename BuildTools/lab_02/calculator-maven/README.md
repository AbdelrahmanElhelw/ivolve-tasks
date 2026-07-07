# 🛠️ Building and Packaging a Java Application with Maven

This project demonstrates the core lifecycle of a Java application using **Apache Maven**. It walks through installing the build tool, cloning the source code, running automated unit tests, and compiling the application into a distributable Java Archive (`.jar`).

---

## Step 1: Install Apache Maven

Refresh your local package index and install Apache Maven from the official Ubuntu repositories:

```bash
sudo apt update && sudo apt install maven -y
```

Verify the installation and check the active version:

```bash
mvn -version
```

<img width="978" height="137" alt="mavin_installation" src="https://github.com/user-attachments/assets/acf4cb1b-b021-4535-9ad6-fa037f7a2a49" />

---

## Step 2: Clone the Source Code

Navigate to your training workspace directory and clone the project repository using Git:

```bash
# Clone the repository
git clone [https://github.com/Ibrahim-Adel15/calculator-maven.git](https://github.com/Ibrahim-Adel15/calculator-maven.git)

# Move into the project directory
cd calculator-maven

# View the project directory tree
tree
```

---

## Step 3: Run Automated Unit Tests

In Maven, testing is a dedicated phase in the default build lifecycle. When you trigger the `test` phase, Maven compiles the source code and executes all unit tests using the **Maven Surefire Plugin**.

```bash
mvn test
```

<img width="1032" height="320" alt="test" src="https://github.com/user-attachments/assets/d53d1613-5770-4899-8364-6e47603a51dd" />

---

## Step 4: Build the Application (Generate Artifact)

To package the compiled code into a distributable Java Archive (`.jar`), run the `package` command. 

> **💡 DevOps Pro-Tip:** Prepending the `clean` command ensures you wipe out the old `target/` directory and remove any corrupted artifacts from previous builds before generating a clean new one.

```bash
mvn clean package
```

Verify that the artifact was generated successfully inside the `target/` directory:

```bash
ll target/
```

<img width="1007" height="275" alt="build_artifacts" src="https://github.com/user-attachments/assets/569c638c-c9d0-4bd3-896a-617bce388209" />

---

## Step 5: Run and Verify the Application

Now that the application is cleanly packaged into a standard JAR file, execute it using the Java Virtual Machine (`java` command):

```bash
java -jar target/calculator.jar
```

<img width="1158" height="166" alt="Run verify" src="https://github.com/user-attachments/assets/e3e612d8-537b-479e-9d7a-05f2045b9c8e" />

---

### ✅ Verification Complete
The application executes seamlessly in a headless Linux environment, confirming that all dependencies and compiled bytecode are properly bundled inside the artifact!

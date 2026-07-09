FROM maven:3.9.16-eclipse-temurin-17-alpine AS build
WORKDIR /app1
COPY . .
RUN mvn package 

FROM amazoncorretto:17-alpine3.21-full
WORKDIR /app
COPY --from=build /app1/target/demo-0.0.1-SNAPSHOT.jar .
CMD ["java","-jar","demo-0.0.1-SNAPSHOT.jar"]
EXPOSE 8080

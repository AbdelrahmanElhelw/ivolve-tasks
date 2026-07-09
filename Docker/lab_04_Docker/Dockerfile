FROM amazoncorretto:17-alpine3.23
WORKDIR /app1
RUN adduser -D appuser
USER appuser
COPY  target/demo-0.0.1-SNAPSHOT.jar .
USER appuser
CMD ["java", "-jar", "demo-0.0.1-SNAPSHOT.jar"]
EXPOSE 8080
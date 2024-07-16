# Dockerfile para email-service
# Stage 1: Build
FROM maven:3.9-eclipse-temurin-21-alpine AS build
WORKDIR /app
COPY email/pom.xml /app
COPY email/src /app/src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:21-jre-alpine
COPY --from=build /app/target/email-0.0.1-SNAPSHOT.jar /email.jar
EXPOSE 8082
ENTRYPOINT ["java", "-jar", "/email.jar"]
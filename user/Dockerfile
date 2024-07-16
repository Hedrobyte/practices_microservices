# Dockerfile para user-service
# Stage 1: Build
FROM maven:3.9-eclipse-temurin-21-alpine AS build
WORKDIR /app
COPY user/pom.xml /app
COPY user/src /app/src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:21-jre-alpine
COPY --from=build /app/target/user-0.0.1-SNAPSHOT.jar /user.jar
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "/user.jar"]

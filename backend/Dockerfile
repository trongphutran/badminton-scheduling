# 1: Build stage
FROM maven:3.9.6-eclipse-temurin-21 AS builder

WORKDIR /app

# copy dependencies
COPY pom.xml .

# download libraries
RUN mvn dependency:go-offline

# copy source code
COPY src ./src

# build JAR file (skip testing step, it will test in CI)
RUN mvn clean package -DskipTests

# 2: Run stage
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Copy file .jar from stage 1 to stage 2, change the file name to app.jar
COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8080

# run project
ENTRYPOINT ["java", "-jar", "app.jar"]

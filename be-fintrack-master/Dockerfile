# build stage
# Use an official OpenJDK runtime as a parent image
FROM maven:3.9.11-eclipse-temurin-17 AS build

# Set working directory
WORKDIR /app

# Copy the project files
COPY . .

# Build the application with Maven, using a mirror to avoid connection issues
RUN mvn clean package -DskipTests -Dmaven.wagon.http.ssl.insecure=true -Dmaven.central.mirror=https://maven.aliyun.com/repository/public

# Run stage
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/be-fintrack-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]

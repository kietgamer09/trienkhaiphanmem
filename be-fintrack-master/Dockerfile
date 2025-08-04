FROM maven:3.9.11-eclipse-temurin-17 AS build

WORKDIR /app
COPY . .
COPY settings.xml /root/.m2/
RUN rm -rf /root/.m2/repository
RUN mvn clean package -DskipTests -Dmaven.wagon.http.ssl.insecure=true -DrepositoryUrl=https://maven.aliyun.com/repository/public

FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/be-fintrack-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]

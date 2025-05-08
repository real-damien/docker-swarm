FROM openjdk:8-jdk-alpine AS build
WORKDIR /app

COPY .mvn/ .mvn/
COPY mvnw .
COPY pom.xml .
RUN ./mvnw dependency:go-offline
COPY src/ src/
RUN ./mvnw clean package -DskipTests

FROM openjdk:8-jdk-alpine
WORKDIR /app

RUN apk add --no-cache bash

RUN apk add --no-cache bash

COPY --from=build /app/target/payment-session-0.0.1-SNAPSHOT.jar app.jar

COPY wait-for-it.sh /app/wait-for-it.sh
RUN chmod +x /app/wait-for-it.sh

EXPOSE 8084

ENTRYPOINT ["/app/wait-for-it.sh", "postgres:5432", "--", "java", "-jar", "app.jar"]
FROM openjdk:17-slim

WORKDIR /app

COPY target/gateway-service-1.0-SNAPSHOT.jar /app/

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "gateway-service-1.0-SNAPSHOT.jar", "--spring.profiles.active=prod"]

FROM openjdk:17-jdk-slim

ADD gateway-service-1.0-SNAPSHOT.jar /app/gateway.jar

EXPOSE 8080

CMD ["java", "-jar", "/app/gateway.jar", "--spring.profiles.active=prod"]

FROM openjdk:17-jdk-slim

ADD user-service-1.0-SNAPSHOT-encrypted.jar /app/user-service.jar

EXPOSE 8081

CMD ["java", "-javaagent:/app/user-service.jar", "-jar", "/app/user-service.jar", "--spring.profiles.active=prod"]

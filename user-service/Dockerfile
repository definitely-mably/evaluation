FROM openjdk:17-slim

WORKDIR /app

COPY target/user-service-1.0-SNAPSHOT-encrypted.jar /app/

EXPOSE 8081

ENTRYPOINT ["java", "-javaagent:user-service-1.0-SNAPSHOT-encrypted.jar", "-jar", "user-service-1.0-SNAPSHOT-encrypted.jar", "--spring.profiles.active=prod"]

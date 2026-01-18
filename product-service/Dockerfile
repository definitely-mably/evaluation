FROM openjdk:17-jdk-slim

ADD product-service-1.0-SNAPSHOT-encrypted.jar /app/product-service.jar

EXPOSE 8082

CMD ["java", "-javaagent:/app/product-service.jar", "-jar", "/app/product-service.jar", "--spring.profiles.active=prod"]

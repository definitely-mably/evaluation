FROM openjdk:17-slim

WORKDIR /app

COPY target/product-service-1.0-SNAPSHOT-encrypted.jar /app/

EXPOSE 8082

ENTRYPOINT ["java", "-javaagent:product-service-1.0-SNAPSHOT-encrypted.jar", "-jar", "product-service-1.0-SNAPSHOT-encrypted.jar", "--spring.profiles.active=prod"]

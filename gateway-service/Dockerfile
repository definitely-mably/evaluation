FROM openjdk:17-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && apk update && apk add busybox-extras

ADD gateway-service-1.0-SNAPSHOT.jar /app/gateway.jar

EXPOSE 8080

CMD ["java", "-jar", "/app/gateway.jar", "--spring.profiles.active=prod"]

FROM registry.cn-hangzhou.aliyuncs.com/library/openjdk:17-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && apk update && apk add busybox-extras

ADD user-service-1.0-SNAPSHOT-encrypted.jar /app/user-service.jar

EXPOSE 8081

CMD ["java", "-javaagent:/app/user-service.jar", "-jar", "/app/user-service.jar", "--spring.profiles.active=prod"]

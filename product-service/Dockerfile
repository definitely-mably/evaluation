FROM registry.cn-hangzhou.aliyuncs.com/library/openjdk:17-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && apk update && apk add busybox-extras

ADD product-service-1.0-SNAPSHOT-encrypted.jar /app/product-service.jar

EXPOSE 8082

CMD ["java", "-javaagent:/app/product-service.jar", "-jar", "/app/product-service.jar", "--spring.profiles.active=prod"]

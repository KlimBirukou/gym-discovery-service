FROM gradle:9-jdk21-corretto AS builder

WORKDIR /build

COPY gradlew .
COPY gradle gradle
COPY build.gradle settings.gradle ./

RUN ./gradlew dependencies --no-daemon

COPY src src

RUN ./gradlew bootJar -x test --no-daemon

FROM amazoncorretto:21-al2023

WORKDIR /app

COPY --from=builder /build/build/libs/gym-discovery-service-*.jar app.jar

EXPOSE 8761

HEALTHCHECK --interval=10s --timeout=3s --start-period=30s --retries=5 \
  CMD curl -f http://localhost:8761/actuator/health || exit 1

ENTRYPOINT ["java", "-jar", "app.jar"]

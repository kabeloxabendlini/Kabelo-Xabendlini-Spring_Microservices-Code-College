# Stage 1: Unpack the Spring Boot application
FROM eclipse-temurin:11-jdk-jammy AS build

LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

ARG JAR_FILE
COPY ${JAR_FILE} app.jar

RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)

# Stage 2: Run the application
FROM eclipse-temurin:11-jdk-jammy

VOLUME /tmp

ARG DEPENDENCY=/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

ENTRYPOINT ["java","-cp","app:app/lib/*","com.optimagrowth.organization.OrganizationServiceApplication"]

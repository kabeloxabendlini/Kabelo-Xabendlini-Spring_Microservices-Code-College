# Stage 1
# Start with a base image containing Java 11 JDK
FROM eclipse-temurin:11-jdk-jammy AS build

# Add Maintainer Info
LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

# The application's JAR file
ARG JAR_FILE

# Add the application's JAR to the container
COPY ${JAR_FILE} app.jar

# Unpackage the JAR file
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)

# Stage 2
# Use a lightweight Java 11 runtime image
FROM eclipse-temurin:11-jre-jammy

# Add volume pointing to /tmp
VOLUME /tmp

# Copy unpackaged application to the new container
ARG DEPENDENCY=/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

# Execute the application
ENTRYPOINT ["java","-cp","app:app/lib/*","com.optimagrowth.license.LicenseServiceApplication"]

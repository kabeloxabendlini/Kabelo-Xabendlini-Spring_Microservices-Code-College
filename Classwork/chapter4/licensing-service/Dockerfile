# Stage 1: Extract the Spring Boot application
# Start with a Java 11 image containing the JDK tools
FROM eclipse-temurin:11-jdk AS build

# Add Maintainer Info
LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

# The application's JAR file
ARG JAR_FILE

# Add the application's JAR to the container
COPY ${JAR_FILE} app.jar

# Unpackage the executable Spring Boot JAR
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)

# Stage 2: Run the application
# Use a smaller Java 11 runtime image
FROM eclipse-temurin:11-jre

# Add volume pointing to /tmp
VOLUME /tmp

# Copy the unpackaged application into the runtime container
ARG DEPENDENCY=/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

# Execute the application
ENTRYPOINT ["java","-cp","app:app/lib/*","com.optimagrowth.license.LicenseServiceApplication"]

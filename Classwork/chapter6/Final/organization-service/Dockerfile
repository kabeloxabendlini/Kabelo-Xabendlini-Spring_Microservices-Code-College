# Stage 1: Build the application image
FROM eclipse-temurin:17-jdk-alpine AS build

# Add maintainer information
LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

# The application's JAR file
ARG JAR_FILE

# Copy the application JAR into the build image
COPY ${JAR_FILE} app.jar

# Extract the application JAR
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)

# Stage 2: Run the application
FROM eclipse-temurin:17-jdk-alpine

# Add a volume pointing to /tmp
VOLUME /tmp

# Copy the extracted application files
ARG DEPENDENCY=/target/dependency

COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

# Open Java modules required by the older Eureka/XStream dependency
ENTRYPOINT ["java", "--add-opens=java.base/java.util=ALL-UNNAMED", "--add-opens=java.base/java.lang.reflect=ALL-UNNAMED", "--add-opens=java.base/java.text=ALL-UNNAMED", "--add-opens=java.desktop/java.awt.font=ALL-UNNAMED", "-cp", "app:app/lib/*", "com.optimagrowth.organization.OrganizationServiceApplication"]

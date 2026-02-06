# Use Maven with OpenJDK 21 for building the application.
# This is the "build stage" of a multi-stage Docker build.
FROM maven:3.9.9-eclipse-temurin-21 AS build

# Set the working directory inside the container to /app.
WORKDIR /app

# Copy only the Maven configuration file (pom.xml) to the container.
COPY pom.xml .

# Download all dependencies defined in pom.xml.
# This step is cached unless pom.xml changes, speeding up rebuilds.
RUN mvn dependency:go-offline -B

# Copy the source code into the container.
COPY src ./src

# Build the application, skipping tests for faster build.
RUN mvn clean package -DskipTests

# ---- Runtime Stage ----

# Use a lightweight Java 21 JRE image for running the app (smaller and faster than full JDK).
FROM eclipse-temurin:21-jre-jammy

# Set the working directory inside the runtime container.
WORKDIR /app

# Copy the built JAR file from the build stage into the runtime container.
COPY --from=build /app/target/backend-0.0.1-SNAPSHOT.jar app.jar

# Expose port 8080 (the default Spring Boot port).
EXPOSE 8080

# Command to run the Spring Boot application.
CMD ["java", "-jar", "app.jar"]
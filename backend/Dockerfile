# ==================== Stage 1: Build ====================
FROM gradle:8-jdk21 AS build
WORKDIR /workspace

# Copy build config files first for caching
COPY build.gradle settings.gradle gradlew ./
COPY gradle/ gradle/

# Copy source code
COPY src/ src/

# Build the JAR (skip tests since they were run separately)
RUN ./gradlew bootJar -x test --no-daemon

# ==================== Stage 2: Runtime ====================
FROM eclipse-temurin:21-jre-jammy
WORKDIR /app

COPY --from=build /workspace/build/libs/*.jar /app/app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app/app.jar"]

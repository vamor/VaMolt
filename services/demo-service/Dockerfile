# ---- Build stage ----
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# 1) Parent POM exact op het pad waar relativePath naar wijst (/pom.xml)
COPY pom.xml /pom.xml
RUN --mount=type=cache,target=/root/.m2 mvn -B -q -N -f /pom.xml -DskipTests install

# 2) Service code
COPY services/demo-service/pom.xml ./pom.xml
COPY services/demo-service/src ./src
# (Als je Maven Wrapper gebruikt:)
# COPY services/demo-service/.mvn ./.mvn
# COPY services/demo-service/mvnw ./mvnw

# 3) Build service (parent is nu zowel lokaal bestand als in ~/.m2 aanwezig)
RUN --mount=type=cache,target=/root/.m2 mvn -B -q -DskipTests package

# ---- Runtime stage ----
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/target/*.jar /app/app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
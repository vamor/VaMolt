# ---- Build stage ----
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# Kopieer alleen wat nodig is uit de service-map
COPY pom.xml /app/parent.pom
RUN --mount=type=cache,target=/root/.m2 mvn -B -q -N -f /app/parent.pom -DskipTests install

COPY services/demo-service/pom.xml ./pom.xml
COPY services/demo-service/src ./src
# (Als je wrapper gebruikt:)
# COPY services/demo-service/.mvn ./.mvn
# COPY services/demo-service/mvnw ./mvnw

# Bouw (skip tests in image build)
RUN --mount=type=cache,target=/root/.m2 mvn -B -q -DskipTests package

# ---- Runtime stage ----
FROM eclipse-temurin:21-jre
WORKDIR /app
# Pak de jar (ongeacht naam) uit de build-stage
COPY --from=build /app/target/*.jar /app/app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
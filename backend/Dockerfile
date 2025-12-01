# --- Build-Stage: Maven kompiliert das Projekt ---
    FROM maven:3.9-eclipse-temurin-21 AS build
    WORKDIR /app
    
    # pom.xml kopieren und Dependencies vorladen (Cache)
    COPY pom.xml .
    RUN mvn -q dependency:go-offline
    
    # Restlichen Source-Code kopieren und bauen
    COPY src ./src
    RUN mvn clean package -DskipTests
    
    # --- Runtime-Stage: leichtgewichtiger Java-Container ---
    FROM eclipse-temurin:21-jre
    WORKDIR /app
    
    # JAR aus der Build-Stage kopieren
    COPY --from=build /app/target/*.jar app.jar
    
    # Standard-ENV-Variablen (können beim Start überschrieben werden)
    ENV MYSQL_HOST=db
    ENV MYSQL_PORT=3306
    ENV MYSQL_DB=kochbuch
    ENV MYSQL_USER=appuser
    ENV MYSQL_PASSWORD=secret
    
    # Spring Boot läuft standardmässig auf Port 8080
    EXPOSE 8080
    
    # Startkommando
    ENTRYPOINT ["java", "-jar", "app.jar"]
    
# Dockerfile para la aplicación Java

# Usa una imagen base de Java 17. Cambia la versión si tu nuevo proyecto usa otra.
FROM eclipse-temurin:17-jdk-jammy

# Define un directorio de trabajo dentro del contenedor
ENV APP_HOME=/app
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME

# Expone el puerto que la aplicación usará dentro del contenedor
EXPOSE 8084

# Copia el archivo JAR compilado de tu proyecto.
# El comodín '*' permite que funcione sin importar la versión exacta del JAR.
# Asegúrate de que tu proyecto genere el JAR en la carpeta 'target/'.
COPY target/*.jar app.jar

# Crea un usuario no-root por seguridad, para que la app no se ejecute como administrador.
RUN useradd -m appuser && chown -R appuser:appuser $APP_HOME
USER appuser

# (Opcional) Health check para saber si la aplicación está saludable.
# Funciona genial si tu app es Spring Boot con Actuator. Si no, puedes quitarlo.
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

# El comando para iniciar la aplicación cuando se levante el contenedor.
ENTRYPOINT ["java", "-jar", "app.jar"]
# Usa una imagen base con JRE 17 en Alpine
FROM eclipse-temurin:17-alpine

# Instalar dependencias necesarias para descargar e instalar dockerize
RUN apk add --no-cache wget tar

# Descargar y extraer dockerize
RUN wget https://github.com/jwilder/dockerize/releases/download/v0.6.1/dockerize-linux-amd64-v0.6.1.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-v0.6.1.tar.gz \
    && rm dockerize-linux-amd64-v0.6.1.tar.gz

# Agrega un grupo 'devopsc'
RUN addgroup devopsc

# Agrega un usuario 'admin' y lo asigna al grupo 'devopsc'
RUN adduser -D -G devopsc admin

# Create a new mount point at /tmp on native host because a volume is more efficient and faster than filesystem
VOLUME /tmp

# Copiamos el JAR a la imagen
ARG JAR_FILE

# Establece una variable de entorno para la constraseña de la base de datos
ARG DB_PASSWORD

# Establece la variable de entorno DB_PASSWORD con el valor del argumento
ENV DB_PASSWORD=$DB_PASSWORD

# Copia el archivo JAR
COPY ${JAR_FILE} /tmp/app.jar

# Change ownership of the /app directory to the "admin" user
RUN chown -R admin:devopsc /tmp

# Cambia al usuario 'admin'
USER admin

# Comando por defecto al ejecutar el contenedor
# Usamos dockerize para esperar que el servicio de Postgres esté listo antes de iniciar la aplicación
CMD ["dockerize", "-wait", "tcp://postgres_db:5432", "-timeout", "30s", "java", "-jar", "/tmp/app.jar"]

Práctica: Pipeline de Integración Continua (CI/CD) con Git, Docker, Jenkins y SonarQube



Estudiante: Aless

Código: U202215537

Repositorio base: https://github.com/upc-is-si732/deisw-retail-store



Entregables



URL del fork en GitHub (con Dockerfile y Jenkinsfile): https://github.com/Aless7666/deisw-retail-store

URL del repositorio público en Docker Hub: https://hub.docker.com/r/aless2212/retail-store-u202215537



Desarrollo



Fork del repositorio: se hizo un fork del repositorio base a la cuenta personal de GitHub y se clonó localmente.

Infraestructura como código: el proyecto usa Maven y Java 26 (Spring Boot 4.0.6). Se implementó el Dockerfile en dos etapas: compilación con el JDK 26 y ejecución con el JRE 26 sobre el .jar generado. Se implementó el Jenkinsfile con las etapas Build, SonarQube Analysis, Quality Gate y Docker Build \& Push. La imagen se nombra aless2212/retail-store-u202215537, en minúsculas, con los tags del número de build y latest.

Jenkins: se levantó en un contenedor con una imagen propia que incluye el cliente de Docker, conectado a la red devops-net. Se configuraron las credenciales sonar-token (token de SonarQube) y dockerhub-creds (Docker Hub), y el servidor SonarQube con el nombre SonarQube. Se creó el proyecto Pipeline apuntando al fork.

SonarQube: se levantó en un contenedor, se creó el proyecto con su clave y se configuró el webhook hacia http://jenkins:8080/sonarqube-webhook/ para recibir el resultado del Quality Gate.

Distribución: tras superar el Quality Gate, el pipeline construyó y subió la imagen a Docker Hub. La ejecución #9 terminó en verde.

Verificación: se descargó la imagen desde Docker Hub y se ejecutó en un contenedor conectado a una base de datos PostgreSQL 16, en la red devops-net. La API responde en Swagger.



Problemas encontrados y solución



Permiso de ejecución de mvnw: Git no guardaba el permiso de ejecución. Se corrigió con git update-index --chmod=+x mvnw.

Conexión entre contenedores: los contenedores del agente Docker no estaban en la red de Jenkins y SonarQube. Se agregó --network devops-net a los agentes.

Clave del proyecto en SonarQube: no coincidía con la del Jenkinsfile. Se corrigió la variable SONAR\_PROJECT\_KEY.

Webhook bloqueado: SonarQube no permitía webhooks a direcciones privadas. Se desactivó Enable local webhooks validation (solo para el entorno de laboratorio).

Puerto de la aplicación: la aplicación escucha en el puerto 8096, no en el 8080. Se mapeó 8082 del host al 8096 del contenedor.

Base de datos: la aplicación necesitaba PostgreSQL. Se levantó un contenedor postgres:16 en la misma red y se pasaron las variables DB\_HOST, DB\_USERNAME y DB\_PASSWORD.



Capturas



docker images con la imagen retail-store-u202215537.

docker ps con retail-store-app y postgres-db en ejecución.

SonarQube: análisis del proyecto (Passed).

SonarQube: webhook jenkins con entrega exitosa.

Jenkins: Stage View de la ejecución #9 en verde.

Docker Hub: repositorio con descripción e imagen.

Swagger de la API en http://localhost:8082/swagger-ui/index.html.


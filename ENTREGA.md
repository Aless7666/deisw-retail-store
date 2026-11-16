\# Práctica: Pipeline de Integración Continua (CI/CD) con Git, Docker, Jenkins y SonarQube



\*\*Estudiante:\*\* Aless  

\*\*Código:\*\* U202215537  

\*\*Repositorio base:\*\* https://github.com/upc-is-si732/deisw-retail-store



\## Entregables



\- \*\*URL del fork en GitHub\*\* (con Dockerfile y Jenkinsfile): https://github.com/Aless7666/deisw-retail-store

\- \*\*URL del repositorio público en Docker Hub:\*\* https://hub.docker.com/r/aless2212/retail-store-u202215537



\## Desarrollo



1\. \*\*Fork del repositorio:\*\* se hizo un fork del repositorio base a la cuenta personal de GitHub y se clonó localmente.

2\. \*\*Infraestructura como código:\*\* el proyecto usa Maven y Java 26 (Spring Boot 4.0.6). Se implementó el Dockerfile en dos etapas: compilación con el JDK 26 y ejecución con el JRE 26 sobre el `.jar` generado. Se implementó el Jenkinsfile con las etapas \*Build\*, \*SonarQube Analysis\*, \*Quality Gate\* y \*Docker Build \& Push\*. La imagen se nombra `aless2212/retail-store-u202215537`, en minúsculas, con los tags del número de build y `latest`.

3\. \*\*Jenkins:\*\* se levantó en un contenedor con una imagen propia que incluye el cliente de Docker, conectado a la red `devops-net`. Se configuraron las credenciales `sonar-token` (token de SonarQube) y `dockerhub-creds` (Docker Hub), y el servidor SonarQube con el nombre `SonarQube`. Se creó el proyecto Pipeline apuntando al fork.

4\. \*\*SonarQube:\*\* se levantó en un contenedor, se creó el proyecto con su clave y se configuró el webhook hacia `http://jenkins:8080/sonarqube-webhook/` para recibir el resultado del Quality Gate.

5\. \*\*Distribución:\*\* tras superar el Quality Gate, el pipeline construyó y subió la imagen a Docker Hub. La ejecución #9 terminó en verde.

6\. \*\*Verificación:\*\* se descargó la imagen desde Docker Hub y se ejecutó en un contenedor conectado


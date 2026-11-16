pipeline {
    agent any

    environment {
        DOCKERHUB_USER   = 'aless2212'
        CODIGO           = 'U202215537'
        SONAR_PROJECT_KEY = 'retail-storeretail-store-U202215537'
        IMAGE            = "${DOCKERHUB_USER}/retail-store-${CODIGO.toLowerCase()}"
        TAG              = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'eclipse-temurin:26-jdk'
                    args '-u root'
                }
            }
            steps {
                sh 'chmod +x mvnw && ./mvnw -B -DskipTests clean package'
            }
        }

        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'eclipse-temurin:26-jdk'
                    args '-u root --network devops-net'
                }
            }
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'chmod +x mvnw && ./mvnw -B -DskipTests sonar:sonar -Dsonar.projectKey=$SONAR_PROJECT_KEY -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.token=$SONAR_AUTH_TOKEN'
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DU', passwordVariable: 'DP')]) {
                    sh 'echo "$DP" | docker login -u "$DU" --password-stdin'
                    sh "docker build -t ${IMAGE}:${TAG} -t ${IMAGE}:latest ."
                    sh "docker push ${IMAGE}:${TAG}"
                    sh "docker push ${IMAGE}:latest"
                }
            }
        }
    }
}
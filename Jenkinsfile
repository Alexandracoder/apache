pipeline {
    agent any

    environment {
        DOCKER_IMAGE =apache-php-docker
        TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "test_apache_container"
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-token', url: 'https://github.com/Alexandracoder/apache.git'

        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${TAG} ."
                }
            }
        }

        stage('Test Docker Image') {
            steps {
                script {
                    // Ejecutar contenedor en segundo plano
                    sh "docker run -d --rm --name ${CONTAINER_NAME} -p 8080:8080 ${DOCKER_IMAGE}:${TAG}"
                    
                    // Esperar unos segundos a que Apache levante
                    sh "sleep 5"

                    // Probar si el servidor responde
                    sh "curl -f http://localhost:8080"

                    // Detener contenedor
                    sh "docker stop ${CONTAINER_NAME}"
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    sh "docker tag ${DOCKER_IMAGE}:${TAG} ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds-id') {
                        sh "docker push ${DOCKER_IMAGE}:${TAG}"
                        sh "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }
    }
}

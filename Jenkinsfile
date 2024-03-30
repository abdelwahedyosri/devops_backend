pipeline {
    agent any

    stages {
        stage('Start Docker Services') {
            steps {
                script {
                    // Start Docker services using Docker Compose
                    sh "docker-compose up -d"
                }
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    docker.build("yosriabdelwahed/Devops_Project:latest")

                    // Push Docker image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("yosriabdelwahed/Devops_Project:latest").push()
                    }
                }
            }
        }
        stage('Stop Docker Services') {
            steps {
                script {
                    // Stop Docker services using Docker Compose
                    sh "docker-compose down"
                }
            }
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Execute Commands in Docker Containers') {
            steps {
                script {
                    // Execute commands in the SonarQube container
                    sh "docker exec sonarqube ls -la"

                    // Execute commands in the Prometheus container
                    sh "docker exec prometheus ps aux"

                    // Execute commands in the Grafana container
                    sh "docker exec grafana cat /etc/hosts"

                    // Execute commands in the Nexus container
                    sh "docker exec nexus df -h"
                }
            }
        }
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image (if needed)
                    docker.build("yosriabdelwahed/Devops_Project:latest")

                    // Push Docker image to Docker Hub (if needed)
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("yosriabdelwahed/Devops_Project:latest").push()
                    }
                }
            }
        }
    }
}
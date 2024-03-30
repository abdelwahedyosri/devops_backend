pipeline {
    agent any

    stages {
        stage('Stop and Remove Existing Containers') {
            steps {
                script {
                    // Stop and remove existing containers
                    docker.stopAll()
                    docker.remove(container: 'sonarqube', force: true)
                    docker.remove(container: 'prometheus', force: true)
                    docker.remove(container: 'grafana', force: true)
                    docker.remove(container: 'nexus', force: true)
                }
            }
        }
        stage('Start SonarQube') {
            steps {
                script {
                    docker.image('sonarqube:latest').run('-d -p 9000:9000 --name sonarqube')
                }
            }
        }
        stage('Start Prometheus') {
            steps {
                script {
                    docker.image('prom/prometheus:latest').run('-d -p 9090:9090 --name prometheus')
                }
            }
        }
        stage('Start Grafana') {
            steps {
                script {
                    docker.image('grafana/grafana:latest').run('-d -p 3000:3000 --name grafana')
                }
            }
        }
        stage('Start Nexus') {
            steps {
                script {
                    docker.image('sonatype/nexus3:latest').run('-d -p 8081:8081 --name nexus')
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
    }
}

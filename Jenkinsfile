pipeline {
    agent any

    environment {
        SONARQUBE_JDBC_USERNAME = 'sonar'
        SONARQUBE_JDBC_PASSWORD = 'sonar'
    }

    stages {
        stage('Run Containers') {
            steps {
                script {
                    // Check if the Docker images are running
                    sh 'docker ps -a'
                    
                    // Run the Docker containers using docker-compose
                    sh 'docker-compose up -d'
                }
            }
        }
    }
    
    post {
        always {
            // Stop and remove the Docker containers
            sh 'docker-compose down'
        }
    }
}
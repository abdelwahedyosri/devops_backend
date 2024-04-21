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
                    
                    // Start the containers using Docker Compose
                    sh 'docker-compose -f docker-compose.yml up -d'
                }
            }
        }
    }

    post {
        always {
            // Stop and remove containers after build
            script {
                sh 'docker-compose -f docker-compose.yml down'
            }
        }
    }
}

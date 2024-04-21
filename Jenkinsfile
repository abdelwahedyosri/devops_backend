pipeline {
    agent any

    environment {
        SONARQUBE_JDBC_USERNAME = 'sonar'
        SONARQUBE_JDBC_PASSWORD = 'sonar'
    }
     tools {
        // Define the SonarQube Scanner installation
        // You should use the name you provided during installation
        sonarqubeScanner 'SonarQubeScanner'
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
        
        stage('Build and analyze') {
            steps {
                 script {
                    withSonarQubeEnv('SonarQube') {
                        sh '''mvn clean package sonar:sonar \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=admin \
                        -Dsonar.password=admin'''
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Stop and remove containers after the build
            sh 'docker-compose down'
        }
    }
}

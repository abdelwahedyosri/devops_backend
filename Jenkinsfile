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
        
        stage('Build and analyze') {
            steps {
                script {
                    // Execute your build commands here
                    // For example: sh 'mvn clean install'

                    // Run SonarQube analysis
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar'
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

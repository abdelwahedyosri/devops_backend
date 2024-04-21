pipeline {
    agent any

    environment {
        SONARQUBE_JDBC_USERNAME = 'sonar'
        SONARQUBE_JDBC_PASSWORD = 'sonar'
    }

    stages {
        stage('Run Containers') {
            steps {
                // Run the Docker containers
                script {
                    sh 'docker-compose up -d'
                }
            }
        }
        stage('Build and analyze') {
            steps {
                 script {
                    withSonarQubeEnv('SonarQube') {
                        // Path to your source code
                        sh "sonar-scanner"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        SONARQUBE_JDBC_USERNAME = 'sonar'
        SONARQUBE_JDBC_PASSWORD = 'sonar'
    }

    tools {
        // Define SonarQube Scanner
        sonarqubeScanner 'sonar-scanner'
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
                // Execute the SonarQube scan
                script {
                    withSonarQubeEnv('sonar-server') {
                        sh '''sonar-scanner \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://sonar-server:9000 \
                        -Dsonar.login=admin \
                        -Dsonar.password=admin'''
                    }
                }
            }
        }
    }
}

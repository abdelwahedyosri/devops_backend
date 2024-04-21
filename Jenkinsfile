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
                // Execute the SonarQube scan
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh '''sonar-scanner \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=admin \
                        -Dsonar.password=admin'''
                    }
                }
            }
        }
    }
}

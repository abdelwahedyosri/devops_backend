pipeline {
    agent any
    tools {
            // Define SonarQube Scanner
            // Make sure to match the tool name with the one configured in Global Tool Configuration
            // Replace 'SonarQubeScanner' with the name you provided in Global Tool Configuration
            sonarqubeScanner 'sonar-scanner'
        }
    environment {
        SONARQUBE_HOST_URL = 'http://192.168.33.10:3000'
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
                         sh "${tool 'sonar-scanner'}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any

   

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

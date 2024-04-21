pipeline {
    agent any
    
    environment {
        SONARQUBE_JDBC_USERNAME = 'sonar'
        SONARQUBE_JDBC_PASSWORD = 'sonar'
    }
    
    stages {
        stage('Build and Run Containers') {
            steps {
                script {
                    // Create the network sonarnet if it does not exist
                    sh 'docker network ls | grep sonarnet || docker network create sonarnet'
                    
                    // Stop and remove existing mysql container
                    sh 'docker stop mysql || true'
                    sh 'docker rm mysql || true'
                    
                    // Run mysql container
                    docker.image("mysql:latest").run("-d --name mysql -p 3306:3306 --network sonarnet -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=sonar -e MYSQL_USER=sonar -e MYSQL_PASSWORD=sonar")
                    
                    // Stop and remove existing sonarqube container
                    sh 'docker stop sonarqube || true'
                    sh 'docker rm sonarqube || true'
                    
                    // Run sonarqube container
                    docker.image("sonarqube:latest").run("-d --name sonarqube -p 9000:9000 -p 9092:9092 --network sonarnet -e SONARQUBE_JDBC_URL=jdbc:mysql://mysql:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false&SONARQUBE_JDBC_USERNAME=${SONARQUBE_JDBC_USERNAME}&SONARQUBE_JDBC_PASSWORD=${SONARQUBE_JDBC_PASSWORD}")
                    
                    // Stop and remove existing grafana container
                    sh 'docker stop grafana || true'
                    sh 'docker rm grafana || true'
                    
                    // Run grafana container
                    docker.image("grafana/grafana:latest").run("-d --name grafana -p 3000:3000 --network sonarnet -e GF_SECURITY_ADMIN_PASSWORD=admin")
                }
            }
        }
    }
    
    post {
        always {
            script {
                // Stop and remove all containers
                sh 'docker stop sonarqube mysql grafana || true'
                sh 'docker rm sonarqube mysql grafana || true'
            }
        }
    }
}
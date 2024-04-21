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
                    docker.image("mysql:latest").run("-d --name mysql -p 3306:3306 --network sonarnet -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=sonar -e MYSQL_USER=sonar -e MYSQL_PASSWORD=sonar")
                    
                    docker.image("grafana/grafana:latest").run("-d --name grafana -p 3000:3000 --network sonarnet -e GF_SECURITY_ADMIN_PASSWORD=admin")
                }
            }
        }
    }
    
    post {
        always {
            script {
            
                
                docker.container('mysql').stop()
                docker.container('mysql').remove()
                
                docker.container('grafana').stop()
                docker.container('grafana').remove()
            }
        }
    }
}
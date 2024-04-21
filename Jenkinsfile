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
                    docker.image("sonarqube:latest").run("-d --name sonarqube -p 9000:9000 -p 9092:9092 --network sonarnet -e SONARQUBE_JDBC_URL=jdbc:mysql://mysql:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false -e SONARQUBE_JDBC_USERNAME=${SONARQUBE_JDBC_USERNAME} -e SONARQUBE_JDBC_PASSWORD=${SONARQUBE_JDBC_PASSWORD}")
                    
                    docker.image("mysql:latest").run("-d --name mysql -p 3306:3306 --network sonarnet -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=sonar -e MYSQL_USER=sonar -e MYSQL_PASSWORD=sonar")
                    
                    docker.image("grafana/grafana:latest").run("-d --name grafana -p 3000:3000 --network sonarnet -e GF_SECURITY_ADMIN_PASSWORD=admin")
                }
            }
        }
    }
    
    post {
        always {
            script {
                docker.container('sonarqube').stop()
                docker.container('sonarqube').remove()
                
                docker.container('mysql').stop()
                docker.container('mysql').remove()
                
                docker.container('grafana').stop()
                docker.container('grafana').remove()
            }
        }
    }
}
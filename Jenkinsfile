pipeline {
    agent any
    
    stages {
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('', '') {
                        docker.image("sonarqube").run("--rm -d -p 9000:9000 -p 9092:9092 --name sonarqube --network sonarnet -e SONARQUBE_JDBC_URL=jdbc:mysql://mysql:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false -e SONARQUBE_JDBC_USERNAME=sonar -e SONARQUBE_JDBC_PASSWORD=sonar sonarqube:latest")
                        docker.image("mysql").run("--rm -d -p 3306:3306 --name mysql --network sonarnet -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=sonar -e MYSQL_USER=sonar -e MYSQL_PASSWORD=sonar mysql:latest")
                        docker.image("prom/prometheus").run("--rm -d -p 9090:9090 --name prometheus --network sonarnet prom/prometheus:latest")
                        docker.image("grafana/grafana").run("--rm -d -p 3000:3000 --name grafana --network sonarnet -e GF_SECURITY_ADMIN_PASSWORD=admin grafana/grafana:latest")
                    }
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
                docker.container('prometheus').stop()
                docker.container('prometheus').remove()
                docker.container('grafana').stop()
                docker.container('grafana').remove()
            }
        }
    }
}

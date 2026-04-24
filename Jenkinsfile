pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                docker run --rm \
                  -v $(pwd):/app \
                  -w /app \
                  maven:3.9.9-eclipse-temurin-17 \
                  mvn clean package
                  '''
          }
      }

        stage('Run App (Tomcat)') {
            steps {
                sh '''
                docker rm -f foodapp || true
                docker run -d -p 8081:8080 --name foodapp tomcat:9
                sleep 10
                docker cp target/*.war foodapp:/usr/local/tomcat/webapps/
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'Food App Deployed Successfully 🍔'
        }
        failure {
            echo 'Deployment Failed ❌'
        }
    }
}

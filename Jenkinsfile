pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                 bat 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=java-cicd-demo -Dsonar.host.url=http://localhost:9000'
                }
             }
        }
        stage('Quality Gate') {
            steps {
                 timeout(time: 5, unit: 'MINUTES') {
                 waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t java-cicd-demo:v1 .'
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                docker rm -f java-app
                docker run -d -p 8082:8081 --name java-app java-cicd-demo:v1
                '''
            }
        }
    }
}
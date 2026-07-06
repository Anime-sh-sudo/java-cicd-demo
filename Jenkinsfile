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
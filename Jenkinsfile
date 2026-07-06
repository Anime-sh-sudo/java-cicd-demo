pipeline {
    agent any

    tools {
        maven 'maven'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building project using Maven...'
                bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                bat 'mvn test'
            }
        }
    }

    post {
        success {
            echo 'Build Successful!'
        }

        failure {
            echo 'Build Failed!'
        }

        stage('Docker Build'){
            steps{
                bat 'docker build -t java-cicd-demo:v1 .'
            }
        }
        stage('Deploy'){
            step{
                bat 'docker rm -f java-app || exit 0'
                bat 'docker run -d -p 8082:8081 --name java-app java-cicd-demo:v1'
            }
        }
    }
}
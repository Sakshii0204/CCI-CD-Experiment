pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t cicd-web-app:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker rm -f cicd-web-container 2>nul || exit /b 0'
                bat 'docker run -d -p 9090:80 --name cicd-web-container cicd-web-app:latest'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker ps'
            }
        }
    }
}
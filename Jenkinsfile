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

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    powershell '$env:DOCKER_PASS | docker login -u $env:DOCKER_USER --password-stdin'
                    bat 'docker tag cicd-web-app:latest %DOCKER_USER%/cicd-web-app:latest'
                    bat 'docker push %DOCKER_USER%/cicd-web-app:latest'
                }
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
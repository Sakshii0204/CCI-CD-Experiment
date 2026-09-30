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

        stage('Test DockerHub Credential') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-pat',
                    variable: 'DOCKER_PAT'
                )]) {
                    powershell '''
                        $bytes = [System.Text.Encoding]::UTF8.GetBytes($env:DOCKER_PAT)
                        $hash = [System.Security.Cryptography.SHA256]::Create().ComputeHash($bytes)
                        $hex = [BitConverter]::ToString($hash).Replace("-", "")
                        Write-Host "Credential length: $($env:DOCKER_PAT.Length)"
                        Write-Host "Credential SHA256: $hex"
                    '''
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
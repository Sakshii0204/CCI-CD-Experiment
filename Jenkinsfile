stage('Push to DockerHub') {
    steps {
        withCredentials([string(
            credentialsId: 'dockerhub-pat',
            variable: 'DOCKER_PAT'
        )]) {
            powershell '$env:DOCKER_PAT | docker login -u sakshipawar2004 --password-stdin'
            bat 'docker tag cicd-web-app:latest sakshipawar2004/cicd-web-app:latest'
            bat 'docker push sakshipawar2004/cicd-web-app:latest'
        }
    }
}
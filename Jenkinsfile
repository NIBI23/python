pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = 'dockerhub-credentials'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build('nibin23/dev')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    sh """
                    echo \$DOCKER_HUB_CREDENTIALS_PSW | docker login -u \$DOCKER_HUB_CREDENTIALS_USR --password-stdin https://index.docker.io/v1/
                    dockerImage.push('v2')
                    docker logout https://index.docker.io/v1/
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose up -d'
                    echo 'Deploying the application...'
                }
            }
        }
    }
}


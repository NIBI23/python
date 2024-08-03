pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS ='dockerhub-credentials'
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
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh """
                        echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USERNAME --password-stdin https://index.docker.io/v1/
                        dockerImage.push('v2')
                        docker logout https://index.docker.io/v1/
                        """
                    }
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


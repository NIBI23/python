pipeline {
    agent any
    stages {
	stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build nibin23/dev
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry https://hub.docker.com/repositories/nibin23', 'dockerhub-credentials') {
                        dockerImage.push("v2")
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

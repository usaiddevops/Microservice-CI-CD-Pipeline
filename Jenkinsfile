pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/your-app'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/usaiddevops/Microservice-CI-CD-Pipeline.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'usaidrehman/') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    dockerImage.run('-d -p 80:80')
                }
            }
        }
    }
    post {
        cleanup {
            sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
        }
    }
}

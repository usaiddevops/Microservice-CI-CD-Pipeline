pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'usaidrehman/my-app'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("$Microservice-CI-CD-Pipeline}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'usaidrehman') {
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

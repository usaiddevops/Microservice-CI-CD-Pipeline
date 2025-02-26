pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'usaidrehman/microservice-ci-cd-pipeline' // Updated Docker Hub repository name
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/usaiddevops/Microservice-CI-CD-Pipeline.git' // Updated GitHub repository link
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
                    docker.withRegistry('', 'docker-hub-credentials-id') {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    // Run Docker container using shell command instead of docker.run()
                    sh "docker run -d -p 80:80 ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
    post {
        always {
            // Cleanup to free space
            sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_NUMBER} || true"
        }
    }
}

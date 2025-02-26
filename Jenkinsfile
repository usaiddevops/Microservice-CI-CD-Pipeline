pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'usaidrehman/microservice-ci-cd-pipeline' // Your Docker Hub repository
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/usaiddevops/Microservice-CI-CD-Pipeline.git' // Your GitHub repository
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Enable BuildKit for faster & modern Docker builds
                    sh 'DOCKER_BUILDKIT=1 docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-credentials-id') {
                        sh "docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    sh "docker run -d -p 80:80 ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
    post {
        always {
            sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_NUMBER} || true"
        }
    }
}

pipeline {
    agent any
    environment {
        REPO = 'your-docker-repo/your-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
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
                    dockerImage = docker.build("${env.REPO}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', "${env.DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-credentials']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@your-ec2-instance << EOF
                    docker pull ${REPO}:${BUILD_NUMBER}
                    docker stop your-container || true
                    docker rm your-container || true
                    docker run -d --name your-container -p 80:80 ${REPO}:${BUILD_NUMBER}
                    EOF
                    '''
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

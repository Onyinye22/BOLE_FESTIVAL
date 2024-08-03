pipeline {
    agent any

    environment {
        DOCKER_HUB_TOKEN = credentials('docker-hub-token')
        DOCKER_IMAGE_NAME = "your-dockerhub-username/your-image-name"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Log in to Docker Hub') {
            steps {
                script {
                    sh "echo ${DOCKER_HUB_TOKEN} | docker login -u ${DOCKER_HUB_USER} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'docker logout'
            }
        }
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Failed to build or push the Docker image.'
        }
    }
}

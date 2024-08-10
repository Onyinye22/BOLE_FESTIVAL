pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('Docker_PAT')
        DOCKER_IMAGE = 'estheronyinye/bolefest'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository from GitHub
                git url: 'https://github.com/Goodyoma/BOLE_FESTIVAL.git', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the repository
                    docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    // Push the Docker image to DockerHub
                    docker.withRegistry('https://index.docker.io/v1/', 'DockerPat') {
                        docker.image("${env.DOCKER_IMAGE}:${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
    }
}


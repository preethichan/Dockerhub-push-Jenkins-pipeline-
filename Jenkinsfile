
ipipeline {
    agent any
    
    environment {
        DOCKER_HUB_REPO = 'preethichan/nginx-custom'
        DOCKER_HUB_CREDENTIALS = 'dockerhub-credentials-id'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:${IMAGE_TAG}")
                }
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                sh "docker rmi ${DOCKER_HUB_REPO}:${IMAGE_TAG}"
                sh "docker rmi ${DOCKER_HUB_REPO}:latest"
            }
        }
    }
    
    post {
        success {
            echo "Successfully built and pushed ${DOCKER_HUB_REPO}:${IMAGE_TAG}"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}

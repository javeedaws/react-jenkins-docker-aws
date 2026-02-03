pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-devops-demo:dev"
        CONTAINER_NAME = "react-devops-demo-dev"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out dev branch..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image for dev..."
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Deploying container on staging port 8080..."
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                docker run -d -p 8080:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Dev branch deployed successfully on staging!"
        }
        failure {
            echo "❌ Deployment failed. Check logs!"
        }
    }
}

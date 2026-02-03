pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-devops-demo:prod"
        CONTAINER_NAME = "react-devops-demo-prod"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out main branch..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image for production..."
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Deploying container on production port 80..."
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                docker run -d -p 80:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Main branch deployed successfully to production!"
        }
        failure {
            echo "❌ Deployment failed. Check logs!"
        }
    }
}

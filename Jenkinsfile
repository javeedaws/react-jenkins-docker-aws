pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('react-devops-demo:dev')
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                sh '''
                docker stop react-devops-demo || true
                docker rm react-devops-demo || true
                docker run -d -p 8080:80 --name react-devops-demo react-devops-demo:dev
                '''
            }
        }
    }
}

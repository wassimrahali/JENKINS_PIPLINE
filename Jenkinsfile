pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "wassimrahali/backend-8guess"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/wassimrahali/JENKINS_PIPLINE.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                // Example for Node.js
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Container (Deploy)') {
            steps {
                echo 'Deploying app...'
                sh '''
                docker rm -f app || true
                docker run -d -p 3000:3000 --name app $DOCKER_IMAGE
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline succeeded'
        }
        failure {
            echo '❌ Pipeline failed'
        }
    }
}

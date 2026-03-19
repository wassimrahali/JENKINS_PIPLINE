pipeline {
    agent {
        docker {
            image 'node:18'
            args '-u root:root' // ensure permission for Docker commands if needed
        }
    }

    environment {
        DOCKER_IMAGE = "wassimrahali/backend-8guess"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/wassimrahali/JENKINS_PIPLINE.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            agent any // Use a node with Docker installed
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Container (Deploy)') {
            agent any // Use a node with Docker installed
            steps {
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

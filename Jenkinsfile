pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "wassimrahali/backend-8guess"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/wassimrahali/JENKINS_PIPELINE.git'
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
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Container (Deploy)') {
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

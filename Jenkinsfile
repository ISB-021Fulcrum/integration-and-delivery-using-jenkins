pipeline {
    agent any
    environment {
        PORT = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
        IMAGE_TAG = "${env.BRANCH_NAME == 'main' ? 'nodemain:v1.0' : 'nodedev:v1.0'}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: env.BRANCH_NAME, url: 'your-repo-url'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_TAG} ."
            }
        }
        stage('Deploy') {
            steps {
                sh "docker stop ${IMAGE_TAG} || true"
                sh "docker rm ${IMAGE_TAG} || true"
                sh "docker run -d --expose ${PORT} -p ${PORT}:${PORT} ${IMAGE_TAG}"
            }
        }
    }
}

pipeline {
    agent any

    tools {
        nodejs 'node' // Ensure this is configured under Global Tool Configuration
    }

    environment {
        IMAGE_NAME = 'nodedev'
        IMAGE_TAG = 'v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/xtraovv/epam-jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build' // Adjust if your app doesn't need a build step
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
        stage('Run docker image'){
            steps {
                script {
                    docker.Image.run("-p 3001:3000")
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and Docker image creation succeeded.'
        }
        failure {
            echo '❌ Build failed.'
        }
    }
}
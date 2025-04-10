pipeline {
    agent any

    tools {
        git 'mygit'             // Ensure this is defined in Global Tool Configuration
        nodejs 'node'         // Node.js tool also needs to be defined globally
    }

    environment {
        IMAGE_NAME = 'nodedev'
        IMAGE_TAG = 'v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit([
                    branches: [[name: '*/dev']],
                    userRemoteConfigs: [[url: 'https://github.com/xtraovv/epam-jenkins.git']]
                ])
            }
        }

        stage('Build application') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Run Docker Image') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${IMAGE_TAG}").run("-p 3001:3000")
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

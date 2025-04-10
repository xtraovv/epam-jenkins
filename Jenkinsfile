pipeline {
    agent any

    tools {
        dockerTool 'docker'
        nodejs 'node'
    }

    environment {
        IMAGE_NAME = 'nodedev:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/xtraovv/epam-jenkins.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def dockerHome = tool 'docker'
                    env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
                sh """
                docker build -t $IMAGE_NAME .
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                docker run -d -p 3001:3000 --name nodedev_container $IMAGE_NAME
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

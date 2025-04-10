pipeline {
    agent any  // Runs on the built-in Jenkins node

    environment {
        IMAGE_NAME = "nodemain"
        IMAGE_TAG = "v1.0"
        FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Tests') {
            steps {
                sh 'chmod +x scripts/test.sh && scripts/test.sh'
            }
        }

        stage('Build Artifacts') {
            steps {
                sh 'chmod +x scripts/build.sh && scripts/build.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${FULL_IMAGE} ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d --name ${IMAGE_NAME}_container ${FULL_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed"
        }
        cleanup {
            script {
                sh "docker rm -f ${IMAGE_NAME}_container || true"
                sh "docker rmi ${FULL_IMAGE} || true"
            }
        }
    }
}
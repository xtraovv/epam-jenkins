pipeline {
    agent any

    tools {
        nodejs 'node' // NodeJS tool as configured in Jenkins
    }

    environment {
        IMAGE_NAME = 'nodemain'
        IMAGE_TAG = 'v1.0'
        FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
        CONTAINER_NAME = "${IMAGE_NAME}_container"
    }

    stages {
        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Install Node Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'chmod +x scripts/test.sh && scripts/test.sh'
            }
        }

        stage('Build App') {
            steps {
                sh 'chmod +x scripts/build.sh && scripts/build.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${FULL_IMAGE}", '.')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh "docker run -d -p 3000:3000 --name ${CONTAINER_NAME} ${FULL_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }

        cleanup {
            script {
                sh "docker rm -f ${CONTAINER_NAME} || true"
                sh "docker rmi ${FULL_IMAGE} || true"
            }
        }
    }
}

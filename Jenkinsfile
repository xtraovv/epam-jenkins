pipeline {
    agent any

    tools {
        dockerTool 'docker'
        nodejs 'node'
    }

    environment {
        IMAGE_NAME = 'nodemain:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/xtraovv/epam-jenkins.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }

        stage('Remove docker image and container') {
            steps {
                script {
                    sh '''
                    # Stop and remove container if exists
                    CONTAINER_ID=$(docker ps -aqf "name=nodemain_container")
                    if [ -n "$CONTAINER_ID" ]; then
                        echo "Stopping container $CONTAINER_ID..."
                        docker stop "$CONTAINER_ID"
                        docker rm "$CONTAINER_ID"
                    else
                        echo "No running container matching 'nodemain' found."
                    fi

                    # Remove image if exists
                    IMAGE_ID=$(docker images -q nodemain)
                    if [ -n "$IMAGE_ID" ]; then
                        echo "Removing image $IMAGE_ID..."
                        docker rmi "$IMAGE_ID"
                    else
                        echo "No image matching 'nodemain' found."
                    fi
                    '''
                }
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
                docker run -d -p 3000:3000 --name nodemain_container $IMAGE_NAME
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

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

                stage('Remove docker image and container')  {
            steps {
                script {
                    sh "docker container stop `docker container ls | grep nodedev | awk '{print $1}'`"
                    sh "docker container rm `docker container ls | grep nodedev | awk '{print $1}'`"
                    sh "docker image rm `docker images | grep nodedev | awk '{print $3}'`"
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

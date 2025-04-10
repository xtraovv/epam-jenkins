pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Running on branch: ${env.BRANCH_NAME}"
                sh 'ls -l'
            }
        }
    }
}

pipeline {
    agent any

    environment {
        // NODE_VERSION is not needed if NodeJS is installed on the agent
        DOCKER_IMAGE = 'ishmumr7/jom-tapau'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '9ebe7d16-982e-4607-99ac-a342a42d85a3', url: 'git@github.com:ishmumr7/jom-tapau.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        // Add other stages like linting, testing, building Docker image, etc.
    }

    post {
        success {
            echo 'Build successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}

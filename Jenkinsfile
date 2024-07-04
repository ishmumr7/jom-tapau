pipeline {
    agent any

    environment {
        NODE_VERSION = '18.12.1'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub repository
                git credentialsId: '9ebe7d16-982e-4607-99ac-a342a42d85a3', url: 'git@github.com:ishmumr7/jom-tapau.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Use Node.js
                tool name: "node-${NODE_VERSION}", type: 'NodeJS'

                // Install npm dependencies
                sh 'npm install'
            }
        }

        stage('Lint') {
            steps {
                // Run linting
                sh 'npm run lint'
            }
        }

        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Build') {
            steps {
                // Build the React application
                sh 'npm run build'
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Archive the build artifacts
                archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo 'Build and tests passed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}

pipeline {
    agent any
    
    environment {
        NODE_VERSION = '18.12.1'
        DOCKER_IMAGE = 'ishmumr7/jom-tapau'
        DOCKER_TAG = 'latest'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git credentialsId: '9ebe7d16-982e-4607-99ac-a342a42d85a3', url: 'https://github.com/ishmumr7/jom-tapau.git', branch: 'main'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                tool name: "node-${NODE_VERSION}", type: 'NodeJS'
                sh 'npm install'
            }
        }
        
        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }
        
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker', url: '']) {
                        // Push Docker image to Docker Hub
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }
        
        stage('Clean Up') {
            steps {
                script {
                    // Remove Docker image locally to free up space
                    sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Build, tests, and Docker image push succeeded!'
        }
        failure {
            echo 'Build, tests, or Docker image push failed.'
        }
    }
}

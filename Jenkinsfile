pipeline {
    agent any

    environment {
        NODE_VERSION = '18.12.1'
        DOCKER_IMAGE = 'ishmumr7/jom-tapau'
        DOCKER_TAG = 'latest'
        SSH_PRIVATE_KEY = credentials('9ebe7d16-982e-4607-99ac-a342a42d85a3')
    }

    stages {
        stage('Checkout') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: '9ebe7d16-982e-4607-99ac-a342a42d85a3', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        mkdir -p ~/.ssh
                        echo "$SSH_KEY" > ~/.ssh/id_ed25519
                        chmod 600 ~/.ssh/id_ed25519
                        ssh -o StrictHostKeyChecking=no git@github.com || true
                    '''
                }
                git url: 'git@github.com:ishmumr7/jom-tapau.git', credentialsId: '9ebe7d16-982e-4607-99ac-a342a42d85a3'
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
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker', url: '']) {
                        sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
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

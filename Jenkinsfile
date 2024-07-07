pipeline {
    agent any
    tools {
        nodejs 'node' // Ensure this matches the name configured in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ishmumr7/jom-tapau']]])
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Build React App') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t ishmumr7/jom-tapau .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker', variable: 'dockerhubpwd')]) {
                        bat 'docker login -u ishmumr7 -p %dockerhubpwd%'
                    }
                    bat 'docker push ishmumr7/jom-tapau'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Make sure kubectl is installed and configured in the Jenkins environment
                    bat 'kubectl apply -f deploymentservice.yaml --kubeconfig=%k8sconfig%'
                }
            }
        }
    }
}

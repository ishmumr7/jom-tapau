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
                sh 'npm install'
            }
        }
        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ishmumr7/jom-tapau .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u ishmumr7 -p ${dockerhubpwd}'
                    }
                    sh 'docker push ishmumr7/jom-tapau'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfig'
                }
            }
        }
    }
}

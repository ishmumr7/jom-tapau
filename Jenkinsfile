pipeline {
    agent any
    tools {
        nodejs 'node' 
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: '9ebe7d16-982e-4607-99ac-a342a42d85a3', url: 'https://github.com/ishmumr7/jom-tapau.git']])
            }
        }
        stage('Build') {
            steps {
                bat 'npm install --force'
            }
        }
        stage('Build') {
            steps {
                bat 'npm install --force'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    bat 'docker build -t ishmumr7/jom-tapau .'
                }
            }
        }
        stage('Push Image to Hub') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        bat 'docker tag jom-tapau-image/demo:latest ishmumr7/jom-tapau:latest'
                        bat 'docker push ishmumr7/jom-tapau:latest'
                    }
                }
            }
        }

        // stage('Install Dependencies') {
        //     steps {
        //         sh 'npm install'
        //     }
        // }
        // stage('Build React App') {
        //     steps {
        //         sh 'npm run build'
        //     }
        // }
        // stage('Build Docker Image') {
        //     steps {
        //         script {
        //             sh 'docker build -t ishmumr7/jom-tapau .'
        //         }
        //     }
        // }
        // stage('Push Docker Image') {
        //     steps {
        //         script {
        //             withCredentials([string(credentialsId: 'docker', variable: 'dockerhubpwd')]) {
        //                 sh 'docker login -u ishmumr7 -p ${dockerhubpwd}'
        //             }
        //             sh 'docker push ishmumr7/jom-tapau'
        //         }
        //     }
        // }
        // stage('Deploy to Kubernetes') {
        //     steps {
        //         script {
        //             kubernetesDeploy configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfig'
        //         }
        //     }
        // }
    }
}

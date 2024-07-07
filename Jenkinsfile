pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t ishmumr7/jom-tapau .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push <your-dockerhub-username>/react-devops-project'
                }
            }
        }
    }
}
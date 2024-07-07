pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t <your-dockerhub-username>/react-devops-project .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'Username', passwordVariable: 'Password')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push <your-dockerhub-username>/react-devops-project'
                }
            }
        }
    }
}
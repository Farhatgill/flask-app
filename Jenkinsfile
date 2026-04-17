pipeline {
    agent any

    stages {
        stage('Code') {
            steps {
                git branch: 'main',  url: 'https://github.com/yo-its-anas/flask-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t flask-app-cicd:latest .'
            }
        }

        stage('Push to Dockerhub') {
            steps {
                withCredentials([usernamePassword (
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                 )]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}" 
                    sh "docker image tag ${env.dockerHubUser}/flask-app-cicd:latest"
                    sh "docker push ${env.dockerHubUser}/flask-app-cicd:latest"
                 }
            }
        }
        stage('Docker Compose Build') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}

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
        stage('Push to Docker hub'){
            steps{
                withCredentials([usernamePassword (
                    credentialsId: 'dockerhubCreds',
                    usernamevariable: 'dockerHubuser',
                    passwordvariable: 'dockerHubPass'
                )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}" 
                    sh "docker tag flask-app-cicd:latest ${env.dockerHubUser}/flask-app-cicd:latest"
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

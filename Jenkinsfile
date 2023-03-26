pipeline {
    agent any
    tools {
        go 'golang'
    }
    environment {
        CGO_ENABLED = 0
    }
    stages {
        stage('Preparation') {
            steps {
                git url: "https://github.com/febriyanadji/jenkins-golang", branch: 'main'
            }
        }
        stage('unit-test') {
            steps {
                sh 'go mod download'
                sh 'go test -v'
            }
        }
        stage('build-docker-image') {
            steps {
                script {
                    sh 'docker build -t febriyanadji/http-golang-app:latest .'
                }
            }
        }
        stage('push-docker-image') {
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u febriyanadji -p ${dockerhubpwd}'
                   }
                   sh 'docker push febriyanadji/http-golang-app:latest'
                }
            }
        }
    }
}
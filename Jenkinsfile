pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                echo 'Repository Cloned'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t gov-hospital:v1 .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker stop gov-hospital-container || true'
                sh 'docker rm gov-hospital-container || true'
                sh 'docker run -d -p 80:80 --name gov-hospital-container gov-hospital:v1'
            }
        }
    }
}
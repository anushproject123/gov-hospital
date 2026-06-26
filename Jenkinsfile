pipeline {
    agent any

    environment {
        IMAGE_NAME = "anushcez/gov-hospital"
    }

    stages {

        stage('Build Image') {
            steps {
                sh 'docker build --no-cache -t $IMAGE_NAME:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Kubernetes') {
            steps {
                sh '''
                kubectl set image deployment/gov-hospital gov-hospital=$IMAGE_NAME:latest
                kubectl rollout restart deployment/gov-hospital
                kubectl rollout status deployment/gov-hospital
                '''
            }
        }
    }
}
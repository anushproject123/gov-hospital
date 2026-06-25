pipeline {
agent any


environment {
    IMAGE_NAME = "anushcez/gov-hospital"
}

stages {

    stage('Build Image') {
        steps {
            sh 'docker build -t $IMAGE_NAME:v5 .'
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
            sh 'docker push $IMAGE_NAME:v5'
        }
    }
}


}


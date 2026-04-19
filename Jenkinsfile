pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        DOCKER_HUB_USER = "poorans"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: 'https://github.com/urk23cs1165/exp5'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d --name myapp -p 5000:5000 $DOCKER_HUB_USER/$IMAGE_NAME:latest
                '''
            }
        }
    }
}
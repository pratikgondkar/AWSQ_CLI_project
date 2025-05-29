pipeline {
    agent any

    environment {
        IMAGE_NAME = 'amazon-q-cli'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Create Docker Container') {
            steps {
                sh 'docker create --name my-container ${IMAGE_NAME}'
            }
        }

        stage('List Docker Images and Containers') {
            steps {
                sh 'docker images'
                sh 'docker ps -a'
              }
         }

    }
}

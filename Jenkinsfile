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

        stage('Run Container') {
            steps {
                sh 'docker run --rm ${IMAGE_NAME} echo "Container started successfully"'
            }
        }
    }
}

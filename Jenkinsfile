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

        stage('Run Container with Bash') {
            steps {
                sh 'docker run -it --entrypoint /bin/bash ${IMAGE_NAME}'
            }
        }
    }
}

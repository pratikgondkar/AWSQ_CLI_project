pipeline {
    agent any

    environment {
        IMAGE_NAME = 'amazon-q-cli'
        CONTAINER_NAME = 'my-container'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def imageExists = sh(
                        script: "docker images -q ${IMAGE_NAME} || true",
                        returnStdout: true
                    ).trim()

                    if (imageExists) {
                        echo "Docker image '${IMAGE_NAME}' already exists. Skipping build."
                    } else {
                        sh "docker build -t ${IMAGE_NAME} ."
                    }
                }
            }
        }

        stage('Create Docker Container') {
            steps {
                script {
                    def containerExists = sh(
                        script: "docker ps -a --format '{{.Names}}' | grep -w ${CONTAINER_NAME} || true",
                        returnStdout: true
                    ).trim()

                    if (containerExists) {
                        echo "Container '${CONTAINER_NAME}' already exists. Skipping creation."
                    } else {
                        sh "docker create -p 8000:8000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                    }
                }
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

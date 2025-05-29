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
                        script: "docker images -q ${IMAGE_NAME}",
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

        stage('Create and Start Docker Container') {
            steps {
                script {
                    def containerExists = sh(
                        script: "docker ps -a --format '{{.Names}}' | grep -w ${CONTAINER_NAME} || true",
                        returnStdout: true
                    ).trim()

                    if (containerExists) {
                        echo "Container '${CONTAINER_NAME}' already exists. Skipping creation."

                        // Check if container is running
                        def isRunning = sh(
                            script: "docker ps --format '{{.Names}}' | grep -w ${CONTAINER_NAME} || true",
                            returnStdout: true
                        ).trim()

                        if (!isRunning) {
                            sh "docker start ${CONTAINER_NAME}"
                            echo "Started existing container '${CONTAINER_NAME}'."
                        } else {
                            echo "Container '${CONTAINER_NAME}' is already running."
                        }
                    } else {
                        sh "docker run -d -p 8000:8000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                        echo "Created and started new container '${CONTAINER_NAME}'."
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

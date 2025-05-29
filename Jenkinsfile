pipeline {
    agent any

    environment {
        IMAGE_NAME = 'amazon-q-cli'
        CONTAINER_NAME = 'amazon-q-cli-container'
    }

    stages {
        stage('Check Existing Docker Image') {
            steps {
                script {
                    def imageExists = sh(
                        script: "docker images -q ${IMAGE_NAME}",
                        returnStdout: true
                    ).trim()

                    if (imageExists) {
                        echo "✅ Docker image '${IMAGE_NAME}' already exists. Skipping build."
                    } else {
                        echo "🔨 Docker image not found. Building now..."
                        sh "docker build -t ${IMAGE_NAME} ."
                    }
                }
            }
        }

        stage('Check and Run Container') {
            steps {
                script {
                    def containerRunning = sh(
                        script: "docker ps --filter 'name=${CONTAINER_NAME}' --format '{{.Names}}'",
                        returnStdout: true
                    ).trim()

                    if (containerRunning == CONTAINER_NAME) {
                        echo "✅ Container '${CONTAINER_NAME}' is already running. Skipping run."
                    } else {
                        echo "🚀 Starting new container '${CONTAINER_NAME}'..."
                        sh """
                            docker run -d --name ${CONTAINER_NAME} \\
                            --entrypoint /bin/bash ${IMAGE_NAME}
                        """
                        sleep 2
                        def running = sh(
                            script: "docker ps --filter 'name=${CONTAINER_NAME}' --format '{{.Names}}'",
                            returnStdout: true
                        ).trim()
                        if (running == CONTAINER_NAME) {
                            echo "🎉 Container '${CONTAINER_NAME}' is running successfully!"
                        } else {
                            error("❌ Container failed to start.")
                        }
                    }
                }
            }
        }
    }
}

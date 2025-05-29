pipeline {
    agent any

    environment {
        IMAGE_NAME = 'amazon-q-cli'
        CONTAINER_NAME = 'amazon-q-cli-container'
    }

    stages {
        stage('Build Image If Needed') {
            steps {
                script {
                    def image = sh(script: "docker images -q ${IMAGE_NAME}", returnStdout: true).trim()
                    if (!image) {
                        echo "🔨 Image not found. Building..."
                        sh "docker build -t ${IMAGE_NAME} ."
                    } else {
                        echo "✅ Image '${IMAGE_NAME}' already exists."
                    }
                }
            }
        }

        stage('Run Container If Not Running') {
            steps {
                script {
                    def isRunning = sh(script: "docker ps --filter 'name=^/${CONTAINER_NAME}$' --format '{{.Names}}'", returnStdout: true).trim()
                    if (!isRunning) {
                        echo "🚀 Starting container '${CONTAINER_NAME}'..."
                        sh """
                            docker run -d --name ${CONTAINER_NAME} \\
                            -p 8000:8000 \\
                            --entrypoint /bin/bash ${IMAGE_NAME}
                        """
                        sleep 2
                        def running = sh(script: "docker ps --filter 'name=^/${CONTAINER_NAME}$' --format '{{.Names}}'", returnStdout: true).trim()
                        if (running == CONTAINER_NAME) {
                            echo "🎉 Container '${CONTAINER_NAME}' is running successfully!"
                        } else {
                            error "❌ Failed to start container."
                        }
                    } else {
                        echo "✅ Container '${CONTAINER_NAME}' is already running."
                    }
                }
            }
        }
    }
}

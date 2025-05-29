pipeline {
    agent any

    environment {
        IMAGE_NAME = 'amazon-q-cli'
        CONTAINER_NAME = 'amazon-q-cli-container'
    }

    stages {
        stage('Build Image If Not Exists') {
            steps {
                script {
                    if (!sh(script: "docker images -q ${IMAGE_NAME}", returnStdout: true).trim()) {
                        echo "🔨 Image not found. Building..."
                        sh "docker build -t ${IMAGE_NAME} ."
                    } else {
                        echo "✅ Image '${IMAGE_NAME}' already exists."
                    }
                }
            }
        }

        stage('Create Container If Not Exists') {
            steps {
                script {
                    if (!sh(script: "docker ps -a --filter 'name=^/${CONTAINER_NAME}$' --format '{{.Names}}'", returnStdout: true).trim()) {
                        echo "📦 Creating container '${CONTAINER_NAME}'..."
                        sh "docker create --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                    } else {
                        echo "✅ Container '${CONTAINER_NAME}' already exists."
                    }

                    def created = sh(
                        script: "docker ps -a --filter 'name=^/${CONTAINER_NAME}$' --format '{{.Names}}'",
                        returnStdout: true
                    ).trim()

                    if (created == CONTAINER_NAME) {
                        echo "🎉 Container '${CONTAINER_NAME}' has been created successfully!"
                    } else {
                        error("❌ Container creation failed.")
                    }
                }
            }
        }
    }
}

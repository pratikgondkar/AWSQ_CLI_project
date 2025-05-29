stage('Create and Start Docker Container') {
    steps {
        script {
            def containerExists = sh(
                script: "docker ps -a --format '{{.Names}}' | grep -w ${CONTAINER_NAME} || true",
                returnStdout: true
            ).trim()

            if (containerExists) {
                echo "Container '${CONTAINER_NAME}' already exists. Skipping creation."
                // Start the container if it is not running
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
            }
        }
    }
}

pipeline {
    agent any

    environment {
        IMAGE_NAME = 'awsq-cli'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Run Amazon Q CLI') {
            steps {
                script {
                    sh 'rm -f output.*'
                    sh '''
                    docker run --rm -v ${WORKSPACE}:/workspace awsq-cli amazon-q diagram /workspace/diagram.mcp
                    '''
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'output.*', fingerprint: true
            }
        }
    }
}

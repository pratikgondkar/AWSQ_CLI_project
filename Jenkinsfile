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
                    docker run --rm -v $PWD:/workspace ${IMAGE_NAME} \
                        q /workspace/diagram.mcp
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

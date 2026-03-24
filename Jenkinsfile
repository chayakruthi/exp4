pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp-image"
    }

    stages {
        stage('Checkout') {
            steps {
                // This pulls your code from GitHub/GitLab
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Builds the image using the Dockerfile we fixed earlier
                    sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Example: Run a simple check to see if the image exists
                    sh "docker images | grep ${IMAGE_NAME}"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Stop old container if it exists, then run the new one
                    sh "docker stop my-flask-app || true"
                    sh "docker rm my-flask-app || true"
                    sh "docker run -d -p 5000:5000 --name my-flask-app ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
}

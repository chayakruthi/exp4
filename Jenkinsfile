pipeline {
    agent any

    environment {
        // Define your image name and internal container name here
        IMAGE_NAME = "myapp-image"
        CONTAINER_NAME = "flask-app-container"
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls the code from your GitHub repository
                checkout scm
            }
        }

        stage('Prepare Environment') {
            steps {
                script {
                    // This removes any old container with the same name so the new one can start
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Builds the image using the Dockerfile in your root directory
                    // Using BUILD_NUMBER ensures every build has a unique version tag
                    sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                    sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Runs the newly built image on port 5000
                    sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Verify') {
            steps {
                script {
                    // Checks if the container is actually running
                    sh "docker ps | grep ${CONTAINER_NAME}"
                    echo "Application is running at http://localhost:5000"
                }
            }
        }
    }

    post {
        success {
            echo "Successfully deployed build #${env.BUILD_NUMBER}"
        }
        failure {
            echo "Build #${env.BUILD_NUMBER} failed. Check the logs above."
        }
    }
}

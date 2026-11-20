
pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "anil-demo-app"

        // Python installation path
        PYTHON_PATH = "C:/Users/STUDENT/AppData/Local/Programs/Python/Python314/python.exe"

        // Docker installation path
        DOCKER_PATH = "C:/Users/STUDENT/AppData/Local/Programs/DockerDesktop/resources/bin/docker.exe"
    }

    stages {

        stage('1. Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('2. Application Build & Test') {
            steps {
                echo "Checking Python application syntax..."

                bat '"%PYTHON_PATH%" -m py_compile app.py'

                echo "Python syntax check completed successfully!"
            }
        }

        stage('3. Build & Tag Docker Image') {
            steps {
                echo "Building Docker image..."

                bat '''
                    "%DOCKER_PATH%" build ^
                    -t %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER% ^
                    -t %REGISTRY%/%IMAGE_NAME%:latest .
                '''

                echo "Docker image built successfully!"
            }
        }

        stage('4. Push Image to Registry') {
            steps {
                echo "Pushing Docker image to registry..."

                bat '"%DOCKER_PATH%" push %REGISTRY%/%IMAGE_NAME%:%BUILD_NUMBER%'

                bat '"%DOCKER_PATH%" push %REGISTRY%/%IMAGE_NAME%:latest'

                echo "Docker images pushed successfully!"
            }
        }
    }

    post {
        success {
            echo "======================================"
            echo "PIPELINE EXECUTED SUCCESSFULLY!"
            echo "Image: ${REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}"
            echo "======================================"
        }

        failure {
            echo "PIPELINE FAILED! Check Console Output."
        }
    }
}

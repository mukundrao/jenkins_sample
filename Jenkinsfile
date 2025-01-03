pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'flask-app3:latest'
        DOCKER_CONTAINER = 'flask3-container'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Stop any running container before starting a new one
                    sh 'docker rm -f $DOCKER_CONTAINER || true'
                    // Run the Docker container
                    sh 'docker run -d -p 5005:5005 --name $DOCKER_CONTAINER $DOCKER_IMAGE'
                }
            }
        }
    }
    post {
        always {
            // Clean up any stopped containers
            sh 'docker rm -f $DOCKER_CONTAINER || true'
        }
    }
}

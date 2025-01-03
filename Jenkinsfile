pipeline {
    agent {
        docker { 
            image 'docker:20.10.24' // Use a specific Docker CLI image
            args '--privileged' // Ensure Docker-in-Docker is allowed
        }
    }
    environment {
        // Define environment variables
        DOCKER_IMAGE = 'flask_image'
        DOCKER_CONTAINER = 'flask_container'
    }
    stages {
        stage('Checkout') {
            steps {
                // Check out code from your repository
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image from the Dockerfile in the workspace
                    sh '''
                    docker build -t $DOCKER_IMAGE .
                    '''
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh '''
                    docker run -d --name $DOCKER_CONTAINER -p 5005:5005 $DOCKER_IMAGE
                    '''
                }
            }
        }
    }
    post {
        always {
            script {
                // Clean up the Docker container and image
                sh '''
                docker stop $DOCKER_CONTAINER || true
                docker rm $DOCKER_CONTAINER || true
                docker rmi $DOCKER_IMAGE || true
                '''
            }
        }
    }
}

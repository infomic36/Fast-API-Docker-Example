pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'yourusername/fastapi-app'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image for FastAPI application
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run the FastAPI container and execute tests inside it
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'pytest tests/'
                    }
                }
            }
        }

        stage('Push to Docker Registry') {
            when {
                branch 'main' // Only push when on the main branch
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed. Check logs for details.'
        }
    }
}

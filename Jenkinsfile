pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'hello-world-app'
        CONTAINER_NAME = 'hello-world-app-container'
        PORT = '5000'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/por314159ug/app-python-1.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Stop Existing Container (if any)') {
            steps {
                script {
                    // Attempt to stop and remove the container if it exists
                    sh """
                        docker ps -q --filter "name=${CONTAINER_NAME}" | grep -q . && docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME} || echo "No existing container to stop"
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${env.DOCKER_IMAGE}:latest").run("-d --name ${CONTAINER_NAME} -p ${PORT}:${PORT}")
                }
            }
        }
    }
}

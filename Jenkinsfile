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
                git url: 'https://github.com/por314159ug/v0-python-application.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE)
                }
            }
        }

        stage('Cleanup Existing Container') {
            steps {
                script {
                    // Stop and remove any existing container using port 5000
                    powershell '''
                        $containers = docker ps -q --filter "publish=5000"
                        if ($containers) {
                            docker stop $containers
                            docker rm $containers
                        }
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${env.DOCKER_IMAGE}:latest").run("-p ${env.PORT}:${env.PORT} -d")
                }
            }
        }
    }
}

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

        stage('Stop Existing Container Using Port 5000') {
            steps {
                script {
                    bat """
                        @echo off
                        echo Checking for any container using port %PORT%...

                        :: List all running containers and filter by port
                        docker ps --filter "publish=%PORT%" --format "{{.ID}}" > containers_using_port.txt

                        :: If the file has any containers, stop and remove them
                        for /f "tokens=*" %%i in (containers_using_port.txt) do (
                            echo Stopping and removing container %%i
                            docker stop %%i
                            docker rm %%i
                        )

                        :: Clean up the temporary file
                        del containers_using_port.txt
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${env.DOCKER_IMAGE}:latest").run("--name ${env.CONTAINER_NAME} -p ${env.PORT}:${env.PORT} -d")
                }
            }
        }
    }
}

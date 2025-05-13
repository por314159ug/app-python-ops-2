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

                        :: Check for containers using port 5000
                        for /f "tokens=*" %%i in ('docker ps -q --filter "publish=%PORT%"') do (
                            echo Stopping container %%i using port %PORT%
                            docker stop %%i
                            docker rm %%i
                        )
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

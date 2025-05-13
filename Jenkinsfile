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

        stage('Stop Existing Container or Process Using Port 5000') {
            steps {
                script {
                    bat """
                        @echo off
                        echo Checking for any process or container using port %PORT%...

                        :: Check if any container is using port 5000
                        FOR /F "tokens=*" %%i IN ('docker ps -q --filter "publish=%PORT%"') DO (
                            echo Stopping container %%i using port %PORT%
                            docker stop %%i
                            docker rm %%i
                        )

                        :: Check if port 5000 is occupied by a process (netstat and taskkill)
                        FOR /F "tokens=5" %%i IN ('netstat -ano ^| findstr :%PORT%') DO (
                            echo Killing process %%i using port %PORT%
                            taskkill /PID %%i /F
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

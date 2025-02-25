pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hardikverma3044/mr-react-app"
        CONTAINER_NAME = "goofy_leaderberg"
        DEPLOY_PORT = "3000"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/erHardikVerma/Dockerized-React-App-with-Nginx.git'
            }
        }

        stage('Install Dependencies & Build') {
            steps {
                bat '''
                call npm install
                call npm run build
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

        stage('Run Nginx Server') {
            steps {
                bat '''
                docker ps -q --filter "name=%CONTAINER_NAME%" | findstr . >nul && docker stop %CONTAINER_NAME% || echo "Container not running"
                docker ps -a -q --filter "name=%CONTAINER_NAME%" | findstr . >nul && docker rm %CONTAINER_NAME% || echo "Container not found"
                docker run -d -p %DEPLOY_PORT%:80 --name %CONTAINER_NAME% %DOCKER_IMAGE%
                '''
            }
        }
    }
}

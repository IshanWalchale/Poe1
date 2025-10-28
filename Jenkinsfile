pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "poe1-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/IshanWalchale/Poe1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                echo Building Docker image: %DOCKER_IMAGE%
                docker build -t %DOCKER_IMAGE% .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                    echo Logging in to Docker Hub...
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    echo Tagging image...
                    docker tag %DOCKER_IMAGE% %DOCKER_USER%/%DOCKER_IMAGE%:latest
                    echo Pushing image to Docker Hub...
                    docker push %DOCKER_USER%/%DOCKER_IMAGE%:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and push successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}

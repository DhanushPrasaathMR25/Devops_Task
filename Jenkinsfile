pipeline {
    agent any

    environment {
        IMAGE_NAME = "dhanushprasaathmr07/my-app"
        REGISTRY = "docker.io"
        DOCKER_CREDENTIALS_ID = "Docker"
        GITHUB_CREDENTIALS_ID = "Github"
        APP_DIR = "." // Changed APP_DIR to current directory
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: GITHUB_CREDENTIALS_ID, url: 'https://github.com/DhanushPrasaathMR25/Devops_Task.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:latest ." // Removed 'cd $APP_DIR &&'
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                script {
                    sh "docker-compose down && docker-compose up -d" // Removed 'cd $APP_DIR &&'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully! '
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKER_CRED = credentials('DockerHubCredentials')
    }

    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'npm run test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                bat 'docker build -t gandonov2/student-registry-app:${BUILD_NUMBER} .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Logging in to Docker Hub...'
                bat 'echo $DOCKER_CRED_PASS | docker login -u $DOCKER_CRED_USER --password-stdin'

                echo 'Pushing Docker image with build tag...'
                bat 'docker push gandonov2/student-registry-app:${BUILD_NUMBER}'

                echo 'Tagging Docker image as latest...'
                bat 'docker tag gandonov2/student-registry-app:${BUILD_NUMBER} gandonov2/student-registry-app:latest'

                echo 'Pushing Docker image with latest tag...'
                bat 'docker push gandonov2/student-registry-app:latest'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

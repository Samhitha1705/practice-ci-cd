pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-cred-id' // Jenkins stored credentials
        DOCKER_IMAGE = "vedasamhitha17/flask-ci-cd-demo"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Samhitha1705/practice-ci-cd.git'
            }
        }

        stage('Install Dependencies & Test') {
            steps {
                bat 'python -m pip install --upgrade pip'
                bat 'pip install -r requirements.txt'
                bat 'python -m pytest'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        bat 'docker build -t %DOCKER_USER%/flask-ci-cd-demo:%BUILD_NUMBER% .'
                        bat 'docker tag %DOCKER_USER%/flask-ci-cd-demo:%BUILD_NUMBER% %DOCKER_USER%/flask-ci-cd-demo:latest'
                        bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                        bat 'docker push %DOCKER_USER%/flask-ci-cd-demo:%BUILD_NUMBER%'
                        bat 'docker push %DOCKER_USER%/flask-ci-cd-demo:latest'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build, Test, and Docker Push Successful!'
        }
        failure {
            echo 'Build Failed! Check logs.....'
        }
    }
}

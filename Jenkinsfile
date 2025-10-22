pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-cred-id'
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
                bat 'pytest'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        def app = docker.build("${DOCKER_IMAGE}:${BUILD_NUMBER}")
                        app.push()
                        app.push("latest")
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
            echo 'Build Failed! Check logs.'
        }
    }
}

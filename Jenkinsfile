pipeline {
    agent {
        kubernetes {
            cloud 'kubernetes-production'
            inheritFrom 'ci-v2'
        }
    }

    environment {
        PYTHON_VERSION = '3.8'
        VENV = '/tmp/venv'
        DOCKER_REGISTRY = 'mamer2'
        DOCKER_IMAGE_NAME = 'simple-python'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/MostafaAmer-2/simple-python.git'
            }
        }

        stage('Install Python') {
            steps {
                script {
                    sh "apk add --no-cache python3 py3-pip"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "${VENV}/bin/pip install -r requirements.txt"
            }
        }

        stage('Run Tests') {
            steps {
                sh "${VENV}/bin/python -m pytest"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}", '.')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.image("${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                echo 'Deploying to test environment...'
                // Add deployment steps if needed
            }
        }

        stage('Deploy to Production') {
            when {
                expression { env.BRANCH_NAME == 'main' }
            }
            steps {
                echo 'Deploying to production...'
                // Add production deployment steps if needed
            }
        }
    }
}

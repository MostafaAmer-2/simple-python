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
    }

    stages {
        stage('Checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/MostafaAmer-2/simple-python.git'
            }
        }

        stage('Setup Python') {
            steps {
                sh "pyenv local ${PYTHON_VERSION}"
                sh "python -m venv ${VENV}"
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

        stage('Deploy to Test Environment') {
            steps {
                echo 'Deploying to test environment...'
            }
        }

        stage('Deploy to Production') {
            when {
                expression { env.BRANCH_NAME == 'master' }
            }
            steps {
                echo 'Deploying to production...'
            }
        }
    }


}

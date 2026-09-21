pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    python -m pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    . .venv/bin/activate
                    mkdir -p test-results
                    pytest -v --junitxml=test-results/results.xml
                '''
            }

            post {
                always {
                    junit(
                        testResults: 'test-results/results.xml',
                        allowEmptyResults: true
                    )
                }
            }
        }

        stage('Build') {
            steps {
                sh '''
                    . .venv/bin/activate
                    python -m compileall src
                '''
            }
        }
    }

    post {
        success {
            echo 'AI/DS BUILD SUCCESSFUL'
        }

        failure {
            echo 'AI/DS BUILD FAILED'
        }
    }
}

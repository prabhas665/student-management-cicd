
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                    @echo off
                    "C:\\Program Files\\Python313\\python.exe" -m venv --clear .venv || exit /b 1
                    ".venv\\Scripts\\python.exe" -m pip install -r requirements.txt || exit /b 1
                '''
            }
        }

        stage('Test') {
            steps {
                bat '".venv\\Scripts\\python.exe" -m pytest -v'
            }
        }

        stage('Run Application') {
            steps {
                bat '".venv\\Scripts\\python.exe" app.py'
            }
        }

        stage('Package Release') {
            steps {
                bat '''
                    @echo off
                    if not exist release mkdir release
                    copy /Y app.py release\\app.py || exit /b 1
                    copy /Y requirements.txt release\\requirements.txt || exit /b 1
                '''
                archiveArtifacts artifacts: 'release/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: All stages completed.'
        }
        failure {
            echo 'FAILED: Check Console Output.'
        }
    }
}

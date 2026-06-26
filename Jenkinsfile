pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Beyo04/test_django.git'
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                pip install pip-audit
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                python manage.py test
                '''
            }
        }

        stage('Security Scan') {
            steps {
                sh '''
                . venv/bin/activate
                pip-audit || true
                '''
            }
        }

        stage('Django Security Check') {
            steps {
                sh '''
                . venv/bin/activate
                python manage.py check --deploy
                '''
            }
        }
    }
}

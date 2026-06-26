pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/Beyo04/test_django_pro.git', branch: 'main'
            }
        }

        stage('Setup Dependencies') {
            steps {
                sh 'python3 -m pip install --upgrade pip'
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Django Tests') {
            steps {
                sh 'python manage.py test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t django-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop django-app-container || true
                docker rm django-app-container || true
                docker run -d -p 8000:8000 --name django-app-container django-app
                '''
            }
        }
    }
}

pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-app"
        CONTAINER_NAME = "django"
    }

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
                pip install pip-audit
                pip-audit || true
                '''
            }
        }

        stage('Django Deploy Check') {
            steps {
                sh '''
                . venv/bin/activate
                python manage.py check --deploy || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$BUILD_NUMBER .
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker run -d -p 8000:8000 --name $CONTAINER_NAME $IMAGE_NAME:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        success {
            echo "Django app deployed successfully 🚀"
        }
        failure {
            echo "Build failed ❌ check logs"
        }
    }
}

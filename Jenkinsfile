pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-app"
        CONTAINER_NAME = "django-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Beyo04/test_django_pro.git'
            }
        }

        stage('Create Virtual Environment & Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Django Tests') {
            steps {
                sh '''
                . venv/bin/activate
                python manage.py test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d -p 8000:8000 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful"
        }
        failure {
            echo "❌ Build Failed - Check Logs"
        }
    }
}

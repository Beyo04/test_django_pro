pipeline {
    agent any

    environment {
        IMAGE_NAME = "django-app"
        DOCKERHUB_REPO = "YOUR_DOCKERHUB_USERNAME/django-app"
        CONTAINER_NAME = "django-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Beyo04/test_django_pro.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Tag Image for Docker Hub') {
            steps {
                sh 'docker tag $IMAGE_NAME $DOCKERHUB_REPO:latest'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $DOCKERHUB_REPO:latest'
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
                sh 'docker run -d -p 8000:8000 --name $CONTAINER_NAME $DOCKERHUB_REPO:latest'
            }
        }
    }

    post {
        success {
            echo "✅ Build, Push & Deployment Successful"
        }
        failure {
            echo "❌ Pipeline Failed - Check Logs"
        }
    }
}

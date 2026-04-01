pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "trendapp"
        TAG = "latest"
        DOCKER_HUB = "your-dockerhub-username"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/devopsmuthuraman/Trendapp.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB/$IMAGE_NAME:$TAG .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
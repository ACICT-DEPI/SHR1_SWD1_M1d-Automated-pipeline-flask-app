pipeline {
    agent any

    environment {
        DOCKER_USERNAME = 'afnandior'
        DOCKER_PASSWORD = 'docker_1234'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Docker Login') {
            steps {
                script {
                    sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    docker build -t flask-app .
                    '''
                }
            }
        }

        stage('Push Docker Image to Registry') {
            steps {
                script {
                    sh '''
                    docker tag flask-app afnandior/flask-app:latest
                    docker push afnandior/flask-app:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                    minikube kubectl -- config use-context minikube
                    minikube kubectl -- apply -f K8S/deployment.yml
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh '''
                    minikube kubectl -- get pods
                    '''
                }
            }
        }

        stage('Push to Team Repository') {


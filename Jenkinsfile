pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    // Clone the team repository
                    sh '''
                    git remote remove team-repo || true
                    git remote add team-repo https://github.com/codefresh-contrib/python-flask-sample-app.git
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

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
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
                    kubectl apply -f K8S/deployment.yml
                    kubectl apply -f K8S/service.yml
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh '''
                    kubectl rollout status deployment/flask-app -n python-flask-app
                    '''
                }
            }
        }

        stage('Push to Team Repository') {
            steps {
                script {
                    sh '''
                    git remote remove team-repo || true
                    git remote add team-repo https://github.com/AbdullahElmasry/DevOps_engineer_track_project_DEPI.git

                    git config --global user.name "AbdullahElmasry"
                    git config --global user.email "611afnanmohamed@gmail.com"
                    git add .

                    # Check if there are changes to commit
                    if [ "$(git diff --cached --quiet; echo $?)" -ne 0 ]; then
                        git commit -m "Automated commit from Jenkins" || true
                        git push team-repo main
                    else
                        echo "لا توجد تغييرات لارتكابها"
                    fi
                    '''
                }
            }
        }
    }
}



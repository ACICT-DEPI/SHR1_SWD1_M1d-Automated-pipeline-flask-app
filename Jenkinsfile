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

        stage('Run Docker Container') {
            steps {
                script {
                    sh '''
                    docker run -d -p 5000:5000 flask-app
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

                    // Check if there are changes to commit
                    if (sh(script: 'git diff --cached --quiet', returnStatus: true) != 0) {
                        sh 'git commit -m "Automated commit from Jenkins"'
                        sh 'git push team-repo main'
                    } else {
                        echo "لا توجد تغييرات لارتكابها"
                    }
                    '''
                }
            }
        }
    }
}



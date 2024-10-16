pipeline {
    agent any
    environment {
        APP_NAME = "flask-app" 
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/codefresh-contrib/python-flask-sample-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("${APP_NAME}")
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    app.run('-p 5000:5000')
                }
            }
        }
        stage('Push to Team Repository') {
            steps {
                script {
                    // تحقق مما إذا كان المستودع البعيد موجودًا، إذا كان موجودًا قم بإزالته
                    sh '''
                    if git remote get-url team-repo &> /dev/null; then
                        git remote remove team-repo
                    fi
                    git remote add team-repo https://github.com/AbdullahElmasry/DevOps_engineer_track_project_DEPI.git
                    '''
                    
                    // دفع التغييرات إلى الـ remote repository
                    sh 'git push team-repo master' // تأكد من أنك في الفرع الصحيح
                }
            }
        }
    }
}

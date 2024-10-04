pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/codefresh-contrib/python-flask-sample-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def app = docker.build("flask-app")
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
                    // إضافة الـ remote repository
                    sh 'git remote add team-repo https://github.com/AbdullahElmasry/DevOps_engineer_track_project_DEPI.git'
                    // دفع التغييرات إلى الـ remote repository
                    sh 'git push team-repo master' // تأكد من أنك في الفرع الصحيح
                }
            }
        }
    }
}

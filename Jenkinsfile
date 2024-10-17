pipeline {
    agent any
    environment {
        APP_NAME = "flask-app"
        GITHUB_USER_EMAIL = "611afnanmohamed@gmail.com"  // أدخل هنا البريد الإلكتروني للفريق 
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: '1bd8b51f-64f2-4b96-925f-d2eeb534cf8f', url: 'https://github.com/codefresh-contrib/python-flask-sample-app.git'
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
                    
                    // إعداد بيانات الاعتماد
                    withCredentials([usernamePassword(credentialsId: '1bd8b51f-64f2-4b96-925f-d2eeb534cf8f', usernameVariable: 'GITHUB_USER', passwordVariable: 'GITHUB_PASS')]) {
                        sh '''
                        git config --global user.name "$GITHUB_USER"
                        git config --global user.email "$GITHUB_USER_EMAIL"
                        git add .  # تأكد من إضافة جميع التغييرات

                        // تحقق من وجود تغييرات لإجراء commit
                        if ! git diff --cached --quiet; then
                            git commit -m "Automated commit from Jenkins" || true
                            git push team-repo master
                        else
                            echo "لا توجد تغييرات لارتكابها"
                        fi
                        '''
                    }
                }
            }
        }
    }
}





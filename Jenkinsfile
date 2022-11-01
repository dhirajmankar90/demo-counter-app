pipeline {
    agent any

    stages {
        stage('Git Checkout stage') {
            steps {
              git 'https://github.com/dhirajmankar90/demo-counter-app.git'
            }
        }
    }

    post
    {
        always
        {
            emailext attachLog: true, body: 'Your pipeline build was successful. ', subject: 'Pipeline Status Report', to: 'dhirajmankar1210@gmail.com'
        }
    }
}
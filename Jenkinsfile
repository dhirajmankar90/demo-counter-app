pipeline {
    agent any

    stages {
        stage('Git Checkout stage') {
            steps {
              git 'https://github.com/dhirajmankar90/demo-counter-app.git'
            }
        }
        stage('Unit testing') {
            steps {
              bat 'mvn test'
            }
        }
        stage('Integration testing') {
            steps {
              bat 'mvn verify -DskipUnitTests'
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
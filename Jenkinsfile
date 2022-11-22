pipeline {
    agent any
    tools {
         maven 
    }

    stages {
        stage('Git Checkout stage') {
            steps {
              git 'https://github.com/dhirajmankar90/demo-counter-app.git'
            }
        }
        stage('Unit testing') {
            steps {
              sh 'mvn test'
            }
        }
}

}
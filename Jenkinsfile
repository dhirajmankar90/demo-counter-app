pipeline {
    agent any
    tools {
       maven "maven-3.8.6"
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
        stage('Integration testing') {
            steps {
              sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('maven Build') {
            steps {
              sh 'mvn clean install'
            }
        }
        stage ('Static code analysis') {
        steps {
            withSonarQubeEnv(credentialsId: 'sonar-api-key') {
            sh 'mvn clean package sonar:sonar'
            }
        }
        }

    }
}
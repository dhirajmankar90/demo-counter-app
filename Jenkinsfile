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
            script {
                 withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                 sh 'mvn clean package sonar:sonar'
            }
            }
        }
        }
        stage ('Quality gate status'){
            steps {
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                }
            }
        }
        stage ('Upload jar file to nexus'){
            steps{
                script{
                    def readPomVersion = readMavenPom file: 'pom.xml'
                    def nexusRepo = readPomVersion.version.endsWith ("SNAPSHOT") ? "demoapp-snapshot":"demoapp-release"
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: '3.110.176.7:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo, 
                    version: "${readPomVersion.version}"
                }
            }
        }
        stage ('Build Docker image'){
            steps {
                script{
                    sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID dhirajmankar90/$JOB_NAME:v1.$BUILD_ID'
                    sh 'docker image tag $JOB_NAME:v1.$BUILD_ID dhirajmankar90/$JOB_NAME:latest'
                }
            }
        }
        stage ('push to dockerhub') {
        steps {
            script {
                 withCredentials([string(credentialsId: variable: 'dockerHub_Auth')]) {
                 sh 'docker login -u dhirajmankar90 -p ${dockerHub_Auth}'
                 sh 'docker image push dhirajmankar90/$JOB_NAME:v1.$BUILD_ID'     
                 sh 'docker image push dhirajmankar90/$JOB_NAME:latest'      
                }
                }
            }
        }

    }
}
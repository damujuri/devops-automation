pipeline {
    agent any
    tools{
        maven 'Maven3'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/damujuri/devops-automation']]])
                bat 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    bat 'docker build -t damujuri/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   // withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                   //bat 'docker login -u damujuri -p ${dockerhubpwd}'
                    bat 'docker login -u damujuri -p Sarojini9'
                    //}
                    bat 'docker push damujuri/devops-integration'
               }
            }
        }
    }
}
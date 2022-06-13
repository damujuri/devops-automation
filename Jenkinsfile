pipeline {
    agent any
    tools{
        maven 'Maven3.6.1'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/damujuri/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t damujuri/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'Sarojini9', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u damujuri -p ${dockerhubpwd}'

}
                   sh 'docker push damujuri/devops-integration'
                }
            }
        }
    }
}
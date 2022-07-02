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
        
        stage('SonarQube analysis') {
        //def scannerHome = tool 'SonarScanner 4.0';
            steps{
                withSonarQubeEnv('sonarqube') {
                // If you have configured more than one global server connection, you can specify its name
                // sh "${scannerHome}/bin/sonar-scanner"
                    bat 'mvn sonar:sonar'
                    //bat 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
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
        stage('Push image to K8s'){
            steps{
                script{
                    //kubernetesDeploy configs: 'deploymentservice.yaml', kubeConfig: [path: ''], kubeconfigId: 'k8configpwd', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
                    kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'k8configpwd')
                    }
                }
            }
    }
}

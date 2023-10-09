pipeline {
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Goodluck101/devops-automation2.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                 sh 'docker build -t goodluck101/devops-integration .'
                }
            }
        }
                stage('Push Image to Hub'){
            steps{
                script{
                 withCredentials([string(credentialsId: 'dockerhbpwd', variable: 'dockerhbpwd')]) {
                 sh "docker login -u goodluck101 -p \"${dockerhbpwd}\""
}
                 sh 'docker push goodluck101/devops-integration'
                }
            }
        }
                 stage('Deploy to k8') {

            steps {

                script {

                    kubernetesDeploy (configs: 'deploymentservice.yaml', kubeconfigId: 'kubeconpwd')
                    //kubernetesDeploy configs: '', kubeConfig: [path: ''], kubeconfigId: 'kubeconfpwd', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']

                }

            }
        }
    }
}

pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node14'
    }

    environment {
        SCANNER_HOME =  tool 'sonar-scanner'
        GITHUB_TOKEN = credentials('GITHUB_TOKEN')
        CHART_NAME = "inf-project"
        CHART_VERSION = "0.1.0"
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://158.160.64.78:8081/repository/inf-helm/"
        NEXUS_REPOSITORY = "inf-helm"
        NEXUS_CREDENTIAL_ID = credentials('NEXUS_CREDENTIAL_ID')
        NEXUS_USERNAME = "admin"
        NEXUS_PASSWORD = "123"
    }



    stages {
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
    
       
        stage("Checkout from SCM Helm Chart"){
            steps {
                echo 'Checkout from SCM Helm Chart..'
                git branch: 'main', credentialsId: 'jenkins-github', url: 'https://github.com/jakkaru-devops/inf-helm.git'
            }
        }
  

        stage('Configure Git') {
            steps {
                sh 'git config --global user.email "savamedvedevvv@gmail.com"'
                sh 'git config --global user.name "jakkaru-devops"'
                sh 'git config --list --show-origin'
                sh 'git config --global push.autoSetupRemote true'
            }
        }


       stage('Build helm chart') {
            steps {
                sh 'echo "Current directory: $(pwd)"'
                sh 'helm package .'
                withCredentials([usernamePassword(credentialsId: 'NEXUS_CREDENTIAL_ID', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASS')]) {
                    sh "curl -u ${NEXUS_USER}:${NEXUS_PASS} http://158.160.64.78:8081/repository/inf-helm/ --upload-file my-chart-1.0.0.tgz"
                }
            }
        }

        

       
            
        


    }
}

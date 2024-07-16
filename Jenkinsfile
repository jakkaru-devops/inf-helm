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
        NEXUS_URL = "158.160.64.78:8081"
        NEXUS_REPOSITORY = "inf-helm"
        NEXUS_CREDENTIAL_ID = credentials('NEXUS_CREDENTIAL_ID')
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
            }
        }

        

        stage('Deploy To Nexus Repository Helm Chart') {
            steps {
            //    withCredentials([usernamePassword(credentialsId: 'NEXUS_CREDENTIAL_ID', usernameVariable: 'admin', passwordVariable: '123')]) {
            //         sh "curl -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} -X PUT --upload-file ./target/${CHART_NAME}-${CHART_VERSION}.tgz ${NEXUS_URL}${CHART_NAME}/${CHART_VERSION}/${CHART_NAME}-${CHART_VERSION}.tgz"
            //     }
                curl "-u "admin":"123" `http://158.160.64.78:8081/repository/inf-helm/` --upload-file "$CHART_NAME-$CHART_VERSION""

            }
        }


    }
}

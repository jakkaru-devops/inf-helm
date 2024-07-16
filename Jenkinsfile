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
        CHART_VERSION = "${BUILD_NUMBER}"
        NEXUS_URL = "http://158.160.64.78:8081/repository/inf-helm"
        CR_REGISTRY = "cr.yandex/crpn9ikb6hp5v19o9957"
        CR_REPOSITORY = "inf-frontend-dev"
        IMAGE_NAME = "${CR_REGISTRY}" + "/" + "${CR_REPOSITORY}"
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


       stage('Build helm chart repo version frontend ') {
            steps {
                sh 'echo "Current directory: $(pwd)"'
                sh 'ls -la'
            }
        }

        

        stage('Deploy To Nexus Repository Helm Chart') {
            steps {
                sh "curl -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} -X PUT --upload-file ./target/$CHART_NAME-$CHART_VERSION.tgz $NEXUS_URL$CHART_NAME/$CHART_VERSION/$CHART_NAME-$CHART_VERSION.tgz"
            }
        }


    }
}

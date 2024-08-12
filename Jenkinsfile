pipeline {
  agent any
    environment {
      EMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      IMAGE_NAME = 'my-app-image:latest'
    }
  stages {
    stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker.build${IMAGE_NAME}'
                }
            }
        }
    stage ('Semgrep') {
      steps {
        sh 'pip3 install semgrep'
      }
    }
    stage('Install Trivy') {
            steps {
                script {
                    // Установка Trivy 
                    sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/scripts/install.sh | sh -s -- -b /usr/local/bin'
                }
            }
        }
    stage ('CommitSemgrep') {
      steps {
        sh 'semgrep scan'
      }
    }
    stage ('CommitTrivy') {
      steps {       
        sh "trivy image ${IMAGE_NAME}"
      }
    }
  }
}

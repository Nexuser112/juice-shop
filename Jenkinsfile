pipeline {
  agent any
    environment {
      EMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      IMAGE_NAME = 'my-app-image:latest'
    }
  stages {
    stage ('Install Semgrep') {
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
    stage('Install Syft') {
            steps {
                script {
                    // Установка Syft
                    sh 'curl -sfL https://raw.githubusercontent.com/anchore/syft/main/install.sh | sh -s -- -b ${SYFT_PATH}'
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
    stage ('BuildSyft') {
      steps {       
        sh "trivy image ${IMAGE_NAME}"
      }
    }
    stage ('BuildGrype') {
      steps {       
        sh "trivy image ${IMAGE_NAME}"
      }
    }
  }
}

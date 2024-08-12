pipeline {
  agent any
    environment {
      EMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      IMAGE_NAME = 'my-app-image:latest'
      SYFT_PATH = "${env.WORKSPACE}/syft"
      GRYPE_PATH = "${env.WORKSPACE}/grype"
      TRIVY_PATH = "${env.WORKSPACE}/trivy"
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
                    sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/scripts/install.sh | sh -s -- -b ${TRIVY_PATH}'
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
    stage('Install Grype') {
            steps {
                script {
                    // Установка Grype 
                    sh 'curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh -s -- -b ${GRYPE_PATH}'
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

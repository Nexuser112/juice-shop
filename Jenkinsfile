pipeline {
  agent any
    environment {
      EMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
      IMAGE_NAME = 'Dockerfile'
      SYFT_PATH = "${env.WORKSPACE}/syft"
      GRYPE_PATH = "${env.WORKSPACE}/grype"
      TRIVY_PATH = "${env.WORKSPACE}/trivy"
      TRIVY_REPORT_FS = 'trivy-report-fs.json'
      SYFT_REPORT_FS = 'syft-report-fs.json'
      GRYPE_REPORT_FS = 'grype-report-fs.json'
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
        sh 'trivy fs --format json --output ${TRIVY_REPORT_FS} ${TRIVY_PATH}'
      }
    }
    stage ('BuildSyft') {
      steps {       
        sh '${SYFT_PATH}/syft dir:${SYFT_PATH} -o json > ${SYFT_REPORT_FS}'
      }
    }
    stage ('BuildGrype') {
      steps {       
        sh '${GRYPE_PATH}/grype dir:${GRYPE_PATH} -o json > ${GRYPE_REPORT_FS}'
      }
    }
  }
}

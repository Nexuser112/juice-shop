pipeline {
  agent any
    environment {
      SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
    }
  stages {
    stage ('Semgrep') {
      steps {
         sh 'pip3 install semgrep'
      }
    }
    stage ('Trivy') {
      steps {
        // Install trivy
                sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.18.3'
                sh 'curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl'
      }
    }
    stage ('Pre-commit') {
      steps {
        sh 'semgrep ci'
      }
    }
    stage ('Commit') {
      steps {
        sh 'semgrep ci'
      }
    }
    stage ('Build') {
      steps {
        sh 'semgrep ci'
      }
    }
    stage ('Test') {
      steps {
                // Scan all vuln levels
                sh 'mkdir -p reports'
                sh 'trivy filesystem --ignore-unfixed --vuln-type os,library --format template --template "@html.tpl" -o reports/nodjs-scan.html ./nodejs'
                publishHTML target : [
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'nodjs-scan.html',
                    reportName: 'Trivy Scan',
                    reportTitles: 'Trivy Scan'
                ]

                // Scan again and fail on CRITICAL vulns
                sh 'trivy filesystem --ignore-unfixed --vuln-type os,library --exit-code 1 --severity CRITICAL ./nodejs'
      }
    }
    stage ('Deploy') {
      steps {
        sh 'semgrep ci'
      }
    } 
    stage ('Post-Deploy') {
      steps {
        sh 'semgrep ci'
      }
    }
  }
}


pipeline {
  agent any
    environment {
      EMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
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
        sh -s 'apt-get install trivy'
      }
    }
    stage ('CommitSemgrep') {
      steps {
        sh 'semgrep scan'
      }
    }
    stage ('CommitTrivy') {
      steps {       
        //Scan all vuln levels
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
  }
}

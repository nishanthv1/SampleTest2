pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('SEMGREP_APP_TOKEN')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"

      //  SEMGREP_TIMEOUT = "300"
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/Nithink-Hexaware/Sample-Test.git'
            }
            }
        
      stage('Semgrep-Scan') {
          steps {
            sh "pip3 install semgrep"
            sh "semgrep --config=auto /"
          }
      }
}
}

pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('secret_key')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"

      //  SEMGREP_TIMEOUT = "300"
    }

     stages {
        stage('Build') {
            steps {
                git 'https://github.com/nishanthv-hexa/SampleTEs1.git'
            }
            }
        stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML ', odcInstallation: 'Owasp dependency Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
      stage('Semgrep-Scan') {
          steps {
            sh "pip3 install semgrep"
            sh "semgrep ci"
          }
      }

  stage('SonarQube Analysis') {
            steps {
                script{
    def scannerHome = tool 'Sonarscanner'; 
                withSonarQubeEnv('Sonarqube') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
      }
            }
  }
        
         
//         stage('PMD'){
//             steps{
//             sh 'pmd -d -f txt -r rulesets/java/quickstart.xml -reportfile report.txt'}}
//         stage ("bandit"){
//            steps {
//                 sh 'bandit -r /var/lib/jenkins/workspace/SampleTest1 -f json -o /home/azureuser/archerysec-cli//banditResult.json'
//             }}
//          stage ("NPM Audit"){
//              steps{
//              sh 'npm audit'
//              }}
     }
}
 

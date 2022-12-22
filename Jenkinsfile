pipeline {
    agent any
  environment {
      // SEMGREP_BASELINE_REF = ""

        SEMGREP_APP_TOKEN = credentials('secret_key')
        SEMGREP_PR_ID = "${env.CHANGE_ID}"
        DEFECTDOJO_TOKEN= credentials('defectdojo_token')

      //  SEMGREP_TIMEOUT = "300"
    }

     stages {
        stage('Build') {
            steps {
                git 'https://github.com/nishanthv-hexa/SampleTEs1.git'
            }
            }
//         stage ('OWASP Dependency-Check Vulnerabilities') {
//             steps {
//                 dependencyCheck additionalArguments: '--format HTML --format XML ', odcInstallation: 'Owasp dependency Check'
//                 dependencyCheckPublisher pattern: 'dependency-check-report.xml'
//             }
//         }
//       stage('Semgrep-Scan') {
//           steps {
//             sh "pip3 install semgrep"
//             sh "semgrep ci"
//           }
//       }

//   stage('SonarQube Analysis') {
//             steps {
//                 script{
//     def scannerHome = tool 'Sonarscanner'; 
//                 withSonarQubeEnv('Sonarqube') {
//       sh "${scannerHome}/bin/sonar-scanner"
//     }
//       }
//             } 
//   }
         stage('DefectDojo'){
             steps{
        sh '''curl -k -X 'POST' \\
  'https://10.20.1.16:8443/api/v2/reimport-scan/' \\
  -H 'accept: application/json' \\
  -H 'Authorization: Token 00858f456b56bb4c16f7be481e3e5cd5ed4b5aaa' \\
  -H 'Content-Type: multipart/form-data' \\
  -F 'test=2' \\
  -F 'file=@banditResult.json;type=application/json' \\
  -F 'scan_type=Bandit Scan' \\
  -F 'tags=SampleB1' '''
             }
                          steps{
        sh '''curl -k -X 'POST' \\
  'https://10.20.1.16:8443/api/v2/reimport-scan/' \\
  -H 'accept: application/json' \\
  -H 'Authorization: Token 00858f456b56bb4c16f7be481e3e5cd5ed4b5aaa' \\
  -H 'Content-Type: multipart/form-data' \\
  -F 'test=1' \\
  -F 'file=@dependency-check-report.xml;type=application/json' \\
  -F 'scan_type=Dependency Check Scan' \\
  -F 'tags=SampleD1' '''
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
 

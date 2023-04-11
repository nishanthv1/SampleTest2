pipeline {
    agent any
  environment {
       SEMGREP_BASELINE_REF = ""
       SEMGREP_APP_TOKEN = credentials('secret_key')
       SEMGREP_PR_ID = "${env.CHANGE_ID}"


      //  SEMGREP_TIMEOUT = "300"
    }

     stages {
        stage('Build') {
            steps {
                git 'https://github.com/nishanthv-hexa/SampleTest2.git'
            }
            }
        stage ('OWASP Dependency-Check Vulnerabilities') {
            steps {
                dependencyCheck additionalArguments: '--format HTML --format XML ', odcInstallation: 'Dependency check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
                   stage ("NPM Audit"){
              steps{
              sh 'npm audit || true'
             }}
 
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
                withSonarQubeEnv('Sonarscanner') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
      }
            } 
  }
         stage ("bandit"){
            steps {
                 sh 'bandit -r /var/lib/jenkins/workspace/SampleTest2 -f json -o /var/lib/jenkins/workspace/SampleTest2/bandiresult.json'
             }}

         
           stage('ZAP'){
               steps {
              sh 'docker run -v /home/hexa:/zap/wrk/:rw owasp/zap2docker-stable zap-baseline.py -m 1 -t http://testphp.vulnweb.com/index.php -n Vulnweb.context -u test -x vulnweb2.xml || true'
             sh 'sudo cp /home/hexa/vulnweb2.xml /var/lib/jenkins/workspace/SampleTest2/ || true'
             }
         }
         stage('Trivy'){
             steps{
                 sh 'trivy image nginx -f json -o trivyreport.json '
             }
         }
         stage('DefectDojo'){
             steps{
        sh '''curl -k -X 'POST' \\
  'http://127.0.0.1:8080/api/v2/reimport-scan/' \\
  -H 'accept: application/json' \\
  -H 'Authorization: Token 4150cfe81cd330819c0ae7e18456f4b94b7e8d7a' \\
  -H 'Content-Type: multipart/form-data' \\
  -F 'test=1' \\
  -F 'file=@bandiresult.json;type=application/json' \\
  -F 'scan_type=Bandit Scan' \\
  -F 'tags=SampleB1' '''
                 
                         sh '''curl -k -X 'POST' \\
  'http://127.0.0.1:8080/api/v2/reimport-scan/' \\
  -H 'accept: application/json' \\
  -H 'Authorization: Token 4150cfe81cd330819c0ae7e18456f4b94b7e8d7a' \\
  -H 'Content-Type: multipart/form-data' \\
  -F 'test=2' \\
  -F 'file=@dependency-check-report.xml;type=application/json' \\
  -F 'scan_type=Dependency Check Scan' \\
  -F 'tags=SampleD1' '''
                 
                 sh '''curl -k -X 'POST' \\
  'http://127.0.0.1:8080/api/v2/reimport-scan/' \\
  -H 'accept: application/json' \\
  -H 'Authorization: Token 4150cfe81cd330819c0ae7e18456f4b94b7e8d7a' \\
  -H 'Content-Type: multipart/form-data' \\
  -F 'test=3' \\
  -F 'file=@trivyreport.json;type=application/json' \\
  -F 'scan_type=Trivy Scan' \\
  -F 'tags=SampleT1' '''
                 
//                  sh '''curl -k -X 'POST' \\
//   'http://127.0.0.1:42003/api/v2/reimport-scan/' \\
//   -H 'accept: application/json' \\
//   -H 'Authorization: Token becfdf6ea0a24a5c36a906e87947c074db74bbbb' \\
//   -H 'Content-Type: multipart/form-data' \\
//   -F 'test=5' \\
//   -F 'file=@test.html;type=application/json' \\
//   -F 'scan_type=Wapiti Scan' \\
//   -F 'tags=SampleT1' '''
                 
                                  sh '''curl -k -X 'POST' \\
  'http://127.0.0.1:42003/api/v2/reimport-scan/' \\
  -H 'accept: application/json' \\
  -H 'Authorization: Token  4150cfe81cd330819c0ae7e18456f4b94b7e8d7' \\
  -H 'Content-Type: multipart/form-data' \\
  -F 'test=4' \\
  -F 'file=@vulnweb2.xml;type=application/json' \\
  -F 'scan_type=ZAP Scan' \\
  -F 'tags=SampleT1' '''
             }
         }
        
         
//         stage('PMD'){
//             steps{
//             sh 'pmd -d -f txt -r rulesets/java/quickstart.xml -reportfile report.txt'}}
     }
}
 

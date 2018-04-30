pipeline {
  agent any 

  stages {
   stage ('build') {
     steps{
     sh 'ant -f build.xml -v'
     }
    } 
  }
  post {
    always{
      archiveArtifact artifacts: '/dist/*.jar', fingerprint: true 
  }
 }
}

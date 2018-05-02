pipeline {
  agent none 
  
  stages {
   
    stage ('UnitTest') {
     agent {
       label 'linux'
     }
     steps {
       sh 'ant -f test.xml -v'
       junit 'reports/result.xml'
     post {
       success{
         archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
       }
     }
    }

   stage ('build') {
     agent {
       label 'linux'
     } 
     steps{
       sh 'ant -f build.xml -v'
      }
    }

   stage ('Deploy') {
     agent {
       label 'linux'
     }
     steps {
       sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/"
    }
   } 
   
   stage ('Run on Centos') {
     agent{
       label 'centos'
     }
     steps {
       sh " wget http://sunnyf212.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
       sh " java -jar rectangle_${env.BUILD_NUMBER}.jar"  
      }
   }  
}

}

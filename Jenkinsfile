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
       }
     }

   stage ('build') {
     agent {
       label 'linux'
     } 
     steps{
       sh 'ant -f build.xml -v'
    }
     post {
       success{
         archiveArtifacts artifacts: 'dist/*.jar', fingerprint: true
        }
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
       sh " java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"  
      }
   }

   stage ('Run on Debian') {
     agent {
       docker 'openjdk:8u121-jre'
     }
     steps {
       sh " wget http://sunnyf212.mylabserver.com/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar"
       sh " java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
     }
   }
  
  stage ('Move to Prod') {
    agent {
      label 'linux'
    }
    steps {
      sh "cp /var/www/html/rectangles/all/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
    }
  }
}
}

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
       sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"
       sh "cp dist/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
    }
   } 
   
   stage ('Run on Centos') {
     agent{
       label 'centos'
     }
     steps {
       sh " wget http://sunnyf212.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
       sh " java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"  
      }
   }

   stage ('Run on Debian') {
     agent {
       docker 'openjdk:8u121-jre'
     }
     steps {
       sh " wget http://sunnyf212.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar"
       sh " java -jar rectangle_${env.BUILD_NUMBER}.jar 3 4"
     }
   }
  
  stage ('Move to Prod') {
    agent {
      label 'linux'
    }
      when {
        branch 'master'
      } 
      steps {
        sh "cp /var/www/html/rectangles/all/${env.BRANCH_NAME}/rectangle_${env.BUILD_NUMBER}.jar /var/www/html/rectangles/green/rectangle_${env.BUILD_NUMBER}.jar"
    }
  }

  stage('Promote Development Branch to Master') {
      agent {
        label 'linux'
      }
      when {
        branch 'development'
      }
      steps {
        echo "Stashing Any Local Changes"
        sh 'git stash'
        echo "Checking Out Development Branch"
        sh 'git checkout development'
        echo 'Checking Out Master Branch'
        sh 'git pull origin'
        sh 'git checkout master'
        echo 'Merging Development into Master Branch'
        sh 'git merge development'
        echo 'Pushing to Origin Master'
        sh 'git push origin master'
      }
   }
}
}

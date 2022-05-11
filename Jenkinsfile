def va_r
pipeline {
	
   agent any
   stages{
     stage("Version"){
      steps{
        script{
          va_r  = "${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
          echo "${va_r}"
           }
        }
     }
   stage('Build Using Maven') {
      steps {
        script {
	
	         sh "mvn clean package"	 
        }
      }
   }	
}
}

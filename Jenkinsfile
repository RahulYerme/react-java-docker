def str
pipeline {
	tool{
		Apache Maven 3.6.0
          }
   agent any
	
   stages{
     stage("Version"){
      steps{
        script{
          str  = "${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
          echo "${str}"
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

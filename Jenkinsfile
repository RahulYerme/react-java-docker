def va_r
def dockerimg
pipeline {
	
   agent any
    tools{
     maven 'Apache Maven 3.3.9'
    }
   
   
 stages{
  stage("Version"){
   steps{
    script{
         va_r  = "${env.GIT_BRANCH}-${env.BUILD_NUMBER}"
         echo "${va_r}"
    }
   }
  }
 }
}

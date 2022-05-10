def str1
pipeline{
  agent any
  tools{
       maven 'Apache Maven 3.3.9'
  }
  stages{
    stage("creating version of app"){
      steps{
        script{
          str1 = "${ENV.GIT_BRANCH}"---"${ENV.BUILD_NUMBER}"
          echo "${str1}"
        }
      }
    }
    stage("Build Using Maven"){
      steps{
        script{
          sh "mvn clean package"
        }
      }
    }
  }
  
}

def str
def dockerImage
pipeline {
	environment {
    registry = "rahulyerme1234/reactapp"
    registryCredential = 'dockerhub'
    
  }
	tools{
		maven 'Apache Maven 3.6.0'
          }
   agent any
	/*triggers {
	
pollSCM('H/2 * * * *')
}*/
	
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
   stage("Junit Publish Test"){
      steps{
        script{
          junit allowEmptyResults: true, skipPublishingChecks: true, testDataPublishers: [[$class: 'AttachmentPublisher']],
               testResults: '**/target/surefire-reports/TEST-*.xml'
        }
      }
    }
    stage("publishCode Coverage"){
      steps{
        script{
          publishCoverage adapters: [jacocoAdapter('**/target/site/jacoco/jacoco*.xml')], sourceFileResolver: sourceFiles('NEVER_STORE')
          
        }
      }
    }
    
    stage('Local artifact archive') {
      steps {
        script{
	/*sh 'mkdir javaapp'
	sh 'echo "artifact file" > javapp/target/*.jar'*/
       archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
      }
    }
    }  
    stage("Nexus Repository Upload" ){
      steps{
        script{
	input message: 'Enter Username And Password To Continue.', 
	parameters: [credentials(credentialType: 'com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl', 
	defaultValue: 'nexuscredentials', name: '', required: false)]
         nexusArtifactUploader artifacts: [[artifactId: 'users', classifier: '', 
                                            file: 'target/users-1.0.2-SNAPSHOT.war',
                                            type: 'war']], 
                                            credentialsId: 'nexuscredentials', 
                                            groupId: 'com.bbtutorials', 
                                            nexusUrl: '192.168.33.10:8081', 
                                            nexusVersion: 'nexus3', 
                                            protocol: 'http', 
                                            repository: 'reactapp',
                                            version: '1.0.2-SNAPSHOT'
        }
      }
    }  
    stage('build docker image') {
      steps {
        script {
		  
            dockerImage  = docker.build registry + ":$BUILD_NUMBER"
		  
        }
      }
   }
    stage('push docker image') {
      steps {
		script {
			withDockerRegistry(credentialsId: 'dockerhub') {
			  dockerImage.push("${env.BUILD_NUMBER}")
			 dockerImage.push("latest")
			}
		  }
        
      }
    }
    stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    
                    withMaven(maven:'Apache Maven 3.6.0') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
    }   
     stage('Deploy To Tomcat'){
        steps{
		
	   deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '',
			   url: 'http://192.168.33.10:8082/')], 
		             contextPath: 'reactapp', 
		           onFailure: false, war: 'target/*.war' 
	  }
	   }
}
}

def str
pipeline {
	tools{
		maven 'Apache Maven 3.6.0'
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
        archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
      }
    }
    }  
	 stage("Nexus Repository Upload" ){
      steps{
        script{
	input message: 'Enter Username And Password To Continue.', 
	parameters: [credentials(credentialType: 'com.cloudbees.plugins.credentials.impl.UsernamePasswordCredentialsImpl', 
	defaultValue: 'newnexus', name: '', required: false)]
         nexusArtifactUploader artifacts: [[artifactId: 'users', classifier: '', 
                                            file: 'target/users-1.0.2-SNAPSHOT.jar',
                                            type: 'jar']], 
                                            credentialsId: 'newnexus', 
                                            groupId: 'com.bbtutorials', 
                                            nexusUrl: '192.168.33.10:8081', 
                                            nexusVersion: 'nexus3', 
                                            protocol: 'http', 
                                            repository: 'reactapp',
                                            version: '1.0.2-SNAPSHOT'
        }
      }
    }   
}
}
